---
description: We need to filter and build the VT sequences.
icon: wave-sine
---

# VT Sequences

In your own terminal emulator, VT sequences are the power of any terminal emulator found in literally every PC.

This feature provides several filtering and manipulation tools which allow you to perform these operations on strings that contain escape sequences under the `Terminaux.Sequences` namespace. Currently, these tools are provided:

| Function                       | Description                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------- |
| `FilterVTSequences()`          | Filters all of the VT sequences that are of either a single type or of multiple types  |
| `MatchVTSequences()`           | Matches all of the VT sequences that are of either a single type or of multiple types  |
| `IsMatchVTSequences()`         | Does the string contain all of the VT sequences or a VT sequence of one or more types? |
| `IsMatchVTSequencesSpecific()` | Does the string contain all of the VT sequences or a VT sequence of any specific type? |
| `SplitVTSequences()`           | Splits all of the VT sequences that are either of a single type or of multiple types   |
| `DetermineTypeFromText()`      | Determines the VT sequence type from the given text                                    |

## Usage

Place the `using Terminaux.Sequences;` clause at the top of the file that you want to call these functions in. You need to put the class name `VtSequenceTools` before the function name mentioned above so that it looks like this:

<pre class="language-csharp"><code class="lang-csharp">char EscapeChar = Convert.ToChar(0x1b);
string vtSequence1 = $"{EscapeChar}[38;5;43m";
<strong>string filtered = VtSequenceTools.FilterVTSequences($"Hello!{vtSequence1}");
</strong><strong>TextWriterRaw.WritePlain(filtered);
</strong></code></pre>

### Sequence builder

Terminaux offers a `Builder` namespace that contains building blocks for building a VT sequence for your console applications.

It starts with `VtSequenceBasicChars` which allows you to get a variety of starting-point characters for your VT sequence in case you want to manually build the sequence yourself. Here are the available characters:

* `BEL (\x07 - CTRL+G - BellChar)`
* `BS (\x08 - CTRL+H - BackspaceChar)`
* `CR (\x0D - CTRL+M - CarriageReturnChar)`
* `ENQ (\x05 - CTRL+E - ReturnTerminalStatusChar)`
* `FF (\x0C - CTRL+L - FormFeedChar)`
* `LF (\x0A - CTRL+J - LineFeedChar)`
* `SI (\x0F - CTRL+O - StandardCharacterSetChar)`
* `SO (\x0E - CTRL+N - AlternateCharacterSetChar)`
* `SP (" " - SPACE - SpaceChar)`
* `TAB (\x09 - CTRL+I - HorizontalTabChar)`
* `VT (\x0B - CTRL+K - VerticalTabChar)`
* `ESC (\x1B - EscapeChar)`
* `ST (\x9C - StChar)`
* `CSI (\x9B - CsiChar)`
* `OSC (\x9D - OSCChar)`
* `APC (\x9F - APCChar)`
* `DCS (\x90 - DCSChar)`
* `PM (\x9E - PMChar)`

Each type of VT sequence contain their own class files that stores sequence generation functions based on the given action and argument. Here are a list of supported sequence types:

* APC sequences
  * Application program command
* C1 sequences
  * 8-bit control characters
* CSI sequences
  * Controls beginning with control sequence introducer
* DCS sequences
  * Device control
* ESC sequences
  * Controls beginning with ESC
* OSC sequences
  * Operating system command sequences
* PM sequences
  * Privacy message

To learn more about these sequences, visit the below page:

{% embed url="https://invisible-island.net/xterm/ctlseqs/ctlseqs.html" %}

The builder also provides a powerful tool which allows you to build almost any VT sequence using only the arguments and the specific type of VT sequence (Character attributes for example). This tool is found inside the `VtSequenceBuilderTools` class under the `Terminaux.Sequences.Builder` namespace.

Currently, it provides these tools:

* `BuildVtSequence(VtSequenceSpecificTypes specificType, params object[] arguments)`
  * Allows you to build your VT sequence
  * Returns a string consisting of a VT sequence of the requested type with the requested arguments
* `DetermineTypesFromSequence(string sequence)`
  * Allows you to determine the types from a VT sequence
  * Returns an enumerable containing a list of types and specific types

## Techniques

There are currently four types of operations.

### Filters

Found in `VtSequenceTools.FilterVTSequences()`, when this function is called, Terminaux tokenizes all VT sequences found in a target text and replaces the found sequences with blanks. You can optionally replace it with something, like a text, a syntax, or even another VT sequence.

### Matches

Found in `VtSequenceTools.MatchVTSequences()`, when this function is called, Terminaux tokenizes all VT sequences found in a target text and gets all the matched sequences. You can then wrap the values in the `for` or the `foreach` loop to get information for each sequence.

### Queries

Found in `VtSequenceTools.IsMatchVTSequences()`, when this function is called, Terminaux tokenizes all VT sequences found in a target text and checks the text to see if any part of the text is a VT sequence.

Additionally, it contains `IsMatchVTSequencesSpecific()`, which helps to check the text for any specific VT sequence type.

### Splits

Found in `VtSequenceTools.SplitVTSequences()`, when this function is called, Terminaux tokenizes all VT sequences found in a target text and splits the text with the VT sequences as delimiters.

## Usage in console writers

All console writers internally use the VT sequence filtering tools, like `GetFilteredPositions()` that uses `FilterVTSequences()`, to be able to determine the exact text position. This is to work around Linux Mono installs that report wrong position when VT sequences are appended.

{% hint style="info" %}
This workaround was not necessary as of the modern .NET implementation, but the workaround is still in place, because Terminaux targets .NET Standard, which means that it can be used for .NET Framework console projects, and, thus, Mono on Linux.
{% endhint %}

It works by seeking through every visible letter from the text in the form of "advancing" so that `GetFilteredPositions()` can make sure that the filtered positions are actually correct and sought "to the last letter" rather than "past the last letter" that Mono and the normal `Console.Write()` and `Console.WriteLine()` were suffering.
