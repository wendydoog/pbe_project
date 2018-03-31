# How to solving tridiagonal systems of linear equations in java?
-----

#### method:

`solve`method in `Tridiagonal` class:
```
static void	solve(double[] d, 
                  double[] e, 
                  double[] f, 
                  double[] b, 
                  double[] x) 
```
where the parameters are:
- `d` - (input) Vector of `diagonal` elements. Length N must be >= 2.
- `e` - (input) Vector of `super-diagonal` elements. Length must be N-1.
- `f` - (input) Vector of `sub-diagonal` elements. Length must be N-1.
- `b` - (input) Vector of `right hand side` elements. Length must be N.
- `x` - (output) Solution vector. Length must be N.

-----
#### how to use this method in `intellij`?
- first, we have to download the Parallel Java Library `Java Archive (JAR) file` called ` pj20150107.jar`, which contains the Tridiagonal class.

	downloading address is [here](https://www.cs.rit.edu/~ark/pj.shtml#license).
    
- second, drag the jar file into the project folder you are working on, you will see the jar file in the left in `intellij`.
- third, right click this file in `intellij`, choose `add as library`.
- Fourth, we need to add the below command lines into the front of the java file where you want to use the `solve` method.
	```
	import java.lang.Object;
	import edu.rit.numeric.Tridiagonal;
	```
- Last, feel free to use it at wherever you want!
-----
#### where does the headline problem come from?
This method has been used in my PBE project where I have to solve the tridiagonal systems of linear equations in ADI surface constructure.
click [here] for details.

(haven't insert the link yet, need to be revised.)
    
    



