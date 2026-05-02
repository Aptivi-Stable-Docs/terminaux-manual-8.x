---
description: Console color tools here!
icon: palette
---

# Console Colors

In addition to all of the Terminaux's features, we also provide you with a rich class, `ConsoleColoring`, for console color tools. With this class, you can:

* Set the console color
* Set the console color dryly
* ...and more

{% hint style="info" %}
Please note that some consoles don't support 256 colors and/or 16-bit colors, and some of them might implement these poorly (Terminal.app (`Apple_Terminal`) for example).

Support for 16-bit colors and 256-colors requires a compatible terminal emulator.

* Windows systems: ConEmu, Windows 10 cmd.exe with VT sequences enabled
* Linux systems: xterm, GNOME Terminal, Konsole, etc.
* macOS systems: iTerm2 only
{% endhint %}

For more color functions and to get started making the color sequences, you can consult the below page from Colorimetry:

{% content-ref url="https://app.gitbook.com/s/BdESDsiuTO9fbDXLJ8HV/usage/color-sequences" %}
[Color Sequences](https://app.gitbook.com/s/BdESDsiuTO9fbDXLJ8HV/usage/color-sequences)
{% endcontent-ref %}

***

## <mark style="color:$primary;">Current colors</mark>

The console tools class provides you with two properties that allow you to get the current foreground and the background color.

{% code title="ConsoleColoring.cs" lineNumbers="true" %}
```csharp
public static Color CurrentForegroundColor
public static Color CurrentBackgroundColor
```
{% endcode %}

***

## <mark style="color:$primary;">Color setting</mark>

The console tools class provides you with two functions that set the console foreground and background colors both dryly and permanently.

{% code title="ConsoleColoring.cs" lineNumbers="true" %}
```csharp
public static void SetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSetBackground = true) { }
public static void SetConsoleColorDry(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSetBackground = true) { }
```
{% endcode %}

The first function sets either the console's foreground color or the console's background color permanently. The changes will be done and you can verify this using either `CurrentForegroundColor` or `CurrentBackgroundColor`.

The second function, however, only runs dryly and doesn't set any of the abovementioned properties. This allows you to temporarily set the background or the foreground color.

{% hint style="info" %}
If you want to dryly set the console color, you must either use the plain writers, or use the `Color` instances to apply them to all the `Color`-related parameters.

Currently, `AllowBackground` is set to `false`, which means that background colors are disabled, unless forced. To enable background colors globally, you must enable it.
{% endhint %}

***

## <mark style="color:$primary;">Background loading</mark>

The console color tools class also provides you with background loading functions that allow you to quickly clear the console with the selected background color.

{% code title="ConsoleColoring.cs" lineNumbers="true" %}
```csharp
public static void LoadBack() { }
public static void LoadBack(Color ColorSequence, bool Force = false) { }
public static void LoadBackDry() { }
public static void LoadBackDry(Color ColorSequence, bool Force = false) { }
```
{% endcode %}

The non-dry `LoadBack()` functions allow you to load the background color and permanently set the console background color to either the already-set current background color or to your specified background color. However, the dry version allows you to load the background color temporarily.

{% hint style="info" %}
If you want to dryly set the background color, you must either use the plain writers, or use the background `Color` instances to apply them to all the background color-related parameters.
{% endhint %}

***

## <mark style="color:$primary;">Sequence Initialization</mark>

If you have a Windows system, you can call a function that allows you to initialize the VT sequences. This function in the `ConsoleExtensions` class is called `InitializeSequences()`.

{% hint style="info" %}
If you have already set the VT sequence processing using a registry key found in `HKEY_CURRENT_USER\Console\VirtualTerminalLevel`, this function does nothing.
{% endhint %}

***

## <mark style="color:$primary;">Resetting the colors and the console</mark>

Terminaux provides a console extension that allows you to perform a hard reset using the two VT sequences. When invoked, the terminal will perform two resets:

* Full reset (ESC sequence)
* Soft reset (CSI sequence)

After the reset is done, the screen will be cleared with all the colors reverted to their initial state. The signature is defined below:

```csharp
public static void ResetAll() { }
```

### <mark style="color:$primary;">Resetting the colors</mark>

You can also reset the colors either fully or selectively (foreground or background) by calling one of the following functions:

* `ResetColors()`: Resets both the foreground and the background colors.
* `ResetForeground()`: Resets the foreground color.
* `ResetBackground()`: Resets the background color.

If you wish to dryly set the colors on plain writers or on string builders, you can use the `RenderSetConsoleColor()` function, but you'll have to append one of the following:

* `RenderResetColors()`
* `RenderResetForeground()`
* `RenderResetBackground()`

### <mark style="color:$primary;">Reverting the colors</mark>

Reverting the colors to their currently set colors monitored by Terminaux can be done easily by calling one of the following functions:

* `RevertColors()`: Reverts both the foreground and the background colors.
* `RevertForeground()`: Reverts the foreground color.
* `RevertBackground()`: Reverts the background color.

If you wish to dryly set the colors on plain writers or on string builders, and you want to revert to colors that Terminaux set, you can use the `RenderSetConsoleColor()` function, but you'll have to append one of the following:

* `RenderRevertColors()`
* `RenderRevertForeground()`
* `RenderRevertBackground()`
