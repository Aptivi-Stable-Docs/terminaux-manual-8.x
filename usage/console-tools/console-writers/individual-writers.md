---
description: Here's the documentation for individual writers
icon: pencil-mechanical
---

# Individual Writers

The most simple console writers live here, because they do the absolute minimum to write text or sequences to the console.

***

## <mark style="color:$primary;">Standard console writers</mark>

The standard console writers provide you with non-moving parts of the text. They have their own writers and renderers to allow you to use them in two ways. The writer writes directly to the console, while the renderers are more suitable for the screen feature that you can check out at:

{% content-ref url="../textual-ui/console-screen.md" %}
[console-screen.md](../textual-ui/console-screen.md)
{% endcontent-ref %}

### <mark style="color:$primary;">Normal console writers</mark>

Starting from `ConsoleWriters`, this namespace provides the below classes that can be expanded below.

<details>

<summary>Raw text writer (<code>TextWriterRaw</code>)</summary>

This class provides you with the necessary functions to allow you to write the raw text to the console either to the standard output or the standard error.

This writer provides access to plain console writers and raw console writers that allow you to write text plainly. As for the raw console writing function, it's found in the same class that you can use to print raw text to the console, including escape sequences.

</details>

<details>

<summary>Normal text writer (<code>TextWriterColor</code>)</summary>

This class provides you with the necessary functions to allow you to write the text to the console with and without color.

This writer is the simplest writer with color support, and can be used to write general text to the console effortlessly.

<figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The console writers have three variants of writing functions:

* `Write()`: Default colors
* `WriteColor()`: Foreground colors
* `WriteColorBack()`: Foreground and background colors
{% endhint %}

</details>

<details>

<summary>List entry writer (<code>ListEntryWriterColor</code>)</summary>

This class provides you with the necessary functions to let you write a list entry to the console easily.

This writer allows you to easily write one list entry with its value to the console without having to use two different functions or a long function call for writing this entry. This is internally used by the ListWriterColor writer, which is shown below.

</details>

<details>

<summary>List writer (<code>ListWriterColor</code>)</summary>

This class provides you with the necessary functions to let you write the list entries to the console easily. It supports both non-generic and generic enumerables and dictionaries.

This writer allows you to easily write list entries from either an array or an enumerable, such as lists and dictionaries, to the console without having to write loop statements. This is available in both the generic and the non-generic versions.

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Highlighted text writer (<code>TextWriterHighlightedColor</code>)</summary>

This class provides you with the necessary functions to allow you to write the highlighted text to the console with and without color.

This writer takes the colors and reverts the background and the foreground colors to make text look like as if it's highlighted.

<figure><img src="../../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
By default, the modern way of highlighting specific text using `TextWriterHighlightedColor` is enabled by using a VT sequence intended to reverse the colors. If you still want to use the legacy way, you'll have to set the `legacy` argument to `true` before passing in all the usual parameters.
{% endhint %}

</details>

<details>

<summary>Positional text writer (<code>TextWriterWhereColor</code>)</summary>

This class provides you with the necessary functions to write the text in a specific position to the console with and without color.

This text writer allows you to write text at any position to the console. This moves the cursor to the destination, writes text to the console, and, if instructed, to return to the original position that the cursor was on before the writing process.

<figure><img src="../../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Slow text writer (<code>TextWriterSlowColor</code>)</summary>

This class provides you with the necessary functions to simulate a typewriter writing a requested string to the console with and without color.

</details>

<details>

<summary>Slow positional text writer (<code>TextWriterWhereSlowColor</code>)</summary>

This class provides you with the necessary functions to simulate a typewriter that writes a text in a specific position to the console with and without color.

</details>

<details>

<summary>Wrapped pager (<code>WrappedWriter</code>)</summary>

Provides you with the necessary functions to write long text to the console, with a basic UI that allows you to skim through the text.

### <mark style="color:$primary;">Wrapped pager controls</mark>

The below wrapped pager controls are available when wrapping is enabled:

| Control      | Description                                                                     |
| ------------ | ------------------------------------------------------------------------------- |
| `ESC`        | Exits the pager                                                                 |
| `Page Up`    | Moves the output by one page backward, but stops at the beginning of the output |
| `Page Down`  | Moves the output by one page forward, but stops at the end of the output        |
| `Up Arrow`   | Moves up by one line                                                            |
| `Down Arrow` | Moves down by one line                                                          |
| `Home`       | Goes to the first page                                                          |
| `End`        | Goes to the last page                                                           |
| `Any key`    | Moves the output by one page forward and exits if it reaches end of line        |

</details>
