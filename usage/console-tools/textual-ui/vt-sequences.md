---
description: We need to filter and build the VT sequences.
icon: wave-sine
---

# VT Sequences

In your own terminal emulator, VT sequences are the power of any terminal emulator found in literally every PC.

This feature provides several filtering and manipulation tools which allow you to perform these operations on strings that contain escape sequences under the `Terminaux.Sequences` namespace.

***

## <mark style="color:$primary;">VT sequence types</mark>

Each type of VT sequence contain their own class files that stores sequence generation functions based on the given action and argument. Here are a list of supported sequence types:

<table><thead><tr><th width="159.333251953125">Sequence type</th><th>Description</th></tr></thead><tbody><tr><td>APC sequences</td><td>Application program command</td></tr><tr><td>C1 sequences</td><td>8-bit control characters</td></tr><tr><td>CSI sequences</td><td>Controls beginning with control sequence introducer</td></tr><tr><td>DCS sequences</td><td>Device control</td></tr><tr><td>ESC sequences</td><td>Controls beginning with ESC</td></tr><tr><td>OSC sequences</td><td>Operating system command sequences</td></tr><tr><td>PM sequences</td><td>Privacy message</td></tr></tbody></table>

To learn more about these sequences, visit the below page:

{% embed url="https://invisible-island.net/xterm/ctlseqs/ctlseqs.html" %}

***

## <mark style="color:$primary;">Usage</mark>

Place the `using Terminaux.Sequences;` clause at the top of the file that you want to call these functions in. You need to put the class name `VtSequenceTools` before the function name mentioned above so that it looks like this:

<pre class="language-csharp"><code class="lang-csharp">char EscapeChar = Convert.ToChar(0x1b);
string vtSequence1 = $"{EscapeChar}[38;5;43m";
<strong>string filtered = VtSequenceTools.FilterVTSequences($"Hello!{vtSequence1}");
</strong><strong>TextWriterRaw.WritePlain(filtered);
</strong></code></pre>

Here are some of the common tools. Expand one of the expandables to get more info.

<details>

<summary>Available functions</summary>

Currently, these tools are provided:

<table><thead><tr><th width="269.994873046875">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>FilterVTSequences()</code></td><td>Filters all of the VT sequences that are of either a single type or of multiple types</td></tr><tr><td><code>MatchVTSequences()</code></td><td>Matches all of the VT sequences that are of either a single type or of multiple types</td></tr><tr><td><code>IsMatchVTSequences()</code></td><td>Does the string contain all of the VT sequences or a VT sequence of one or more types?</td></tr><tr><td><code>IsMatchVTSequencesSpecific()</code></td><td>Does the string contain all of the VT sequences or a VT sequence of any specific type?</td></tr><tr><td><code>SplitVTSequences()</code></td><td>Splits all of the VT sequences that are either of a single type or of multiple types</td></tr><tr><td><code>DetermineTypeFromText()</code></td><td>Determines the VT sequence type from the given text</td></tr></tbody></table>

</details>

<details>

<summary>Sequence builder</summary>

Terminaux offers a `Builder` namespace that contains building blocks for building a VT sequence for your console applications.

It starts with `VtSequenceBasicChars` which allows you to get a variety of starting-point characters for your VT sequence in case you want to manually build the sequence yourself.

Here are the available characters:

