---
description: How can you extend your console?
icon: plus
---

# Console Extensions

Terminaux not only provides basic console tools to make building your console applications easier, but you can also rely on the extensions provided below to maximize your productivity. Any extension that is not documented can be referred to in the API documentation below:

{% embed url="https://aptivi.github.io/Terminaux/api/Terminaux.Base.Extensions.html" %}

{% hint style="info" %}
Certain features in this section may not work for different terminal emulators. For such terminal emulators, you may see nothing changing or something completely different happening. Use this feature accordingly.

The demo of Terminaux provides tests for some of the extensions, so you can check your console for support.
{% endhint %}

***

## <mark style="color:$primary;">List of extensions</mark>

Here is the list of some of the extensions that are documented below:

<details>

<summary>Cursor tools</summary>

The `ConsoleCursor` class contains extensions that allow you to change the cursor in the terminal, including making the cursor visible or hidden and changing the cursor shape. The following tools are available:

<table><thead><tr><th width="139.6666259765625">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>CursorVisible</code></td><td>Specifies whether the cursor is visible or not.</td></tr><tr><td><code>CursorType</code></td><td>Specifies the cursor type, like blinking or steady block, underscore, or vertical bar.</td></tr></tbody></table>

</details>

<details>

<summary>Clearing the console</summary>

`ConsoleClearing` provides you with tools to ease clearing the console and resetting its state. You can use the following functions:

<table><thead><tr><th width="279.66668701171875">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>ClearLineToRight()</code></td><td>Allows you to clear the line to the right using the current background color in the current position.</td></tr><tr><td><code>GetClearLineToRightSequence()</code></td><td>Gets a sequence that clears the line to the right using the current background color in the current position.</td></tr><tr><td><code>ResetAll()</code></td><td>Resets the entire console.</td></tr><tr><td><code>GetClearWholeScreenSequence()</code></td><td>Gets a sequence that clears the whole display, including the scrollback buffer.</td></tr></tbody></table>

</details>

<details>

<summary>Console positioning</summary>

In the `ConsolePositioning` class, you can manipulate with the cursor position in various ways, such as calling the below functions that do different things:

<table><thead><tr><th width="219.66668701171875">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>ClearKeepPosition()</code></td><td>Allows you to clear the console and keep the cursor position.</td></tr><tr><td><code>GetFilteredPositions()</code></td><td>Allows you to get the filtered positions (X, Y) after filtering the specified text from the VT sequence in the current cursor position by simulating write operation.</td></tr></tbody></table>

</details>

<details>

<summary>Console formatting</summary>

The `ConsoleFormatting` class provides you with text formatting tools, including making the text bold, italicize it, and more. This affects all text that have been written to the console inside and outside Terminaux. Fortunately, we have two modes:

* Dry setting: You'll have to call the function `GetFormattingSequences()` in the text sequence that you want to write to any writer. After that, you'll have to reset the formatting by hand.
* Non-dry setting: You'll have to call the function `SetFormatting()` to affect all text writes. If you want to reset all formatting, you'll have to call `ResetFormatting()`.

{% hint style="info" %}
Your terminal emulator may not support all the formatting modes, so consult your terminal's manual to verify that all the formats supported by Terminaux are supported. Additionally, the Terminaux demo app provides a test facade, `TestFormatting`, that allows you to test your console for support of all the text formatting options.
{% endhint %}

</details>

<details>

<summary>Console characters</summary>

You can also manipulate with the console characters using the following functions:

<table><thead><tr><th width="219.66668701171875">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>EstimateCellWidth()</code></td><td>Estimates the cell width that a string takes.</td></tr><tr><td><code>EstimateZeroWidths()</code></td><td>Gets how many characters whose width is zero.</td></tr><tr><td><code>EstimateFullWidths()</code></td><td>Gets how many characters whose width is full.</td></tr><tr><td><code>TabWidth</code></td><td>Gets the tab width</td></tr></tbody></table>

{% hint style="info" %}
Please note that this information doesn't indicate the string length either by the amount of UTF-8 characters or by the text element as `StringInfo` class returns. This indicates how many console grid cells a character or a sentence consumes.
{% endhint %}

### <mark style="color:$primary;">Tab width</mark>

You can set the tab width to replace the actual tab characters with the needed amount of space characters to render using the `TabWidth` property. The default tab width is set to 4.

