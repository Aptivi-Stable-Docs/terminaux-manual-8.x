---
description: What's your terminal type and its capabilities?
icon: square-terminal
---

# Terminal Info

Terminal information database is a list of terminal capabilities that control how console applications determine capabilities in the terminal type that the emulator uses, such as the color depth capability and the method of writing sequences that change the background and the foreground color.

Termcap was implemented in 1978, and Terminfo was implemented in 1981-1982. Applications that use `ncurses` (for non-C# applications) or Terminaux (for C# applications) can use the database to get the capabilities.

{% hint style="info" %}
This feature used to be exclusive to Linux users mostly, but we've added the built-in terminal information database so that Windows applications are now free to analyze such data, although `conhost` doesn't emulate any terminal, and is assumed to be compatible with `xterm`.
{% endhint %}

***

## <mark style="color:$primary;">`TermInfoDesc`</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">class</mark>

This class describes a TermInfo instance that contains all properties, including strings, booleans, and integers.

<details>

<summary>Functions of <code>TermInfoDesc</code></summary>

The TermInfo feature of Terminaux can be found in the base console tools namespace, `Terminaux.Base.TermInfo`.  `TermInfoDesc`'s functions contain the following:

<table><thead><tr><th width="150.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>TryLoad()</code></td><td>Tries to load a terminal information description given the terminal type name.</td></tr><tr><td><code>Load()</code></td><td><p>Loads a terminal information description given the terminal type name, and if the name is not given, it uses the current terminal type that your terminal emulator uses.</p><p></p><p>If <code>TryLoad()</code> fails and you've specified a name, it throws an exception. Otherwise, it gives you a fallback <code>TermInfoDesc</code> instance.</p></td></tr><tr><td><code>LoadSafe()</code></td><td><p>Loads a terminal information description given the terminal type name, and if the name is not given, it uses the current terminal type that your terminal emulator uses.</p><p></p><p>If <code>TryLoad()</code> fails or <code>$TERM</code> is not specified, it gives you a fallback <code>TermInfoDesc</code> instance.</p></td></tr><tr><td><code>GetBuiltins()</code></td><td>Gets a list of built-in terminal type names that Terminaux can find in the built-in capabilities list.</td></tr></tbody></table>

</details>

<details>

<summary>Obtaining terminal capabilities</summary>

Built-in terminal capabilities can be accessed as properties in the `TermInfoDesc` instance that the loading functions return when parsing the terminal information files.

For extra properties that Terminaux doesn't cover, this class provides you with the following functions:

<table><thead><tr><th width="139.6666259765625">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>GetBoolean()</code></td><td>Gets a boolean value that is either <code>true</code> or <code>false</code>.</td></tr><tr><td><code>GetNum()</code></td><td>Gets a number value that is a numeric integer.</td></tr><tr><td><code>GetString()</code></td><td>Gets a textual value.</td></tr></tbody></table>

{% hint style="info" %}
All three functions return `null` if Terminaux is unable to get the value.
{% endhint %}

</details>

<details>

<summary>Obtaining current terminal capabilities</summary>

`TermInfoDesc` also allows you to get an instance of `TermInfoDesc` corresponding to the current terminal type as specified in the `$TERM` environment variable for non-Windows systems.

You can get this instance using one of the following properties:

<table><thead><tr><th width="110.333251953125">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Current</code></td><td>Returns a <code>TermInfoDesc</code> instance corresponding to your terminal type.</td></tr><tr><td><code>Fallback</code></td><td>Returns a <code>TermInfoDesc</code> instance corresponding to <code>xterm-256color</code>.</td></tr></tbody></table>

{% hint style="info" %}
Windows systems always use the `xterm-256color` terminal for maximum compatibility.
{% endhint %}

</details>

<details>

<summary>TermInfo string parameters</summary>

Terminaux supports all the [printf(3)](https://manpages.debian.org/bullseye/manpages-dev/printf.3.en.html)-like string parameters that are defined in the [Parameterized Strings](https://manpages.debian.org/bullseye/ncurses-bin/terminfo.5.en.html#Parameterized_Strings) section from the `terminfo(5)` manual page found in the NCurses library.

The following string parameters are supported according to the types:

<table><thead><tr><th width="339.3333740234375">Parameter</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>%%</code></td><td>Format</td><td>Writes a literal <code>%</code></td></tr><tr><td><code>%[[:]flags][width[.precision]][doxXs]</code></td><td>Format</td><td>Formats a parameter in a way similar to <code>printf</code></td></tr><tr><td><code>%c</code></td><td>Format</td><td>Print a parameter as an unsigned character</td></tr><tr><td><code>%s</code></td><td>Format</td><td>Print a parameter as a string</td></tr><tr><td><code>%'c'</code></td><td>Constants</td><td>Single character constant</td></tr><tr><td><code>%{nn}</code></td><td>Constants</td><td>Integral constant</td></tr><tr><td><code>%p[1-9]</code></td><td>Manipulation</td><td>Pushes the <code>n</code>th parameter</td></tr><tr><td><code>%P[a-z]</code></td><td>Manipulation</td><td>Set dynamic variable to pop</td></tr><tr><td><code>%g[a-z]</code></td><td>Manipulation</td><td>Get dynamic variable to push</td></tr><tr><td><code>%P[A-Z]</code></td><td>Manipulation</td><td>Set static variable to pop</td></tr><tr><td><code>%g[A-Z]</code></td><td>Manipulation</td><td>Get static variable to push</td></tr><tr><td><code>%l</code></td><td>Manipulation</td><td>Formats as string length of the pop'd variable</td></tr><tr><td><code>%i</code></td><td>Manipulation</td><td>Adds 1 to the first two variables</td></tr><tr><td><code>%+</code></td><td>Binary (arithmetic)</td><td>Adds two variables</td></tr><tr><td><code>%-</code></td><td>Binary (arithmetic)</td><td>Subtracts two variables</td></tr><tr><td><code>%*</code></td><td>Binary (arithmetic)</td><td>Multiplies two variables</td></tr><tr><td><code>%/</code></td><td>Binary (arithmetic)</td><td>Divides two variables</td></tr><tr><td><code>%m</code></td><td>Binary (arithmetic)</td><td>Returns mod of two variables</td></tr><tr><td><code>%&#x26;</code></td><td>Binary (bit ops)</td><td>Bitwise AND</td></tr><tr><td><code>%|</code></td><td>Binary (bit ops)</td><td>Bitwise OR</td></tr><tr><td><code>%^</code></td><td>Binary (bit ops)</td><td>Bitwise exclusive OR</td></tr><tr><td><code>%=</code></td><td>Binary (logical ops)</td><td>Two variables equal</td></tr><tr><td><code>%&#x3C;</code></td><td>Binary (logical ops)</td><td>Two variables less than</td></tr><tr><td><code>%></code></td><td>Binary (logical ops)</td><td>Two variables greater than</td></tr><tr><td><code>%A</code></td><td>Conditional operations</td><td>Logical AND</td></tr><tr><td><code>%O</code></td><td>Conditional operations</td><td>Logical OR</td></tr><tr><td><code>%!</code></td><td>Unary operations</td><td>Logical complement</td></tr><tr><td><code>%~</code></td><td>Unary operations</td><td>Bit complement</td></tr><tr><td><code>%? expr %t thenpart %e elsepart %;</code></td><td>Conditional</td><td>If-then-else conditional</td></tr></tbody></table>

### <mark style="color:$primary;">Parameter Extraction</mark>

You can extract parameters from a capability string by using the `ExtractParameters()` function found in the `ParameterExtractor` class, passing it the capability that you want to parse. This returns an array of `ParameterInfo` instances that contain the following properties:

* Representation: The parameter that has been extracted from the capability string.
* Index: Zero-based index of the first character of the representation relative to the capability string.

For example, we have this string: `<ESCAPE>=%p1%' '%+%c%p2%' '%+%c`, with `<ESCAPE>` being a designator for `\u001b` that corresponds to the ESCAPE character essential for all terminal [VT sequences](textual-ui/vt-sequences.md) according to Unicode and ASCII encoding. When we ran this string through the parameter extractor, we got this result:

{% code expandable="true" %}
```
<ESCAPE> is not a parameter at index 0
=        is not a parameter at index 1

%p1  starts at index 2
  Representation: %p1
  Index:          2
%' ' starts at index 5
  Representation: %' '
  Index:          5
%+   starts at index 9
  Representation: %+
  Index:          9
%c   starts at index 11
  Representation: %c
  Index:          11
%p2  starts at index 13
  Representation: %p2
  Index:          13
%' ' starts at index 16
  Representation: %' '
  Index:          16
%+   starts at index 20
  Representation: %+
  Index:          20
%c   starts at index 22
  Representation: %c
  Index:          22
```
{% endcode %}

{% hint style="info" %}
For now, it's up to you how to process such strings to manipulate with the parameters. However, in a future Terminaux release, we'll release features that will assist you.
{% endhint %}

</details>

***

## <mark style="color:$primary;">Tabsets</mark>

Tabsets (or tab stops) are numbers that determine the tab width and a stop for each equal width. They are distributed with NCurses under the four types:

* `std`
* `stdcrt`
* `vt100`
* `vt300`

`TabsetParser` allows you to get information about the tabsets from one of the four types, as well as a custom tabset that you've registered and parsed.

Once you've parsed a tabset file, you can utilize the `TabStops` property inside the class instance to get the number of tab stops and their positions, as well as the initialization sequence property.
