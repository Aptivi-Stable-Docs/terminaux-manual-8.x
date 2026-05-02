---
description: Breaking changes for API v8.0
icon: up
---

# API v8.0

Here is a list of breaking changes that happened during the API v8.0 period when differing versions of Terminaux introduced breaking changes.

***

## <mark style="color:$primary;">From 7.0.x to 8.0.x</mark>

Between the 7.0.x and 8.0.x version range, we've made the following breaking changes:

<details>

<summary>BassBoom library path settings moved</summary>

{% code title="TermReaderSettings.cs" lineNumbers="true" %}
```csharp
public string BassBoomLibraryPath
{
    get => bassBoomLibraryRoot ?? "";
    set => bassBoomLibraryRoot = value;
}
```
{% endcode %}

The BassBoom library path is supposed to be set once, because BassBoom loads the library once before being functional. To reflect BassBoom's behavior, we've decided to move the above property to the static class of `Input`.

{% hint style="info" %}
Update all references to this property to use the new location.
{% endhint %}

</details>

<details>

<summary>General console checker removed</summary>

{% code title="ConsoleChecker.cs" lineNumbers="true" %}
```csharp
public static void CheckConsole() { }
public static void AddToCheckWhitelist(Assembly asm) { }
public static void RemoveFromCheckWhitelist(Assembly asm) { }
```
{% endcode %}

We have removed the general console checker as part of an ongoing effort to properly support file redirects and pipes for Terminaux CLI applications that use the provided console writing functions that support color. This will allow your application to use those functions without having to write a separate code path for dumb consoles.

{% hint style="danger" %}
If you are still using the console checker, you'll either have to downgrade to Terminaux 7.x, or you'll have to re-implement the checker yourself according to your requirements.
{% endhint %}

</details>

<details>

<summary>Console filter removed</summary>

{% code title="ConsoleFilter*.cs" lineNumbers="true" %}
```csharp
public static class ConsoleFilter { }
public class ConsoleFilterInfo { }
public enum ConsoleFilterSeverity { }
public enum ConsoleFilterType { }
```
{% endcode %}

We have removed the console filter feature as part of an ongoing effort to properly support file redirects and pipes for Terminaux CLI applications that use the provided console writing functions that support color. This will allow your application to use those functions without having to write a separate code path for dumb consoles.

{% hint style="danger" %}
If you are still using the console filter, you'll either have to downgrade to Terminaux 7.x, or you'll have to re-implement the filter yourself according to your requirements.
{% endhint %}

</details>

***

## <mark style="color:$primary;">From 8.0.x to 8.1.x</mark>

Between the 8.0.x and 8.1.x version range, we've made the following breaking changes:

<details>

<summary>Decoupling color class prepared</summary>

```csharp
// Moved to ColorExtensions as functions
public string VTSequenceForeground { get; set; }
public string VTSequenceForegroundOriginal { get; set; }
public string VTSequenceForegroundTrueColor { get; set; }
public string VTSequenceBackground { get; set; }
public string VTSequenceBackgroundOriginal { get; set; }
public string VTSequenceBackgroundTrueColor { get; set; }
```

```csharp
// Moved to ConsoleColoring from ColorTools for Colorimetry
public static Color CurrentForegroundColor { get; }
public static Color CurrentBackgroundColor { get; }
public static bool ConsoleSupportsTrueColor { get; set; }
public static bool AllowBackground { get; set; }
public static void LoadBack() { }
public static void LoadBack(Color ColorSequence, bool Force = false) { }
public static void LoadBackDry() { }
public static void LoadBackDry(Color ColorSequence, bool Force = false) { }
public static Color GetGray(ColorContrastType contrastType = ColorContrastType.Light) { }
public static void SetConsoleColor(Color ColorSequence, bool Background) { }
public static void SetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSet = true) { }
public static bool TrySetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSet = true) { }
public static void SetConsoleColorDry(Color ColorSequence, bool Background) { }
public static void SetConsoleColorDry(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSet = true) { }
public static bool TrySetConsoleColorDry(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSet = true) { }
public static string RenderSetConsoleColor(Color ColorSequence) { }
public static string RenderSetConsoleColor(Color ColorSequence, bool Background) { }
public static string RenderSetConsoleColor(Color ColorSequence, bool Background = false, bool ForceSet = false, bool canSet = true) { }
public static void ResetColors() { }
public static void ResetForeground() { }
public static void ResetBackground() { }
public static string RenderResetColors() { }
public static string RenderResetForeground() { }
public static string RenderResetBackground() { }
public static void RevertColors() { }
public static void RevertForeground() { }
public static void RevertBackground() { }
public static string RenderRevertColors() { }
public static string RenderRevertForeground() { }
public static string RenderRevertBackground() { }
public static void DetermineTrueColorFromUser() { }
```

To prepare the Color class for decoupling, we've managed to move all Terminaux-specific functions and properties, alongside the functionality, to independent classes that Terminaux applications can rely on. Those classes extend Colorimetry's original color class that was taken straight from Terminaux after the preparation stage.

{% hint style="info" %}
You'll need to update all references to those functions to point to the new class. Additionally, if you're using `VTSequence*` properties, they have now become extension functions, which means that you'll have to add the `()` marks at the end of each call.
{% endhint %}

</details>

<details>

<summary>Moved the themes system to <code>Terminaux.Themes</code></summary>

```csharp
namespace Terminaux.Colors.Themes { }
namespace Terminaux.Colors.Themes.Colors { }
```

As the themes system is not going to be part of Colorimetry due to large portions of the theme system using Terminaux-specific console features, we've decided to move the themes system and their classes to a new namespace, which is `Terminaux.Themes`.

{% hint style="info" %}
You'll need to update all using clauses to point to `Terminaux.Themes` and `Terminaux.Themes.Colors`.
{% endhint %}

</details>

<details>

<summary>Consistency in selection style TUI indexes</summary>

In the `SelectionStyle` class, we've made the experience more consistent by letting all returned answer numbers in the single-selection style TUIs become zero-based.

This was because the multiple selection style TUI used zero-based numbers, while the single selection style TUI originally used the one-based answer numbers, and this traces back to when Nitrocid 0.0.20.0 was under early development on October 2021 as per [this commit](https://github.com/Aptivi/Nitrocid/commit/9b6c35515d1ad2b5d9397e0a752b845c21f7a08d).

Now, we've cleaned that up by making the numbers become indexes, thus making them zero-based.

{% hint style="warning" %}
You'll need to adjust the code to handle the new logic. For example, you used to subtract 1 from the resultant number (for example, `selected - 1`). Now, you don't subtract the number itself (for example, `selected`).
{% endhint %}

</details>

***

## <mark style="color:$primary;">From 8.1.x to 8.2.x</mark>

Between the 8.1.x and 8.2.x version range, we've made the following breaking changes:

<details>

<summary>Moved color tools to Colorimetry</summary>

We've moved all color tools and classes to the Colorimetry library as a result of the previous changes made to Terminaux in v8.1 during the decoupling process. This allows non-Terminaux apps to use the color features that were originally found within Terminaux.

You can consult the Colorimetry library documentation here:

<a href="https://app.gitbook.com/o/fj052nYlsxW9IdL3bsZj/s/BdESDsiuTO9fbDXLJ8HV/" class="button primary">Colorimetry</a>

{% hint style="info" %}
In order to continue using color tools and classes, you'll need to use the Colorimetry library.
{% endhint %}

</details>