</details>

<details>

<summary>Console taskbar progress</summary>

You can make your console application show a progress bar in your taskbar using the `ConsoleTaskbarProgress` class that provides the following functions:

<table><thead><tr><th width="219.66668701171875">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>SetProgress()</code></td><td>Shows a progress indicator in the taskbar with a specified number and a maximum number (signed positive number)</td></tr><tr><td><code>SetProgressUnsigned()</code></td><td>Shows a progress indicator in the taskbar with a specified number and a maximum number (unsigned positive number)</td></tr></tbody></table>

The progress status, `ConsoleTaskbarProgressEnum`, can be described as follows:

<table><thead><tr><th width="89.99993896484375">Visual</th><th width="140">Status</th><th>Description</th></tr></thead><tbody><tr><td><img src="../../.gitbook/assets/image (81).png" alt=""></td><td><code>Normal</code></td><td>Indicates that there is progress, and the progress bar in the taskbar fills up according to the specified value.</td></tr><tr><td><img src="../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td><code>Error</code></td><td>Indicates that there is an error when processing, and the progress bar in the taskbar will be shown as red.</td></tr><tr><td><img src="../../.gitbook/assets/image (1) (1).png" alt="" data-size="original"></td><td><code>Paused</code></td><td>Indicates that there is a paused operation, and the progress bar in the taskbar will be shown as yellow.</td></tr><tr><td><img src="../../.gitbook/assets/image (2) (1).png" alt=""></td><td><code>Indeterminate</code></td><td>Indicates that there is progress that can't be determined, and the progress bar will show as a marquee in the taskbar.</td></tr><tr><td><img src="../../.gitbook/assets/image (3).png" alt=""></td><td><code>NoProgress</code></td><td>Indicates that there is no progress, and the progress bar in the taskbar will be hidden.</td></tr></tbody></table>

{% hint style="warning" %}
This feature is only available for applications running on Windows 7 or higher. The functions that pertain to this feature do nothing when being called on systems older than Windows 7 and on non-Windows systems.
{% endhint %}

</details>

<details>

<summary>Console acoustic tools</summary>

You can also use the acoustic tools for your console, such as beep synth features that were originally implemented on Nitrocid KS. For beep synths, you must call `GetSynthInfo()` before `PlaySynth()` once you're sure that you have a valid beep synth JSON representations that looks like the following:

{% code expandable="true" %}
```json
{
    "name": "Test synth info",
    "chapters": [
        {
            "name": "Chapter 1",
            "synths": [
                "128 100",
                "256 200",
                "512 300",
                "256 400",
                "128 500",
            ]
        },
        {
            "name": "Chapter 2",
            "synths": [
                "256 100",
                "512 200",
                "1024 300",
                "512 400",
                "256 500",
            ]
        },
    ]
}
```
{% endcode %}

As for audio cues, you can consult the following page:

<a href="../input-reader/other-input/audio-cues.md" class="button primary">Audio Cues</a>

</details>

<details>

<summary>Console mode</summary>

You can adjust the console mode by using the following functions:

<table><thead><tr><th width="139">Tool</th><th>Description</th></tr></thead><tbody><tr><td><code>IsRaw</code></td><td>Checks to see if the Linux console is in raw mode.</td></tr><tr><td><code>EnableRaw()</code></td><td>Enables the raw mode.</td></tr><tr><td><code>DisableRaw()</code></td><td>Disables the raw mode.</td></tr></tbody></table>

{% hint style="info" %}
In Linux consoles, you can't use `EnableRaw()` and `DisableRaw()` functions if mouse support is enabled as the way the mouse events are handled require raw mode because they get injected into the `stdin` stream.
{% endhint %}

</details>

<details>

<summary>Miscellaneous console extensions</summary>

The following extensions that don't fit in any of the categories can be used in your applications:

* `SetTitle()`
* `PercentRepeat()`
* `PercentRepeatTargeted()`
* `FilterVTSequences()`
* `GetWrappedSentences()`
* `GetWrappedSentencesByWords()`
* `Truncate()`
* `ShowMainBuffer()`
* `ShowAltBuffer()`
* `PreviewMainBuffer()`
* `PreviewAltBuffer()`
* `Flash()` and `Flash(int)`
* `Bell()`
* `IsOnAltBuffer`
* `SetEncoding()` (Windows only)

</details>
