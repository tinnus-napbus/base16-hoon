# base16-hoon
The [base16](https://github.com/chriskempson/base16) syntax highlighting system in hoon.

The `$sch` structure defined in `/sur/base16/hoon` defines a colour scheme. Text must be parsed to a `$segs` list where each part of the text is tagged with a `$bas` base16 code for foreground and optionally background colour. The `/lib/base16.hoon` door is given the `$sch` as its sample, and then the `$segs` is given to the `++highlight` arm. The result is a `$manx` XML structure whose root is a `div` with a `lang-block-hl` class. The div contains a series of `span`s with the foreground & (maybe) background colour of each `span` specified in its style attribute. The background of the whole div will also be set to the default background colour of the chosen scheme, and `white-space: pre` will be set.

The `/lib/base16/scheme.hoon` library contains all default base16 themes. Each arm is a different scheme.

The `/lib/base16/language` folder contains parsers which take tapes and produce `$seg`s. Currently there's `generic.hoon` which applies no specific highlighting, just default background and foreground for the given scheme. There's also `json.hoon` which highlights JSON syntax.

To use this, you'd do something like:

```hoon
/+  b16=base16, scheme=base16-scheme, b16-json=base16-language-json
::
=/  txt  (trip '{"foo":"bar","baz":true}')
=/  hler  ~(highlight b16 gruvbox-dark-hard:scheme)
(hler ~ (b16-json txt))
```

It will produce a `$manx` that'll look like this when encoded:

```html
<div class="lang-block-hl" style="background-color:#1d2021;white-space:pre;">
	<span style="color:#d5c4a1;">{</span>
	<span style="color:#d3869b;">&quot;foo&quot;</span>
	<span style="color:#d5c4a1;">:</span>
	<span style="color:#b8bb26;">&quot;bar&quot;</span>
	<span style="color:#d5c4a1;">,</span>
	<span style="color:#d3869b;">&quot;baz&quot;</span>
	<span style="color:#d5c4a1;">:</span>
	<span style="color:#fe8019;">true</span>
	<span style="color:#d5c4a1;">}</span>
</div>
```

...which will look like:

![2022-05-12_12h19m24s](https://user-images.githubusercontent.com/77721746/167967882-87fe1a44-276f-4cc0-a4bc-d7e6cfb05c8d.png)


Note you're not limited to the included schemes in `scheme.hoon`, you can define your own `$sch` structure and use that instead.
