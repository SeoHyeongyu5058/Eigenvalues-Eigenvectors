# EigenValues&Eigenvectors<br>

**개요**<br>
* [qr_decomposition( )]()<br>
* [qr_decomposition( ) Example]()<br>
* [eigenVal( )]()<br>
* [eigenVal( ) Example]()<br>
* [ErrorHandling]()

<hr>

## qr_decomposition( )<br>

```c
void qr_decomposition(const Matrix A, Matrix Q, Matrix R);
```
>Parameter<br>
**A** : System Matrix ( nxn ) <br>
**Q** : Orthogonal Matrix <br>
**R** : Upper triangular Matrix <br>

1. matrix **A**를 **고유값**을 보호하면서 matrix **U**( triangular matri )로 바꿔 주는 함수이다.
2. Householder matrix 방식을 사용한다
3. myMatrix.h 안에 선언되어 있다.
4. matrix A can be decomposed as<br>
$$A = Q R $$
5. 다음과 같은 원리를 기반으로 작성되었다.<br>

Initially, matrix R and Q are set as <br>
$$R^0=A, Q^0=I$$
<br> **k=1** <br>
For step 1, a vector **c** was chosen as 1st column of **R** or **A**

$$V_1=
\begin{equation}
    \begin{bmatrix}
    1\\
    V_{12}\\
    V_{13}\\
    \end{bmatrix}
\end{equation}$$

$$c=R_{i1}^{0}=R(:,1)=A(:,1)=
\begin{equation}
    \begin{bmatrix}
    a_{11}\\
    a_{21}\\
    \vdots\\
    a_{n1}\\
    \end{bmatrix}
\end{equation}$$

Create the household matrix of 

$$H^{(1)}=I-{2\over{V^TV}}VV^T$$

where the vector **v** was chosen as

$$V=\bar{c}+\Vert c \Vert \bar{e_{1}}$$

where

$$e_{1}=
\begin{equation}
   \begin{bmatrix}
   \pm 1\\  
   0\\
   0\\
   \vdots\\
   0\\
   \end{bmatrix}
\end{equation}$$

$$e_{1}=+1 (a_{11}\ge 0)\\
e_{1}=-1 (a_{11}< 0)$$
<br>

Update the next matrix Q and R as
    
$$Q^{(1)}=Q^{(0)}H^{(1)}\\
R^{(1)}=H^{(1)}R^{(0)}$$
<br>

$$R^{(1)}=
\begin{equation}
   \begin{bmatrix}
   r_{11}^{(1)} & r_{12}^{(1)} & \cdots & \cdots\\
   0 & r_{22}^{(1)} & \cdots & \cdots\\
   0 & \vdots & \ddots & \vdots\\
   0 & r_{n2}^{(1)} & \cdots & \ddots\\
   \end{bmatrix}
\end{equation}$$
   
   > Note:<br>
    Q is maintained as the orthogonal matrix<br>
    R is NOT triangular matrix yet
    <br>

**k=2**<br>
A vector c was chosen as the 2nd column of **R1**, but with **c**(1:1)=0

$$ c=
\begin{equation}
   \begin{bmatrix}
   0\\
   r_{22}^{(1)}\\
   \vdots\\
   r_{n2}^{(1)}\\
   \end{bmatrix}
\end{equation}$$

Create the household matrix of 
$$H^{(2)}=I-{2\over{V^TV}}VV^T$$
where the vector **v** was chosen as
$$V=\bar{c}+\Vert c \Vert \bar{e_{2}}$$
where

$$e_{2}=
\begin{equation}
   \begin{bmatrix}
   0\\
   \pm 1\\
   0\\
   \vdots\\
   0\\
   \end{bmatrix}
\end{equation}$$

$$e_{2}=+1 (r_{22}\ge 0)\\
e_{2}=-1 (r_{2}< 0)$$
<br>
Update the next matrix Q and R as
   
$$Q^{(2)}=Q^{(1)}H^{(2)}\\
R^{(2)}=H^{(2)}R^{(1)}$$
<br>

