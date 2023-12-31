# PPX quick start

[![codecov](https://codecov.io/github/Xtinc/matrix/branch/master/graph/badge.svg?token=VVGCGPHZ2W)](https://codecov.io/github/Xtinc/matrix)
[![CodeFactor](https://www.codefactor.io/repository/github/xtinc/matrix/badge)](https://www.codefactor.io/repository/github/xtinc/matrix)
[![CI](https://github.com/Xtinc/matrix/actions/workflows/main.yml/badge.svg)](https://github.com/Xtinc/matrix/actions/workflows/matrix.yml)

A quick start for ppx users.

## Table of Contents

- [Background](#background)
- [Install](#install)
- [Usage](#usage)
- [License](#license)

## Background

ppx is a cluster of algorithms for numerical calculation. It consists of 3 parts: linear algebra, non-linear optimization, statistics.

The toolkit contains:

1. Linear algebra operations. ppx provides STL-like **static** containers to describe matrix and related operations such as norm-p, cofactor, determinant...(Be careful dimensions of matrix must be known before program runs. and lazy evaluation is always used in element-wised matrix operators)

2. Non-linear optimization algorithms. For 1D optimization, some line search methods are given.
    For N-Dimensions optimization problems, Direct method and gradient method are available.  

3. Statics toolkits. currently only some filters are developed.(FIR filters generated by window function, SG smoothing filter...)
4. see documents in /doc for more!

## Install

- Requires CPP standard 14
-  Header-only, include header file is enough

## Usage

A detailed usage should refer to document in doc directory. This is an simple example for primary user.

```cpp
#include "liegroup.hpp"
// declarations of  matrix
Matrix<4, 4> a = {1, 2, 3, 4, 5, 6, 7, 8};
PRINT_SINGLE_ELEMENTS(a);
PRINT_SINGLE_ELEMENTS(a(maxloc(a)), "max of a: ");
// slice of a matrix
a({2, 3}, {2, 3}) = Matrix<2, 2>{1, 1, 1, 1};
a(0, {0, -1}) = {89.0, 23.0, 44.0, 9.8};
PRINT_SINGLE_ELEMENTS(a, "a = ");
PRINT_LISTED_ELEMENTS(a, "a in list: ");
// functions for matrix operations
PRINT_SINGLE_ELEMENTS(determinant(a), "determinant(a) = ");
PRINT_SINGLE_ELEMENTS(inverse(a), "inverse(a) = ");
// expression templates for matrix operations
Matrix<4, 4> x = {1, 2, 3, 4, 5, 6, 7, 8};
Matrix<4, 4> y = x.T();
Matrix<4, 4> z = x * 2 + y * 2 + 3.3 + x * y;
PRINT_SINGLE_ELEMENTS(z, "2*(x + x^T) + 3.3 +x*x^T = ");
//  lie group operations
Matrix<3, 3> so3mat = {0, 3, -2, -3, 0, 1, 2, -1, 0};
PRINT_SINGLE_ELEMENTS(vee(so3mat).exp(), "exp(s) = ");
SO3 SO3mat = {0, 1, 0, 0, 0, 1, 1, 0, 0};
PRINT_SINGLE_ELEMENTS(SO3mat.log(), "log(S) = ");
PRINT_SINGLE_ELEMENTS(SE3(SO3mat, {1, 1, 1}), "T = ");
SE3 SE3mat = {1, 0, 0, 0, 0, 0, 1, 0, 0, -1, 0, 0, 0, 0, 3, 1};
PRINT_SINGLE_ELEMENTS(SE3mat, "SE3mat = ");
PRINT_SINGLE_ELEMENTS(SE3mat.log(), "log(s) = ");
PRINT_SINGLE_ELEMENTS(hat(SE3mat.log()));
PRINT_SINGLE_ELEMENTS(se3{1.5708, 0.0, 0.0, 0.0, 2.3562, 2.3562}.exp());
// linear solver for equations
Matrix<4, 3> X{3.5, 1.6, 3.7, 4.3,
                   2.7, -5.7, -0.8, -9.8,
                   -3.1, -6.0, 1.9, 6.9};
Matrix<4, 1> Y{1, 1, 1, 1};
PRINT_SINGLE_ELEMENTS(linsolve<factorization::QR>(X, Y), "solved by QR = ");
PRINT_SINGLE_ELEMENTS(linsolve<factorization::SVD>(X, Y), "solved by SVD = ");
// factorization for matrix
Matrix<4, 3> u{1, 4, 7, 11,
                2, 5, 8, 1,
`               3, 6, 9, 5};
Matrix<3, 1> v{};
Matrix<3, 3> w{};
bool sing = false;
u = svdcmp(u, v, w, sing);
PRINT_SINGLE_ELEMENTS(u, "U = ");
PRINT_SINGLE_ELEMENTS(v, "S = ");
PRINT_SINGLE_ELEMENTS(w, "V = ");
PRINT_SINGLE_ELEMENTS(u * v.diag() * w.T(), "U*S*V = ");
// eigenvalue for symmetry matrix
Matrix<4, 4> g{2, -1, 1, 4,
                   -1, 2, -1, 1,
                   1, -1, 2, -2,
                   4, 1, -2, 3};
PRINT_SINGLE_ELEMENTS(g, "g = ");
PRINT_SINGLE_ELEMENTS(eig<eigensystem::SymValAndVecSorted>(g).vec, "eigen vector of g : ");
PRINT_SINGLE_ELEMENTS(eig<eigensystem::SymOnlyVal>(g), "eigen value of g : ");

```

## Why a new numerical algorithm library ?

When numerical algorithms come to CPP, eigen seems to be first choice for almost all of us. It's currently ultra answer for that. However, eigen now is suffering from some afflictions that all big projects suffered. For example, inconsistency in API design caused by compatibility, compromise in code constrained by generality and over abstract hierarchy. 

ppx is designed to be a more simple numerical toolkit library compared to that. And it's not to be as powerful, multi-functional as eigen or ceres. It makes some strict restrictions on usage to make it design and implement simple and consistent. the goal for performance of algorithm implemented in ppx is that can reach same or 10% level  in time compared to eigen and ceres.

generally speaking, ppx is a numerical library that has some strict restrictions but designed and coded simple for exploring and testing algorithms. 


## License

[MIT](LICENSE) © XTinc
