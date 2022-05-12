# base16-hoon
The [base16](https://github.com/chriskempson/base16) syntax highlighting system in hoon.

The `$sch` structure defined in `/sur/base16/hoon` defines a colour scheme. Text must be parsed to a `$segs` list where each part of the text is tagged with a `$bas` base16 code for foreground and optionally background colour. The `/lib/base16.hoon` door is given the `$sch` as its sample, and then the `$segs` is given to the `++highlight` arm. The result is a `$manx` XML structure whose root is a `div` with a `lang-block-hl` class. The div contains a series of `span`s with the foreground & (maybe) background colour of each `span` specified in its style attribute. The background of the whole div will also be set to the default background colour of the chosen scheme.

The `/lib/base16/scheme.hoon` library contains all default base16 themes. Each arm is a different scheme.

The `/lib/base16/language` folder contains parsers which take tapes and produce `$seg`s. Currently there's `generic.hoon` which applies no specific highlighting, just default background and foreground for the given scheme. There's also `json.hoon` which highlights JSON syntax.

To use this, you'd do something like:

```hoon
/+  b16=base16, scheme=base16-scheme, b16-json=base16-language-json
::
=/  txt  (trip '{"foo":"bar","baz":true}')
=/  hler  ~(highlight b16 gruvbox-dark-hard:scheme)
(hler (b16-json txt))
```

Note you're not limited to the included schemes in `scheme.hoon`, you can define your own `$sch` structure and use that instead.
