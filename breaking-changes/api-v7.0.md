---
description: Breaking changes for API v7.0
icon: up
---

# API v7.0

Here is a list of breaking changes that happened during the API v7.0 period when differing versions of Terminaux introduced breaking changes.

## From 6.0.x to 7.0.x

Between the 6.0.x and 7.0.x version range, we've made the following breaking changes:

### Removed all obsolete functions

{% code title="Affected classes" lineNumbers="true" %}
```csharp
public static class GraphicsTools
public static class ListWriterColor
public static class AlignedFigletTextColor
public static class AlignedTextColor
public static class BarChartColor
public static class BorderColor
public static class BorderTextColor
public static class BoxColor
public static class BoxFrameColor
public static class BreakdownChartColor
public static class CanvasColor
public static class FigletColor
public static class FigletWhereColor
public static class PowerLineColor
public static class ProgressBarColor
public static class ProgressBarVerticalColor
public static class RainbowBackTextWriterColor
public static class RainbowTextWriterColor
public static class SliderColor
public static class SliderVerticalColor
public static class StickChartColor
public static class TableColor
public static class TruncatedLineText
public static class TruncatedText
public static class KeybindingsWriter
public static class LineHandleRangedWriter
public static class LineHandleWriter
```
{% endcode %}

The above classes, which were marked as deprecated back in Terminaux 6.0, have been removed in this API revision to clean things up as we're getting ready to stabilize the cyclic writers to make them more consistent and usable than before.

{% hint style="info" %}
You'll have to use cyclic writers if you want to continue using fancy writers. You can consult [this page](../usage/console-tools/console-writers/cyclic-writers/) for more info.
{% endhint %}

### Base shell info generic class implemented

{% code title="BaseShellInfo.cs" lineNumbers="true" %}
```csharp
public abstract class BaseShellInfo : IShellInfo
```
{% endcode %}

When you create your own shell info classes to give your shell some important information, such as commands and other info, you had to override the `BaseShell` property so that it returned a new instance of your shell. However, this can be a hassle in certain scenarios.

Starting from Nitrocid KS 0.1.2 and Terminaux 7.0, you'll no longer have to override the BaseShell property because we've implemented a generic version of the above class that takes a type of your shell. For example, if your shell is called `OxygenShell` and your shell info class is called `OxygenShellInfo`, you can just inherit the generic version of the class so that it becomes `BaseShellInfo<OxygenShell>`.

As a result, the following properties are removed from the IShellInfo interface:

{% code title="IShellInfo.cs" lineNumbers="true" %}
```csharp
BaseShell? ShellBase { get; }
PromptPresetBase CurrentPreset { get; }
```
{% endcode %}

{% hint style="info" %}
When inheriting the non-generic base shell class, your shell info class might hold wrong information about your shell, even if your commands are defined. Therefore, you must migrate to the generic version of the class if you want to retain your shell settings.
{% endhint %}

### Cyclic writers now use typed `CyclicWriter` classes

The cyclic writers have been redefined to make their classes use the typed `CyclicWriter` classes. These classes consist of:

* `SimpleCyclicWriter`
* `GraphicalCyclicWriter`

This was required because we needed applications to be able to determine whether the writer that they're using was a simple one or a graphical one. Simple cyclic writers are suitable for command line applications, while graphical ones are used in TUIs.

The following writers have been migrated to `SimpleCyclicWriter`:

* `Decoration`
* `Emoji`
* `FigletText`
* `Kaomoji`
* `KeyShortcut`
* `Keybindings`
* `LineHandle`
* `ListEntry`
* `Listing`
* `NerdFonts`
* `PowerLine`
* `ProgressBar`
* `ProgressBarNoText`
* `SimpleProgress`
* `Slider`
* `Spinner`
* `StemLeafChart`
* `TextException`
* `TextMarquee`

The following writers have been migrated to `GraphicalCyclicWriter`:

