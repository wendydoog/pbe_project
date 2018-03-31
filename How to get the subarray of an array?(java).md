# How to get the subarray of an array?(java)

#### Method 1:

-----
`copyOfRange` method in `Arrays` class:
```
int[] copyOfRange(int[] original, int from, int to)
```
where int[] can be replaced by any kind of type.

For example:
```
double[] a = {1.0, 2.5, 3.3, 4.8};
double[] aNew = Arrays.copyOfRange(a, 2, 4);
System.out.println(Arrays.toString(aNew));
```
and the result is:
```
[3.3, 4.8]
```

-----
#### method 2:
-----
`arraycopy` method in `system` class:
```java
public static void arraycopy(Object src, 
							int srcPos, 
							Object dest, 
							int destPos, 
							int length)
```
Copies an array from the specified source array, beginning at the specified position, to the specified position of the destination array. A subsequence of array components are copied from the source array referenced by `src` to the destination array referenced by `dest`. The number of components copied is equal to the length argument. The components at positions `srcPos` through `srcPos+length-1` in the source array are copied into positions `destPos` through `destPos+length-1`, respectively, of the destination array.







#### where does the headline problem come from?
-----

This is the question I encountered when I try to solve tridiagonal systems of linear equations.

 `solve` method in `Tridiagonal` class:
```java
solve(double[] d, double[] e, double[] f, double[] b, double[] x) 
```
The command line above is for solving the tridiagonal systems of linear equations. It requires:

- `d` - (input) Vector of diagonal elements. Length N must be >= 2.
- `e` - (input) Vector of super-diagonal elements. Length must be N-1.
- `f` - (input) Vector of sub-diagonal elements. Length must be N-1.
- `b` - (input) Vector of right hand side elements. Length must be N.
- `x` - (output) Solution vector. Length must be N.

It is convenient for me to initialize `d, e, f` together, which `d, e, f` have the same size.

So I have to cut down the first zero and the last zero in this two vectors, where is the headline problem come from.

-----










          