$$R^{(1)}=
\begin{equation}
   \begin{bmatrix}
   r_{11}^{(1)} & r_{12}^{(1)} & \cdots & \cdots\\
   0 & r_{22}^{(1)} & \cdots & \cdots\\
   0 & \vdots & \ddots & \vdots\\
   0 & 0 & \cdots & \ddots\\
   \end{bmatrix}
\end{equation}$$
   
> Note:<br>
  Q2 is maintained as the orthogonal matrix<br>
  R2 is NOT triangular matrix yet<br>

>After (n-1) steps<br>
 **Q** is still orthogonal.<br>
 **R** has become an upper triangular matrix.<br>

<hr>

## Example <br>
```c++
int main()
{
    //path는 행렬 데이터가 저장된 파일
    Matrix A = txtmat(path_A); 

    // Initially R = A, Q = I
    Matrix R = createMat(A.rows, A.cols); 
    copyVal(A, R);
 
    Matrix Q = eye(A.rows, A.cols);

    qr_decomposition(A, Q, R);

    printMat(A, "Matrix A");
    printMat(Q, "Matrix Q");
    printMat(R, "Matrix R");
}
```

## Output <br>
```c
Matirx A =   30.000000     15.000000     20.000000
             15.000000     22.000000     26.000000
             20.000000     26.000000     40.000000

Matrix Q =    -0.768221        0.635542        0.076957
              -0.384111       -0.553759        0.738789
              -0.512148       -0.537994       -0.669528

Matrix R =    -39.051248      -33.289589      -45.837203
                0.000000      -16.637406      -23.206644
               -0.000000       -0.000000       -6.033447
```

## Warning
>A is square matrix<br>
A.rows = Q.rows, A.cols = Q.cols<br>
A.rows = R.rows, A.cols = Q.cols<br>
VTV is not 0

## Error Handling
```c
if (A.rows != A.cols || A.rows != matQ.rows || A.cols != matQ.cols || A.rows != matR.rows || A.cols != matR.cols)
{
	printf("Error: A is not square matrix A /  The sizes of matrix A and matrix Q are not same / The sizes of matrix A and matrix R are not same\n");

	return;
}

if (v_inner == 0.)
{
	printf("Error: v_inner is 0\n");
	
    return;	
}
```

## eigenVal( )<br>

```c
void eigenVal(Matrix A, Matrix val);
```
>Parameter<br>
**A** : System Matrix ( nxn ) <br>
**val** : Return Matrix ( nxn ) <br>

1. qr_decomposition을 이용해 고유값을 반환받는 함수이다.
2. myMatrix.h 안에 선언되어 있다. 
3. qr_decomposition을 반복하여 triangular similar matrix를 얻는다.
4. 다음과 같은 원리를 기반으로 작성되었다.<br>

$$A_1=Q_1R_1$$
$$A_2=Q_1^TA_1Q_1$$
$$A_3=Q_2^TA_2Q_2$$
$$\vdots$$
$$A_i=
\begin{equation}
    \begin{bmatrix}
    \lambda_1 & X & X & X & X\\
    0 & \lambda_2 & X & X & X\\
    0 & 0 & \lambda_3 & X & X\\
    0 & 0 & 0 & \lambda_4 & X\\
      &   &   &  & \ddots \\
    \end{bmatrix}
\end{equation}$$
> matrix A가 상삼각행렬이 될 때까지 qr_decomposition을 반복한다.<br>

## Example <br>
```c++
int main()
{
    //path는 행렬 데이터가 저장된 파일
    Matrix A = txtmat(path_A); 

    //반환값을 저장할 matrix 선언
    Matrix val = createMat(A.rows, A.cols);
    eigVals = eigenVal(A, val);

    printMat(A, "Matrix A");
    printMat(eigVals, "EigenValue");
}
```

