---
description: We need to write to the console
icon: pen
---

# Console Writers

Terminaux provides a vast amount of console writers for different purposes, like the progress bar writer, writing console output in color, etc. Also, you can use their `Render()` functions found in basically every static writer (not those that are dynamic, such as wrapped writers).

{% hint style="info" %}
The `Render()` functions are made primarily for plain writing operations. You can use them with `TextWriterRaw` writers.
{% endhint %}

***

## <mark style="color:$primary;">Markups</mark>

Terminaux provides you with a simple BBCode-inspired markup syntax intended to style your own text without resorting to complicated string concatenations. It allows you to seamlessly apply formatting to different parts of text designed to be printed to the console.

You can use the markup to convert your markup text to a raw text using the following methods:

* Creating a `Mark` instance with your text and calling `ParseMarkup()`.
* Calling `ParseMarkup()` from `MarkupTools`.

This is all found in the `Terminaux.Writer.CyclicWriters.Renderer.Markup` namespace. This maintains compatibility with [Spectre.Console markup syntax](https://spectreconsole.net/markup) to some degree.

### <mark style="color:$primary;">Supported syntaxes</mark>

You can use the following syntaxes:

* Text formatting that you can use with this syntax: `[format]Hello![/]`
* Color specifiers that Terminaux can parse, such as `Red`, `#FF0000`, or `255;0;0`.

### <mark style="color:$primary;">Text formatting</mark>

As for text formatting, here are the supported formatting specifiers:

<table><thead><tr><th width="140">Specifier</th><th>Description</th></tr></thead><tbody><tr><td><code>bold</code></td><td>Makes the surrounding text bold</td></tr><tr><td><code>conceal</code></td><td>Makes the surrounding text hidden</td></tr><tr><td><code>dim</code></td><td>Makes the surrounding text dimmer</td></tr><tr><td><code>invert</code></td><td>Makes the surrounding text's foreground and background color inverted</td></tr><tr><td><code>italic</code></td><td>Makes the surrounding text italic</td></tr><tr><td><code>rapidblink</code></td><td>Makes the surrounding text blink rapidly</td></tr><tr><td><code>slowblink</code></td><td>Makes the surrounding text blink slowly</td></tr><tr><td><code>standout</code></td><td>Makes the surrounding text stand out</td></tr><tr><td><code>strikethrough</code></td><td>Makes the surrounding text crossed out</td></tr><tr><td><code>underline</code></td><td>Makes the surrounding text underlined</td></tr></tbody></table>
