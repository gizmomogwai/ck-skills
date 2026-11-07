# Reserved identifiers in D

Source of truth: the official [D lexical specification](https://dlang.org/spec/lex.html#keywords).
Consult the specification again when targeting a different language/compiler
version.

## Keywords

Keywords cannot be used as ordinary identifiers, including field and property
function names. Some reserved keywords describe deprecated or unimplemented
features; that does not make the names available.

```text
abstract alias align asm assert auto
body bool break byte
case cast catch cdouble cent cfloat char class const continue creal
dchar debug default delegate delete deprecated do double
else enum export extern
false final finally float for foreach foreach_reverse function
goto
idouble if ifloat immutable import in inout int interface invariant ireal is
lazy long
macro mixin module
new nothrow null
out override
package pragma private protected public pure
real ref return
scope shared short static struct super switch synchronized
template this throw true try typeid typeof
ubyte ucent uint ulong union unittest ushort
version void
wchar while with
__FILE__ __FILE_FULL_PATH__ __FUNCTION__ __LINE__ __MODULE__
__PRETTY_FUNCTION__ __gshared __parameters __rvalue __traits __vector
```

## Compiler-reserved names and naming pitfalls

The [identifier rules](https://dlang.org/spec/lex.html#identifiers) reserve names
starting with two underscores. Do not declare custom `__...` names.
Compiler names such as `__traits` and `__FILE__` remain valid to use for their
documented purpose.

The specification also lists special tokens: `__DATE__`, `__EOF__`, `__TIME__`,
`__TIMESTAMP__`, `__VENDOR__`, and `__VERSION__`. These are compiler facilities,
not available application names.

`property`, `safe`, and `nogc` are not lexical keywords. They are recognized
in built-in attribute syntax (`@property`, `@safe`, `@nogc`); do not mistake
them for universally forbidden identifiers.

The [D naming convention](https://dlang.org/dstyle.html#naming_keywords)
recommends an underscore suffix when an API needs a keyword-like identifier:
`body_`, `scope_`, or `out_`. Prefer a meaningful alternative where possible.