* `AlignedFigletText`
* `AlignedText`
* `AnimatedCanvas`
* `AnimatedText`
* `BarChart`
* `Border`
* `BoundedText`
* `Box`
* `BoxFrame`
* `BreakdownChart`
* `Calendars`
* `Canvas`
* `Eraser`
* `Line`
* `LineChart`
* `LinesChart`
* `PieChart`
* `Selection`
* `StickChart`
*  `SyntaxText`
* `Table`
* `TextPath`
* `WinsLosses`

The following shape renderers have been migrated to `GraphicalCyclicWriter`:

* `Arc`
* `Circle`
*  `Ellipsis`
* `Parallelogram`
* `Rectangle`
* `Square`
* `Trapezoid`
* `Triangle`

Part of the introduction of the two typed cyclic writer classes involved removing both `IStaticRenderable` and `IGeometricShape` interfaces to reduce the complexity involved.

{% hint style="info" %}
Consult the updated cyclic writer documentation for more information.
{% endhint %}

### Changed left and right margins to widths in some graphical writers

{% code title="Some graphical writers" lineNumbers="true" %}
```csharp
public int LeftMargin
{
    get => leftMargin;
    set => leftMargin = value;
}

public int RightMargin
{
    get => rightMargin;
    set => rightMargin = value;
}
```
{% endcode %}

We have replaced the two above properties with the `Width` property that some graphical writers that are derived from `GraphicalCyclicWriter` use to determine the element width. The following writers are affected:

* `FigletText`
* `ProgressBar`
* `ProgressBarNoText`
* `SimpleProgress`
* `TextMarquee`

{% hint style="info" %}
You'll have to calculate the total width that the affected writers need to render to the terminal.
{% endhint %}

### `Keybindings` and `StemLeafChart` writers now act as a simple writer

{% code title="Keybindings.cs + StemLeafChart.cs" lineNumbers="true" %}
```csharp
public int Left { get; set; }
public int Top { get; set; }
```
{% endcode %}

The two above properties have been removed because simple writers are not supposed to use anything related to positioning.

{% hint style="info" %}
You can still use the rendering tools functions to accommodate your needs, should you use positioning to write these.
{% endhint %}

### Moved `WriteRenderable()` and `RenderRenderable()` to `RendererTools`

We have moved all `WriteRenderable()` and `RenderRenderable()` function overloads from `ContainerTools` to `RendererTools` to better convey the goals as we have created the two base cyclic writer classes.

### Migrated `InfoBoxInputPasswordColor` to `InfoBoxInputColor`

{% code title="InfoBoxInputPasswordColor.cs" lineNumbers="true" %}
```csharp
public static class InfoBoxInputPasswordColor
```
{% endcode %}

We've migrated the two classes into one by adding functions labelled with `Password`, such as `WriteInfoBoxInputPassword()`, to the `InfoBoxInputColor` to reduce maintenance burden and to avoid code repetition.

{% hint style="info" %}
You'll have to reference the same functions but with the `InfoBoxInputColor` class.
{% endhint %}

### Removed `SplitVTSequencesMultiple()`

{% code title="VtSequenceTools.cs" lineNumbers="true" %}
```csharp
public static string[] SplitVTSequencesMultiple(string Text, VtSequenceType types = VtSequenceType.All)
```
{% endcode %}

As `SplitVTSequencesMultiple()` provides the same functionality as `SplitVTSequences()` with the target type set to `All`, we've seen the above function as redundant; therefore, we've removed it.

{% hint style="info" %}
Replace all calls to the removed function with `SplitVTSequences()`.
{% endhint %}

### Removed `SliderInside` from `Selection`

{% code title="Selection.cs" lineNumbers="true" %}
```csharp
public bool SliderInside { get; set; }
```
{% endcode %}

As development of Terminaux 7.0 continues, we've seen the above property as redundant due to the fact that it was buggy. We've removed this property to make the selection implementation more simple. This removal was part of the broader mouse improvements that were done in this version of Terminaux.

{% hint style="info" %}
You'll have to manually adjust the left position to add `1`, depending on the look of your application. If you're using this property, you'll have to increment the left position. Otherwise, you don't have to.
{% endhint %}

