1. create a protein class
2. create the protein structure


# create a protein class

```java
import java.io.*;
import java.util.*;

public class protein {

    //define protein variables:
    //number of atoms contained in this protein
    String name; //protein's name
    int natm; //number of atoms contained in this protein
    structure struct; //build up the grid structures of this protein


    //Constructor
    public protein(String proName) throws IOException {
        //based on the name, we need to find the file represented by name
        //call readin method to create xyzr variable
        name = proName;
        natm = getNatm(name);
        struct = new structure(name, natm);
    }

    //some sub_methods that created for constructor or other dominant methods
    private static int getNatm(String name) throws FileNotFoundException {
        File f = new File("proteins/" + name + ".xyzr");
        Scanner in = null;
        try {
            in = new Scanner(f);
        } catch (FileNotFoundException e) {
            System.out.println("can't find the file! ");
        }
        int natm = 0;

        for (natm = 0; in.hasNextLine(); natm++) {
            in.nextLine();
        }

        return natm;
    }
}
```

# create the protein structure
```java
import javax.swing.*;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.Scanner;

public class structure {

    String name;
    int natm;
    double dx = 0.5;
    double dy = 0.5;
    double dz = 0.5;
    double dt = 0.001;
    int nx, ny, nz;
    double[] ix, iy, iz;
    double xleft, xright, yleft, yright, zleft, zright;
    double[][] xyzr;
    double[][][] surface;
    double[][][] gset;


    //Constructor
    public structure(String proName, int numAtom) throws IOException {

        name = proName;
        natm = numAtom;
        xyzr = readin(name);
        domainInitial(xyzr);
        surface = new double[nz][ny][nx];
        try {
            mms_surface();
        } catch (IOException e) {
            e.printStackTrace();
        }


    }
	private void mms_surface() throws IOException {
        System.out.println("___________start of mms_____________");
        gset = new double[nz][ny][nx];
        surfaceInitial();
        System.out.println("surface initialized. gset initialized ");
        //pidSolver();

        //This is for drawing the chart
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new timeEnergyChart().setVisible(true);
            }
        });
    }

    private  double[][] readin(String name) throws FileNotFoundException {
        double[][] xyzr = null;
        File f = new File("proteins/" + name + ".xyzr");
        Scanner in = null;
        try {
            in = new Scanner(f);
        } catch (FileNotFoundException e) {
            System.out.println("can't find the file! ");
        }
        String line;
        int rowLength = natm;
        int columnLength = 4;
        xyzr = new double[rowLength][columnLength];
        for (int i = 0; i < rowLength; i++) {
            for (int j = 0; j < columnLength; j++) {
                xyzr[i][j] = in.nextDouble();
            }

        }

        in.close();
        return xyzr;
    }
    private  void domainInitial(double[][] xyzr) {
        double extValue, tmp, tmpMin, tmpMax;
        extValue = 5.5;
        //----------grid domain setting--------------
        //x-grid
        tmpMin = xyzr[0][0] - xyzr[0][3] - extValue;
        tmpMax = xyzr[0][0] + xyzr[0][3] + extValue;
        for (int i = 1; i < natm; i++) {
            tmp = xyzr[i][0] - xyzr[i][3] - extValue;
            if (tmp < tmpMin)
                tmpMin = tmp;
            tmp = xyzr[i][0] + xyzr[i][3] + extValue;
            if (tmp > tmpMax)
                tmpMax = tmp;
        }
        xleft = Math.floor(tmpMin / dx) * dx;
        xright = Math.ceil(tmpMax / dx) * dx;
        //y-grid
        tmpMin = xyzr[0][1] - xyzr[0][3] - extValue;
        tmpMax = xyzr[0][1] + xyzr[0][3] + extValue;
        for (int i = 1; i < natm; i++) {
            tmp = xyzr[i][1] - xyzr[i][3] - extValue;
            if (tmp < tmpMin)
                tmpMin = tmp;
            tmp = xyzr[i][1] + xyzr[i][3] + extValue;
            if (tmp > tmpMax)
                tmpMax = tmp;
        }
        yleft = Math.floor(tmpMin / dy) * dy;
        yright = Math.ceil(tmpMax / dy) * dy;
        //z-grid
        tmpMin = xyzr[0][2] - xyzr[0][3] - extValue;
        tmpMax = xyzr[0][2] + xyzr[0][3] + extValue;
        for (int i = 1; i < natm; i++) {
            tmp = xyzr[i][2] - xyzr[i][3] - extValue;
            if (tmp < tmpMin)
                tmpMin = tmp;
            tmp = xyzr[i][2] + xyzr[i][3] + extValue;
            if (tmp > tmpMax)
                tmpMax = tmp;
        }
        zleft = Math.floor(tmpMin / dz) * dz;
        zright = Math.ceil(tmpMax / dz) * dz;


        //-------------------------------------------
        nx = (int) ((xright - xleft) / dx) + 1;
        ny = (int) ((yright - yleft) / dy) + 1;
        nz = (int) ((zright - zleft) / dz) + 1;

        System.out.println("xleft,xright =" + xleft +","+ xright );
        System.out.println("yleft,yright =" + yleft +","+ yright );
        System.out.println("zleft,zright =" + zleft +","+ zright );
        System.out.println("nx,ny,nz =" + nx +","+ ny + "," + nz );

        ix = new double[nx];
        iy = new double[ny];
        iz = new double[nz];


        for (int i = 0; i < nx; i++)
            ix[i] = xleft + i*dx;
        for (int i = 0; i < ny; i++)
            iy[i] = yleft + i*dy;
        for (int i = 0; i < nz; i++)
            iz[i] = zleft + i*dz;
    }
    private  void surfaceInitial(){
        double scale = 1.3;
        for (int atom = 0; atom < natm ; atom++){
            double r2 = Math.pow(xyzr[atom][3]*scale,2);
            for (int i = (int) Math.floor((xyzr[atom][0] - xyzr[atom][3] * scale - xleft) / dx);
            i < (int) Math.ceil((xyzr[atom][0]+xyzr[atom][3]*scale-xleft)/dx); i++){
                for (int j = (int) Math.floor((xyzr[atom][1] - xyzr[atom][3] * scale - yleft) / dy);
                     j < (int) Math.ceil((xyzr[atom][1]+xyzr[atom][3]*scale-yleft)/dy); j++){
                    for (int k = (int) Math.floor((xyzr[atom][2] - xyzr[atom][3] * scale - zleft) / dz);
                         k < (int) Math.ceil((xyzr[atom][2]+xyzr[atom][3]*scale-zleft)/dz); k++){
                        double distance2 = Math.pow(xyzr[atom][0]-ix[i],2)+Math.pow(xyzr[atom][1]-iy[j],2)+Math.pow(xyzr[atom][2]-iz[k],2);
                        if (distance2 < r2) surface[k][j][i] = 1.0;
                        else surface[k][j][i] = 0.0;
                    }
                }
            }
        }

        //computation will be conducted within solvent accessiable surface
        scale = 1.4;  //this scale is for gset
        for (int atom = 0; atom < natm ; atom++){
            double r2 = Math.pow(xyzr[atom][3]*scale,2);
            for (int i = (int) Math.floor((xyzr[atom][0] - xyzr[atom][3] * scale - xleft) / dx);
                 i < (int) Math.ceil((xyzr[atom][0]+xyzr[atom][3]*scale-xleft)/dx); i++){
                for (int j = (int) Math.floor((xyzr[atom][1] - xyzr[atom][3] * scale - yleft) / dy);
                     j < (int) Math.ceil((xyzr[atom][1]+xyzr[atom][3]*scale-yleft)/dy); j++){
                    for (int k = (int) Math.floor((xyzr[atom][2] - xyzr[atom][3] * scale - zleft) / dz);
                         k < (int) Math.ceil((xyzr[atom][2]+xyzr[atom][3]*scale-zleft)/dz); k++){
                        double distance2 = Math.pow(xyzr[atom][0]-ix[i],2)+Math.pow(xyzr[atom][1]-iy[j],2)+Math.pow(xyzr[atom][2]-iz[k],2);
                        if (distance2 < r2) gset[k][j][i] = 1.0;
                        else gset[k][j][i] = 0.0;
                    }
                }
            }
        }

        scale = 1.0;  //VdW balls are protected, so no computation within VdW.
        for (int atom = 0; atom < natm ; atom++){
            double r2 = Math.pow(xyzr[atom][3]*scale,2);
            for (int i = (int) Math.floor((xyzr[atom][0] - xyzr[atom][3] * scale - xleft) / dx);
                 i < (int) Math.ceil((xyzr[atom][0]+xyzr[atom][3]*scale-xleft)/dx); i++){
                for (int j = (int) Math.floor((xyzr[atom][1] - xyzr[atom][3] * scale - yleft) / dy);
                     j < (int) Math.ceil((xyzr[atom][1]+xyzr[atom][3]*scale-yleft)/dy); j++){
                    for (int k = (int) Math.floor((xyzr[atom][2] - xyzr[atom][3] * scale - zleft) / dz);
                         k < (int) Math.ceil((xyzr[atom][2]+xyzr[atom][3]*scale-zleft)/dz); k++){
                        double distance2 = Math.pow(xyzr[atom][0]-ix[i],2)+Math.pow(xyzr[atom][1]-iy[j],2)+Math.pow(xyzr[atom][2]-iz[k],2);
                        if (distance2 < r2) gset[k][j][i] = 0.0;

                    }
                }
            }
        }


    }
    private  void pidSolver(){
        double kp = 0.075;
        double ki = 0.175;
        double kd = 0.01;
        double eps = Math.pow(10.0, -7);
        double pidTol = Math.pow(2.5, -3);
        double volOld = volumn();
        double vol;
        double[] error = new double[3];


        surfAdi();
        vol = volumn();
        error[0] = Math.max(Math.abs(vol-volOld)/vol, eps);
    }
    private  void surfAdi(){
        double[][][] epsX = new double[nz][ny][nx];
        double[][][] epsY = new double[nz][ny][nx];
        double[][][] epsZ = new double[nz][ny][nx];
        double[][][] surfNew = new double[nz][ny][nx];
        adiEps(epsX,epsY,epsZ);
        //first step

        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++) {
                    surfNew[k][j][i] = surface[k][j][i]+
                            (epsX[k][j][i]*(surface[k][j][i+1]-surface[k][j][i])+
                             epsX[k][j][i-1]*(surface[k][j][i]-surface[k][j][i-1])+
                             epsY[k][j][i]*(surface[k][j+1][i]-surface[k][j][i])+
                             epsY[k][j-1][i]*(surface[k][j][i]-surface[k][j-1][i])+
                             epsZ[k][j][i]*(surface[k+1][j][i]-surface[k][j][i])+
                             epsZ[k-1][j][i]*(surface[k][j][i]-surface[k-1][j][i]))*dt/2.0/Math.pow(dx,2);
                }
            }
        }
        double[] a = new double[nx-2];
        double[] b = new double[nx-2];
        double[] c = new double[nx-2];
        double[] r = new double[nx-2];
        for(int j = 1; j < ny - 1; j++) {
            for (int k = 1; k < nz - 1; k++) {
                for (int i = 1; i < nx - 1; i++) {
                    if (gset[k][j][i] == 0.0) {
                        a[i - 1] = 0.0;
                        b[i - 1] = 1.0;
                        c[i - 1] = 0.0;
                        r[i - 1] = surface[k][j][i];
                    } else {
                        a[i - 1] = -epsX[k][j][i - 1] * dt / 2.0 / Math.pow(dx, 2);
                        b[i - 1] = 1.0 + (epsX[k][j][i - 1] + epsX[k][j][i]) * dt / 2.0 / Math.pow(dx, 2);
                        c[i - 1] = -epsX[k][j][i] * dt / 2.0 / Math.pow(dx, 2);
                        r[i - 1] = surfNew[k][j][i];
                    }
                }
            }
        }
        a = a.copyOfRange(a, 1, a.length);

        //solve this trigonometric system

        //step2
        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++) {
                    surfNew[k][j][i] = surfNew[k][j][i]-
                            (epsY[k][j][i]*(surface[k][j+1][i]-surface[k][j][i])-
                            epsY[k][j-1][i]*(surface[k][j][i]-surface[k][j-1][i]))*dt/2.0/Math.pow(dx,2);
                }
            }
        }
        for(int k = 1; k < nz - 1; k++) {
            for (int i = 1; i < nx - 1; i++){
                for(int j = 1; j < ny - 1; j++){
                    if (gset[k][j][i] == 0.0 ){
                        a[j-1] = 0.0;
                        b[j-1] = 1.0;
                        c[j-1] = 0.0;
                        r[j-1] = surface[k][j][i];
                    }
                    else{
                        a[j-1] = -epsY[k][j-1][i]*dt/2.0/Math.pow(dx,2);
                        b[j-1] = 1.0+(epsY[k][j-1][i]+epsY[k][j][i])*dt/2.0/Math.pow(dx,2);
                        c[j-1] = -epsY[k][j][i]*dt/2.0/Math.pow(dx,2);
                        r[j-1] = surfNew[k][j][i];
                    }

                }
            }
        }

        //solve this trigonometric system

        //step3
        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++) {
                    surfNew[k][j][i] = surfNew[k][j][i]-
                            (epsZ[k][j][i]*(surface[k+1][j][i]-surface[k][j][i])-
                                    epsZ[k-1][j][i]*(surface[k][j][i]-surface[k-1][j][i]))*dt/2.0/Math.pow(dx,2);
                }
            }
        }

        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++) {
                    if (gset[k][j][i] == 0.0 ){
                        a[k-1] = 0.0;
                        b[k-1] = 1.0;
                        c[k-1] = 0.0;
                        r[k-1] = surface[k][j][i];
                    }
                    else{
                        a[k-1] = -epsZ[k-1][j][i]*dt/2.0/Math.pow(dx,2);
                        b[k-1] = 1.0+(epsZ[k-1][j][i]+epsZ[k][j][i])*dt/2.0/Math.pow(dx,2);
                        c[k-1] = -epsZ[k][j][i]*dt/2.0/Math.pow(dx,2);
                        r[k-1] = surfNew[k][j][i];
                    }

                }
            }
        }

        //Solve for this trigonometric system
        //update surface
    }
    private void adiEps(double[][][] epsX, double[][][] epsY, double[][][] epsZ){
        double eps = Math.pow(10.0,-7);
        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++){
                    double dxright = (surface[k][j][i+1]-surface[k][j][i])/dx;
                    double dyhalf = (-surface[k][j-1][i]+surface[k][j+1][i]-surface[k][j-1][i+1]+surface[k][j+1][i+1])/4.0/dx;
                    double dzhalf = (-surface[k-1][j][i]+surface[k+1][j][i]-surface[k-1][j][i+1]+surface[k+1][j][i+1])/4.0/dx;
                    epsX[k][j][i] = 1.0/Math.sqrt(eps + Math.pow(dxright,2)+ Math.pow(dyhalf,2) + Math.pow(dzhalf,2));
                }
            }
        }
        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++){
                    double dyright = (surface[k][j+1][i]-surface[k][j][i])/dx;
                    double dxhalf = (-surface[k][j][i-1]+surface[k][j][i+1]-surface[k][j+1][i-1]+surface[k][j+1][i+1])/4.0/dx;
                    double dzhalf = (-surface[k-1][j][i]+surface[k+1][j][i]-surface[k-1][j+1][i]+surface[k+1][j+1][i])/4.0/dx;
                    epsY[k][j][i] = 1.0/Math.sqrt(eps + Math.pow(dyright,2)+ Math.pow(dxhalf,2) + Math.pow(dzhalf,2));
                }
            }
        }
        for (int i = 1; i < nx - 1; i++){
            for(int j = 1; j < ny - 1; j++){
                for(int k = 1; k < nz - 1; k++){
                    double dzright = (surface[k+1][j][i]-surface[k][j][i])/dx;
                    double dxhalf = (-surface[k][j][i-1]+surface[k][j][i+1]-surface[k+1][j][i-1]+surface[k+1][j][i+1])/4.0/dx;
                    double dyhalf = (-surface[k][j-1][i]+surface[k][j+1][i]-surface[k+1][j-1][i]+surface[k+1][j+1][i])/4.0/dx;
                    epsX[k][j][i] = 1.0/Math.sqrt(eps + Math.pow(dzright,2) + Math.pow(dxhalf,2) + Math.pow(dyhalf,2));
                }
            }
        }
    }
    private double volumn(){
        double vol;
        int sum = 0;
        for(int i = 0; i < nx; i++){
            for (int j = 0; j < ny; j++){
                for (int k = 0; k < nz; k++){
                    if (surface[k][j][i]>0.98) sum = sum + 1;
                }
            }
        }

        vol = sum*Math.pow(dx,3);
        return vol;
    }
}

```
