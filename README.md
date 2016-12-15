# Multivariate Polynomials

[![Build Status](https://travis-ci.org/blegat/MultivariatePolynomials.jl.svg?branch=master)](https://travis-ci.org/blegat/MultivariatePolynomials.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/4l5i8sbxev8405jl?svg=true)](https://ci.appveyor.com/project/blegat/multivariatepolynomials-jl)
[![Coverage Status](https://coveralls.io/repos/github/blegat/MultivariatePolynomials.jl/badge.svg?branch=master)](https://coveralls.io/github/blegat/MultivariatePolynomials.jl?branch=master)
[![codecov.io](http://codecov.io/github/blegat/MultivariatePolynomials.jl/coverage.svg?branch=master)](http://codecov.io/github/blegat/MultivariatePolynomials.jl?branch=master)

Basic arithmetic, integration, differentiation and evaluation over sparse multivariate polynomials and sparse multivariate moments.
The following types are defined:

* `PolyVar`: A variable. They are created using the `@polyvar` macro, e.g. `@polyvar x y`, `@polyvar x[1:8]`.
* `Monomial`: A product of variables: e.g. `x*y^2`.
* `Term{T}`: A product between an element of type `T` and a `Monomial`, e.g `2x`, `3.0x*y^2`.
* `VecPolynomial{T}`: A sum of `Term{T}`, e.g. `2x + 3.0x*y^2 + y`.
* `Moment{T}`: The multivariate moment of type `T` of a measure, e.g. `E_μ[x*y^2]` is the moment of `μ` corresponding to the monomial `x*y^2`.
* `Measure{T}`: A combination of `Moment{T}` of a measure, e.g. the moments of `x`, `x*y^2` and `y`.

All common algebraic operations between those types are designed to be as efficient as possible without doing any assumption on `T`.
Typically, one imagine `T` to be a subtype of `Number` but it can be anything.
This is useful for example in the package [PolyJuMP](https://github.com/JuliaOpt/PolyJuMP.jl) where `T` is often an affine expression of [JuMP](https://github.com/JuliaOpt/JuMP.jl) decision variables.

Below is a simple usage example
```julia
@polyvar x y
p = 2x + 3.0x*y^2 + y
differentiate(p, x) # compute the derivative of p with respect to x
differentiate(p, [x, y]) # compute the gradient of p
p([y, x], [x, y]) # replace any x by y and y by x
p([x^2], [y]) # replace any occurence of y by x^2
p([1, 2], [x, y]) # evaluate p at [1, 2]
```
Below is an example with `@polyvar x[1:n]`
```julia
n = 3
A = rand(3, 3)
@polyvar x[1:n]
p = dot(x, x) # x_1^2 + x_2^2 + x_3^2
p(A*x, x) # corresponds to dot(A*x, A*x)
p([2, 3], [x[1], x[3]]) # x_2^2 + 13
```
Note that, when doing substitution, it is required to give the `PolyVar` ordering that is meant.
Indeed, the ordering between the `PolyVar` is not alphabetical but rather by order of creation
which can be undeterministic with parallel computing.
Therefore, this order cannot be used for substitution, even as a default (see [here](https://github.com/blegat/MultivariatePolynomials.jl/issues/3) for a discussion about this).

## See also

* [Nemo](https://github.com/wbhart/Nemo.jl) for generic polynomial rings, matrix spaces, fraction fields, residue rings, power series

* [Polynomials](https://github.com/Keno/Polynomials.jl) for univariate polynomials

* [PolynomialRoots](https://github.com/giordano/PolynomialRoots.jl) for a fast complex polynomial root finder

* [MultiPoly](https://github.com/daviddelaat/MultiPoly.jl) another package for sparse multivariate polynomials