### Presentation system now uses the input modules

{% code title="InputInfo.cs" lineNumbers="true" %}
```csharp
public class InputInfo
```
{% endcode %}

Input modules have been implemented, and they are stable enough that the presentation input elements are actually duplicates of the broader input module classes. As a result, we've decided to eliminate the presentation-specific input elements and to rename the `InputInfo` class to `PresentationInputInfo` to avoid ambiguity. As a result, the following input methods have been removed together with the corresponding interface, `IInputMethod`, and the base class, `BaseInputMethod`:

* `MaskedTextInputMethod`
* `SelectionInputMethod`
* `SelectionMultipleInputMethod`
* `TextInputMethod`

This is done as a result of the addition of a new infobox that uses the input modules.

{% hint style="info" %}
You'll have to replace all definitions of the older `InputMethod` classes with the newer `InputModule` classes. The following input methods can be replaced with:

* `MaskedTextInputMethod` -> `MaskedTextBoxModule`
* `SelectionInputMethod` -> `ComboBoxModule`
* `SelectionMultipleInputMethod` -> `MultiComboBoxModule`
* `TextInputMethod` -> `TextBoxModule`
{% endhint %}

### Refactored CIE color models

{% code title="Cie*Full.cs" lineNumbers="true" %}
```csharp
public class CieLabFull : BaseColorModel, IEquatable<CieLabFull> {}
public class CieLchFull : BaseColorModel, IEquatable<CieLchFull> {}
public class CieLuvFull : BaseColorModel, IEquatable<CieLuvFull> {}
```
{% endcode %}

We've refactored the CIE color model classes so that both the limited version and the full version are migrated to one class without the `Full` suffix. This is to reduce confusion and to improve the overall codebase when it comes to color models.

Functions that were suffixed with `Full` were also migrated in the same manner as the affected classes.

{% hint style="info" %}
You'll have to use the functions and the classes of CIE-Lab, CIE-Lch, and CIE-Luv without the `Full` suffix.
{% endhint %}

### Refactored the input management code

{% code title="Input.cs" lineNumbers="true" %}
```csharp
public static bool KeyboardInputAvailable
public static bool MouseInputAvailable
public static bool InputAvailable
```
{% endcode %}

Improved mouse support on Linux means that we'll have to enable non-block I/O operations on the `stdin` stream, which means removing the "poll" operation that uses the `ioctl()` function. However, this approach allowed the mouse escape sequences to "leak" to the console, which is unfeasible and could cause graphical glitches.

As a result, we've decided to ditch the read-twice design and go for the read-once design in a single function, `ReadPointerOrKey()`, while maintaining the async version of the function, called `ReadPointerOrKeyNoBlock()`. This is further improved by introducing our own event stack, which takes the most recent event received from each type.

{% hint style="info" %}
You need to switch the loop code from the below model:

```csharp
SpinWait.SpinUntil(() => Input.InputAvailable);
if (Input.MouseInputAvailable)
{
    // Mouse input received.
    var mouse = Input.ReadPointer();
    if (mouse is null)
        continue;
        
    // Process mouse input here...
}
else if (ConsoleWrapper.KeyAvailable && !Input.PointerActive)
{
    var key = Input.ReadKey();
    
    // Process keyboard input here...
}
```

...to this:

```csharp
var (mouse, key) = Input.ReadPointerOrKey();
if (mouse is not null)
{
    // Process mouse input here...
}
else if (key is ConsoleKeyInfo cki && !Input.PointerActive)
{
    // Process keyboard input here...
}
```
{% endhint %}

### Removed icon quality, keeping the normal version

{% code title="IconsQuality.cs" lineNumbers="true" %}
```csharp
public enum IconsQuality
```
{% endcode %}

We've removed the icons quality enumeration because it's useful only in actual GUIs. As for the console, the icon quality doesn't matter, so we've removed the icons quality enumeration, which causes all the functions in the `IconsManager` class to no longer hold the option parameter, `quality`.