## Output <br>
```c
Matrix A =
      30.000000       15.000000       20.000000
      15.000000       22.000000       26.000000
      20.000000       26.000000       40.000000

EigenValue =
      73.015425        0.000000        0.000000
       0.000000       15.522467        0.000000
       0.000000        0.000000        3.462108

```
## Warning
>A is square matrix<br>
A.rows = val.rows, A.cols = val.cols<br>


## Error Handling
```c
if (A.rows != val.rows || A.cols != val.cols || A.rows != A.cols)
{
	printf("Error: A is not square matrix A /  The sizes of matrix A and matrix val are not same\n");

	return zeros(A.rows, A.cols);
}
```
## eigenVector( )<br>

```c
void eigenvec(Matrix A, Matrix val);
```
>Parameter<br>
**A** : System Matrix ( nxn ) <br>
**val** : Return Matrix ( nxn ) <br>

1. myMatrix.h 안에 선언되어 있다. 
2. 다음과 같은 원리를 기반으로 작성되었다.<br>

$$B=(A-\lambda I)(A-\lambda I)V=BV=0$$
If A is 3x3 matrix
eigenvectors are initially set as

$$V_1=
\begin{equation}
    \begin{bmatrix}
    1\\
    V_{12}\\
    V_{13}\\
    \end{bmatrix} V_2=
    \begin{bmatrix}
    V_{22}\\
    1\\
    V_{23}\\
    \end{bmatrix} V_3=
    \begin{bmatrix}
    V_{31}\\
    V_{32}\\
    1\\
    \end{bmatrix}    
\end{equation}$$


For first eigenvector
<br>

$$\begin{equation}
   \begin{bmatrix}
   b_{11} & b_{12} & b_{13}\\
   b_{21} & b_{22} & b_{23}\\
   b_{31} & b_{32} & b_{33}\\
   \end{bmatrix}
   \begin{bmatrix}
   1\\
   V_{12}\\
   V_{13}\\
   \end{bmatrix}=
   \begin{bmatrix}
   0\\
   0\\
   0\\
   \end{bmatrix}
\end{equation}$$


$$\downarrow$$

$$\begin{equation}
   \begin{bmatrix}
   b_{22} & b_{23}\\
   b_{32} & b_{33}\\
   \end{bmatrix}
   \begin{bmatrix}
   V_{12}\\
   V_{13}\\
   \end{bmatrix}=
   \begin{bmatrix}
   -b_{21}\\
   -b_{31}\\
   \end{bmatrix}
\end{equation}$$


$$\downarrow$$

$$
\begin{equation}
   \begin{bmatrix}
   V_{12}\\
   V_{13}\\
   \end{bmatrix}=
   {\begin{bmatrix}
   b_{22} & b_{23}\\
   b_{32} & b_{33}\\
   \end{bmatrix}}^{-1}
   \begin{bmatrix}
   -b_{21}\\
   -b_{31}\\
   \end{bmatrix}
\end{equation}$$

Then, normalize the eigenvector
$$V_1={V_1\over\Vert V_1\Vert}$$


## Example <br>
```c++
int main()
{
    //path는 행렬 데이터가 저장된 파일
    Matrix A = txtmat(path_A); 

    //반환값을 저장할 matrix 선언
    Matrix val = createMat(A.rows, A.cols);
    eigVals = eigenVal(A, val);
    eigVecs = eigvec(matA, val);

    printMat(eigVals, "EigenValue");
    printMat(eigVecs, "EigenVector");
}
```

## Output <br>
```c
EigenValue =
      73.015425        0.000000        0.000000
       0.000000       15.522467        0.000000
       0.000000        0.000000        3.462108

EigenVector =
       1.000000        3.408315       -0.072080
      -1.005738        1.000000        1.460856
      -1.396468       -1.717200        1.000000

```
## Warning
>A is square matrix<br>
A.rows = val.rows, A.cols = val.cols<br>


## Error Handling
```c
if (A.rows != val.rows || A.cols != val.cols || A.rows != A.cols)
{
	printf("Error: A is not square matrix A /  The sizes of matrix A and matrix val are not same\n");

	return zeros(A.rows, A.cols);
}
```