<table><thead><tr><th width="150">Character code</th><th width="170">Character number</th><th>Character property</th></tr></thead><tbody><tr><td><code>BEL</code></td><td><code>\x07</code></td><td><code>BellChar</code></td></tr><tr><td><code>BS</code></td><td><code>\x08</code></td><td><code>BackspaceChar</code></td></tr><tr><td><code>CR</code></td><td><code>\x0D</code></td><td><code>CarriageReturnChar</code></td></tr><tr><td><code>ENQ</code></td><td><code>\x05</code></td><td><code>ReturnTerminalStatusChar</code></td></tr><tr><td><code>FF</code></td><td><code>\x0C</code></td><td><code>FormFeedChar</code></td></tr><tr><td><code>LF</code></td><td><code>\x0A</code></td><td><code>LineFeedChar</code></td></tr><tr><td><code>SI</code></td><td><code>\x0F</code></td><td><code>StandardCharacterSetChar</code></td></tr><tr><td><code>SO</code></td><td><code>\x0E</code></td><td><code>AlternateCharacterSetChar</code></td></tr><tr><td><code>SP</code></td><td><code>" "</code></td><td><code>SpaceChar</code></td></tr><tr><td><code>TAB</code></td><td><code>\x09</code></td><td><code>HorizontalTabChar</code></td></tr><tr><td><code>VT</code></td><td><code>\x0B</code></td><td><code>VerticalTabChar</code></td></tr><tr><td><code>ESC</code></td><td><code>\x1B</code></td><td><code>EscapeChar</code></td></tr><tr><td><code>ST</code></td><td><code>\x9C</code></td><td><code>StChar</code></td></tr><tr><td><code>CSI</code></td><td><code>\x9B</code></td><td><code>CsiChar</code></td></tr><tr><td><code>OSC</code></td><td><code>\x9D</code></td><td><code>OSCChar</code></td></tr><tr><td><code>APC</code></td><td><code>\x9F</code></td><td><code>APCChar</code></td></tr><tr><td><code>DCS</code></td><td><code>\x90</code></td><td><code>DCSChar</code></td></tr><tr><td><code>PM</code></td><td><code>\x9E</code></td><td><code>PMChar</code></td></tr></tbody></table>

The builder also provides a powerful tool which allows you to build almost any VT sequence using only the arguments and the specific type of VT sequence (Character attributes for example). This tool is found inside the `VtSequenceBuilderTools` class under the `Terminaux.Sequences.Builder` namespace.

Currently, it provides these tools:

<table><thead><tr><th width="266.00006103515625">Function</th><th>Description</th><th>Returns</th></tr></thead><tbody><tr><td><code>BuildVtSequence()</code></td><td>Builds a VT sequence</td><td>A string consisting of a VT sequence of the requested type with the requested arguments</td></tr><tr><td><code>DetermineTypesFromSequence()</code></td><td>Determines the types from a VT sequence</td><td>An enumerable containing a list of types and specific types</td></tr></tbody></table>

</details>

<details>

<summary>Operations</summary>

When Terminaux tokenizes all VT sequences found in a target text, one of the below operations happen.

There are currently four types of operations.

<table><thead><tr><th width="119.6666259765625">Operation</th><th></th></tr></thead><tbody><tr><td>Filtering</td><td><code>FilterVTSequences()</code> replaces the found sequences with blanks. You can optionally replace it with something, like a text, a syntax, or even another VT sequence. </td></tr><tr><td>Matching</td><td><code>MatchVTSequences()</code> gets all the matched sequences. You can then wrap the values in the <code>for</code> or the <code>foreach</code> loop to get information for each sequence.</td></tr><tr><td>Querying</td><td><code>IsMatchVTSequences()</code> checks the text to see if any part of the text is a VT sequence.</td></tr><tr><td></td><td><code>IsMatchVTSequencesSpecific()</code> checks the text for any specific VT sequence type.</td></tr><tr><td>Splitting</td><td><code>SplitVTSequences()</code> splits the text with the VT sequences as delimiters.</td></tr></tbody></table>

</details>

<details>

<summary>Usage in console writers for Mono</summary>

All console writers internally use the VT sequence filtering tools, like `GetFilteredPositions()`, to be able to determine the exact text position. This is to work around Linux Mono installs that report wrong position when VT sequences are appended.

{% hint style="info" %}
This workaround was not necessary as of the modern .NET implementation, but the workaround is still in place, because Terminaux targets .NET Standard, which means that it can be used for .NET Framework console projects, and, thus, Mono on Linux.
{% endhint %}

It works by seeking through every visible letter from the text in the form of "advancing" so that `GetFilteredPositions()` can make sure that the filtered positions are actually correct and sought "to the last letter" rather than "past the last letter" that Mono and the normal `Console.Write()` and `Console.WriteLine()` functions were suffering.

</details>