{% hint style="danger" %}
There is no alternative for this enum, and we're planning to roll out this change to Terminaux 6.1.x and older.
{% endhint %}

### Aligned text writer improved

{% code title="AlignedText.cs and AlignedFigletText.cs" lineNumbers="true" %}
```csharp
public int LeftMargin
public int RightMargin
public bool OneLine (AlignedFigletText)
```
{% endcode %}

The aligned text writer and the figlet counterpart have been improved to get rid of the old margin system that has been causing problems in certain situations. We shouldn't assume that such text is written in the center of the console, so we've decided to make a radical change in `AlignedText` to reflect that in the figlet counterpart.

{% hint style="info" %}
You can use the `Left` and the `Width` properties to replace the older `LeftMargin` and `RightMargin` properties.
{% endhint %}

### VT sequence tools refactored

{% code title="VtSequenceTools.cs" lineNumbers="true" %}
```csharp
public static (VtSequenceType type, Match[] matches)[] MatchVTSequences(string Text, VtSequenceType type = VtSequenceType.All)
public static Dictionary<VtSequenceType, bool> IsMatchVTSequencesSpecific(string Text, VtSequenceType type = VtSequenceType.All)
```
{% endcode %}

We have made several refactors that made the VT sequence tools use the new VT sequence tokenizer instead of the regular expression method that we had first introduced around Q3 2022. We are now using read-only dictionaries to make sure that you get solid results, while maintaining performance.

