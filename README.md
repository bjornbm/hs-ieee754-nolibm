This library is a fork of Patrick Perry's [_ieee754_][1] library, with
the dependency on _libm_ removed. It relies instead on unspecified
behavior.

Problem with depending on libm are discussed in [GHC issue 3242][2].


The only functions from libm used in ieee754 are `copysign` and
`copysignf`. To avoid the libm dependency altogether in
_ieee-754-nolibm_ these functions have been replaced with a pure Haskell
implementation. The pure Haskell implementation is no doubt much slower
than libm, and probably not correct in all cases (involving _NaN_) on
all architectures.

That being said all tests from ieee754 pass on:

* GHC 7.8.3 on Mac OS X 10.9.5.
* GHC 7.8.3 32 bit on Windows Server 2008 RC2.

To pass the `copySign` D5 and F5 tests the `significand` is used to
determine the sign of `y` when `isNaN y`. However, per the [Prelude
docs][3] the behavior of `significand` is unspecified on _NaN_ values,
so the apparent success may be particular to the specific test cases
and/or the combination of compiler and architecture.

All functions other than `copySign` are unchanged from ieee754.

[1]: https://hackage.haskell.org/package/ieee754
[2]: https://ghc.haskell.org/trac/ghc/ticket/3242
[3]: http://hackage.haskell.org/package/base-4.7.0.1/docs/Prelude.html#v:significand