As a result, we've removed all VT sequence tools code that have to do with the regular expressions. If you still want a list of regular expressions for some reason, you'll be able to find them [here](https://github.com/Aptivi/Terminaux/blob/x/exp/ng-te-b3/private/Terminaux.SequenceTypesGen/Resources/sequences.json).

```csharp
// The following classes have been removed:
public static class VtSequenceRegexes {}

// These were removed:
public static class TYPESequences
{
    public static Regex SEQSequenceRegex { get; }
}

public static partial class VtSequenceTools
{
    public static partial Regex GetSequenceFilterRegexFromType(VtSequenceType type) {}
    public static Regex GetSequenceFilterRegexFromType() {}
    public static Regex GetSequenceFilterRegexFromTypes(VtSequenceType types = VtSequenceType.All) {}
}

public static class VtSequenceBuilderTools
{
    public static (VtSequenceType, VtSequenceSpecificTypes) DetermineTypeFromSequence(string sequence) {}
}
```

{% hint style="info" %}
For increased flexbility, `MatchVTSequences()` and `IsMatchVTSequencesSpecific()` both return a full dictionary that contain info about every single sequence type, even if there are no entries. This makes sure that you can now safely use the indexer in the resulting variable without worrying about its existence.
{% endhint %}

### Shell features backported from Nitrocid

The initial migration of the UESH shell started when working on Terminaux 6.0. Since then, the Nitrocid shell has been evaluated for the second time, and found that we needed to make the shell more complete and consistent with Nitrocid and other Terminaux applications.

As a result, we've added new features to Terminaux's shell implementation. However, for this shell to work properly, we've changed how the console wrapper worked by replacing the action-based implementation with the class-based one that doesn't use the actions. Because of the design change, we've removed all necessary properties from the `ConsoleWrapperTools` class.

In addition to that, Nitrocid's codebase has been stripped to meet this requirement by removing Nitrocid's implementation since the Terminaux one has almost all the features.

### Removed color templates to precede the themes feature

```csharp
public class TemplateInfo { }
public static class TemplateTools { }
```

The color template feature introduced in Terminaux wasn't meeting all the requirements of Nitrocid, so we've decided to move the feature over from Nitrocid to Terminaux as-is, removing the legacy templates feature in the process.

{% hint style="info" %}
Use the improved themes feature to take advantage of all the improvements that the writers have undergone.
{% endhint %}

### Removed legacy argument parsing

{% code title="ArgumentParse.cs" lineNumbers="true" %}
```csharp
public static void ParseArgumentsLegacy(string[]? ArgumentsInput, Dictionary<string, ArgumentInfo> arguments)
public static bool IsArgumentPassedLegacy(string[]? ArgumentsInput, string argumentName, Dictionary<string, ArgumentInfo> arguments)
```
{% endcode %}

As we've improved the methodology of processing arguments that are passed to the Terminaux application using the `--` prefix, the legacy argument parsing relied on processing them in the same way without said prefix. As this could cause issues and confusion, we've removed the legacy argument parsing.

{% hint style="info" %}
Use `ParseArguments()` and `IsArgumentPassed()` functions instead.
{% endhint %}

### Removed hex and text viewer classes

```csharp
public static class HexViewInteractive { }
public static class TextViewInteractive { }
```

The hex and text editors have undergone many recent improvements to the point that having the viewer felt like repeating the code. In order to reduce double effort, we've decided to remove the two classes in preparation for the final release of Terminaux 7.0.

{% hint style="info" %}
You can still open the viewer, but use the `HexEditInteractive` and `TextEditInteractive` respectively with passing `false` to the `edit` argument.
{% endhint %}

### Removed obsolete infobox functions

```csharp
public static int WriteInfoBoxButtonsPlain(InputChoiceInfo[] buttons, string text, params object[] vars) =>
public static int WriteInfoBoxButtonsPlain(InputChoiceInfo[] buttons, string text, BorderSettings settings, params object[] vars) =>
public static int WriteInfoBoxButtonsColor(InputChoiceInfo[] buttons, string text, Color InfoBoxButtonsColor, params object[] vars) =>
public static int WriteInfoBoxButtonsColorBack(InputChoiceInfo[] buttons, string text, Color InfoBoxButtonsColor, Color BackgroundColor, params object[] vars) =>
public static int WriteInfoBoxButtons(InputChoiceInfo[] buttons, string text, BorderSettings settings, params object[] vars) =>
public static int WriteInfoBoxButtonsColor(InputChoiceInfo[] buttons, string text, BorderSettings settings, Color InfoBoxButtonsColor, params object[] vars) =>
public static int WriteInfoBoxButtonsColorBack(InputChoiceInfo[] buttons, string text, BorderSettings settings, Color InfoBoxButtonsColor, Color BackgroundColor, params object[] vars) =>
public static int WriteInfoBoxButtonsPlain(string title, InputChoiceInfo[] buttons, string text, params object[] vars) =>
public static int WriteInfoBoxButtonsPlain(string title, InputChoiceInfo[] buttons, string text, BorderSettings settings, params object[] vars) =>
public static int WriteInfoBoxButtons(string title, InputChoiceInfo[] buttons, string text, params object[] vars) =>
public static int WriteInfoBoxButtonsColor(string title, InputChoiceInfo[] buttons, string text, Color InfoBoxTitledButtonsColor, params object[] vars) =>
public static int WriteInfoBoxButtonsColorBack(string title, InputChoiceInfo[] buttons, string text, Color InfoBoxTitledButtonsColor, Color BackgroundColor, params object[] vars) =>
public static int WriteInfoBoxButtons(string title, InputChoiceInfo[] buttons, string text, BorderSettings settings, params object[] vars) =>
public static int WriteInfoBoxButtonsColor(string title, InputChoiceInfo[] buttons, string text, BorderSettings settings, Color InfoBoxTitledButtonsColor, params object[] vars) =>
public static int WriteInfoBoxButtonsColorBack(string title, InputChoiceInfo[] buttons, string text, BorderSettings settings, Color InfoBoxTitledButtonsColor, Color BackgroundColor, params object[] vars) =>

// ...and other similar functions in all infobox classes
```

We have introduced `InfoBoxSettings`. As a result, we've decided to remove all those functions so that you can be more expressive in describing what setting to edit and to minimize ambiguity for arguments. Also, the effort will be reduced to just adding a property and using it.

{% hint style="info" %}
Move the arguments to their relevant fields by creating a new instance of `InfoBoxSettings` and using the properties to assign their correct values. For example, `InfoBoxSettings` contains a property called `Title` that you can use to set the title.
{% endhint %}
