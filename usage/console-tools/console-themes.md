---
description: Coloring stuff...
icon: fill-drip
---

# Console Themes

Themes consist of color information for each color type. They are made with a simple JSON syntax that's easy to use.

***

## <mark style="color:$primary;">Theme structure</mark>

In order to make your own theme, you'll have to understand the theme structure and how it gets serialized to the theme object.

<details>

<summary>Theme JSON structure</summary>

Here's the format of each theme JSON file:

{% code expandable="true" %}
```json
{
    "Metadata": {
        "Name": "Theme name",
        "Description": "This is my theme",
        "IsEvent": false,
        "StartMonth": 2,
        "StartDay": 1,
        "EndMonth": 2,
        "EndDay": 22,
        "Calendar": "Gregorian",
        "Category": "Exciting",
        "UseAccentTypes": [
            "LicenseColor",
            (...)
        ],
    },
    "InputColor": "15",
    "LicenseColor": "15",
    "ContKernelErrorColor": "15",
    (...)
}
```
{% endcode %}

We'll explain things one by one. The `Metadata` key consists of some basic information about the theme, like the name and the description. Themes can also be event-based by setting the appropriate property.

<table><thead><tr><th width="149.99993896484375">Property</th><th width="104.666748046875">Optional</th><th>Description</th></tr></thead><tbody><tr><td><code>Name</code></td><td></td><td>Display name of the theme</td></tr><tr><td><code>Description</code></td><td></td><td>Short and concise description of the theme</td></tr><tr><td><code>IsEvent</code></td><td>●</td><td>Indicates whether the theme is for a specific event (holidays, celebrations, etc.)</td></tr><tr><td><code>StartMonth</code></td><td>●</td><td>The month in which the event starts</td></tr><tr><td><code>StartDay</code></td><td>●</td><td>The day in which the event starts</td></tr><tr><td><code>EndMonth</code></td><td>●</td><td>The month in which the event ends</td></tr><tr><td><code>EndDay</code></td><td>●</td><td>The day in which the event ends</td></tr><tr><td><code>Calendar</code></td><td>●</td><td>The calendar to use, such as Hijri, Chinese, etc.</td></tr><tr><td><code>Category</code></td><td>●</td><td>The category to use, such as Mesmerizing, Exciting, etc.</td></tr><tr><td><code>UseAccentTypes</code></td><td>●</td><td>The color types which the theme management will switch their values with the accent color configurable by the user from the global settings</td></tr></tbody></table>

The following color categories are supported:

| Category        | Description                                                      |
| --------------- | ---------------------------------------------------------------- |
| `Miscellaneous` | Themes that don't fit in the available categories                |
| `Aesthetic`     | Aesthetically beautiful themes                                   |
| `Chinese`       | Themes that are based on the Chinese culture                     |
| `Exciting`      | Themes that make you excited                                     |
| `Mesmerizing`   | Mesmerizing and relaxing themes                                  |
| `Standard`      | Standard themes (usually housing the first themes ever released) |
| `Linux`         | Themes that describe the Linux distro logo colors                |

What follows the metadata is a list of available color types and their color representations using color formats that Colorimetry supports. Your theme files can also support any specifier, as long as the specifier is supported.

<a href="https://app.gitbook.com/s/BdESDsiuTO9fbDXLJ8HV/usage/color-sequences" class="button primary">Color sequences</a>

{% hint style="warning" %}
Make sure that the event start month and day is earlier than the end month and day. The theme parser will swap the day values and will add a year (end month is bigger than the start) if it detects that the start date is later than the end date (i.e. events can't end before they start).
{% endhint %}

</details>

<details>

<summary>Theme color types</summary>

Terminaux provides you with the following color types to help you make an inspiring theme with nice colors for each type:

| Type                                  | Description                                           |
| ------------------------------------- | ----------------------------------------------------- |
| `Input`                               | Input text                                            |
| `License`                             | License color                                         |
| `HostNameShell`                       | Host name color                                       |
| `UserNameShell`                       | User name color                                       |
| `Background`                          | Background color                                      |
| `NeutralText`                         | Neutral text (for general purposes)                   |
| `ListEntry`                           | List entry text                                       |
| `ListValue`                           | List value text                                       |
| `Stage`                               | Stage text                                            |
| `Error`                               | Error text                                            |
| `Warning`                             | Warning text                                          |
| `Option`                              | Option text                                           |
| `Banner`                              | Banner text                                           |
| `Question`                            | Question text                                         |
| `Success`                             | Success text                                          |
| `UserDollar`                          | User dollar sign on shell text                        |
| `Tip`                                 | Tip text                                              |
| `SeparatorText`                       | Separator text                                        |
| `Separator`                           | Separator color                                       |
| `ListTitle`                           | List title text                                       |
| `Progress`                            | General progress text                                 |
| `BackOption`                          | Back option text                                      |
| `TableSeparator`                      | Table separator                                       |
| `TableHeader`                         | Table header                                          |
| `TableValue`                          | Table value                                           |
| `SelectedOption`                      | Selected option                                       |
| `AlternativeOption`                   | Alternative option                                    |
| `WeekendDay`                          | Weekend day                                           |
| `EventDay`                            | Event day                                             |
| `TableTitle`                          | Table title                                           |
| `TodayDay`                            | Today                                                 |
| `TuiBackground`                       | Interactive TUI background color                      |
| `TuiForeground`                       | Interactive TUI foreground color                      |
| `TuiPaneBackground`                   | Interactive TUI pane background color                 |
| `TuiPaneSeparator`                    | Interactive TUI pane separator color                  |
| `TuiPaneSelectedSeparator`            | Interactive TUI pane selected separator color         |
| `TuiPaneSelectedItemFore`             | Interactive TUI pane selected item color (foreground) |
| `TuiPaneSelectedItemBack`             | Interactive TUI pane selected item color (background) |
| `TuiPaneItemFore`                     | Interactive TUI pane item color (foreground)          |
| `TuiPaneItemBack`                     | Interactive TUI pane item color (background)          |
| `TuiOptionBackground`                 | Interactive TUI option background color               |
| `TuiKeyBindingOption`                 | Interactive TUI key binding in option color           |
| `TuiOptionForeground`                 | Interactive TUI option foreground color               |
| `TuiBoxBackground`                    | Interactive TUI box background color                  |
| `TuiBoxForeground`                    | Interactive TUI box foreground color                  |
| `DisabledOption`                      | Disabled option color                                 |
| `TuiKeyBindingBuiltinBackgroundColor` | Interactive TUI builtin key binding background color  |
| `TuiKeyBindingBuiltinForegroundColor` | Interactive TUI builtin key binding foreground color  |
| `TuiKeyBindingBuiltinColor`           | Interactive TUI builtin key binding color             |
| `ProgressFailed`                      | Progress failed text                                  |
| `ProgressPaused`                      | Progress paused text                                  |
| `ProgressWarning`                     | Progress warning text                                 |

### <mark style="color:$primary;">Custom types</mark>

In addition to the above types, the custom types can also be populated in the same way a built-in type would in the theme structure file.

You can use any name as a custom theme color type, and the theme manager will automatically determine what color to use based on a custom type.

</details>

***

## <mark style="color:$primary;">Theme tools</mark>

The kernel theming system provides you with various theme tools that allow you to manage your themes, such as getting the installed themes, setting the theme, and more.

<details>

<summary>Installing and uninstalling a theme</summary>

To install and/or uninstall a theme to the theme manager, use `RegisterTheme()` and `UnregiserTheme()`.

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void RegisterTheme(string theme, ThemeInfo themeInfo) { }
public static void UnregisterTheme(string theme) { }
```
{% endcode %}

</details>

<details>

<summary>Getting the installed themes</summary>

You can get the installed themes using the two convenience functions:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static Dictionary<string, ThemeInfo> GetInstalledThemes() { }
public static Dictionary<string, ThemeInfo> GetInstalledThemesByCategory(ThemeCategory category) { }
```
{% endcode %}

The below two functions do the following:

* The first function gets all the installed themes.
* The second function gets all the installed themes by a category.

</details>

<details>

<summary>Getting colors from a theme</summary>

You can get all the used colors from either a theme name or a `ThemeInfo` instance using the two functions:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static Dictionary<KernelColorType, Color> GetColorsFromTheme(string theme) { }
public static Dictionary<KernelColorType, Color> GetColorsFromTheme(ThemeInfo themeInfo) { }
```
{% endcode %}

Each function does the following:

* The first function gets the theme name and fetches its `ThemeInfo` instance. Then, it calls the second one.
* The second function gets all the colors from a theme and updates them if there are color types that use the color accent.

</details>

<details>

<summary>Editing a theme</summary>

You can edit a theme using a modified `ThemeInfo` instance to overwrite it with a new theme using `EditTheme()` that is defined below:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void EditTheme(string theme, ThemeInfo themeInfo) { }
```
{% endcode %}

This function replaces an existing theme with the new `ThemeInfo` instance. This is useful if you want to save changes to the existing theme or override its colors.

</details>

<details>

<summary>Resetting a theme</summary>

You can reset a theme using the `ResetTheme()` function:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void ResetTheme(string theme) { }
```
{% endcode %}

This function resets all theme colors to their default settings according to the metadata token that holds initial theme color type definitions.

</details>

<details>

<summary>Using <code>ThemeInfo</code> or <code>GetThemeInfo()</code> to get theme information</summary>

To get theme information for a specific theme, you need to use the `ThemeInfo` constructor. The below constructors can be used:

<table><thead><tr><th width="250.3333740234375">Constructor</th><th>Description</th></tr></thead><tbody><tr><td><code>ThemeInfo()</code></td><td>This gives you a new instance of <code>ThemeInfo</code> with the default theme parameters</td></tr><tr><td><code>ThemeInfo(themePath)</code></td><td>This gives you a new instance of <code>ThemeInfo</code> with the theme parameters from the given theme JSON file</td></tr><tr><td><code>ThemeInfo(ThemeFileStream)</code></td><td>This gives you a new instance of <code>ThemeInfo</code> with the theme parameters from the given stream containing the theme JSON contents</td></tr></tbody></table>

Alternatively, you can use `GetThemeInfo()` to get theme information about a specified theme using the theme name. After that, you can apply a theme using this instance using `ApplyTheme`.

</details>

<details>

<summary>Checking to see if a theme exists</summary>

There is a function that lets you check to see if a theme exists. `IsThemeFound()` is usable for such themes and can be provided a name of the theme.

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static bool IsThemeFound(string theme) { }
```
{% endcode %}

</details>

<details>

<summary>Theme application</summary>

You can also change all your kernel colors so that a theme can be applied using either a theme name, a theme file name, or a `ThemeInfo` instance using the below functions:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
public static void ApplyThemeFromRegistered(string theme) { }
public static void ApplyThemeFromFile(string ThemeFile) { }
public static void ApplyTheme(ThemeInfo themeInfo) { }
public static void SetColorsTheme(ThemeInfo ThemeInfo) { }
public static bool TrySetColorsTheme(ThemeInfo ThemeInfo) { }
```
{% endcode %}

Each function does the following:

* The first function gets a theme name and gets an instance of `ThemeInfo` for it. Then, it sets the current theme colors to the colors that are defined by a theme.
* The second function parses a theme JSON file while getting an instance of `ThemeInfo` from it and sets the current theme colors to the colors that are set by that theme.
* The third function gets all the colors from `ThemeInfo` and sets the current theme colors to the colors that are defined by that instance.

</details>

<details>

<summary>Checking the color requirements</summary>

You can also check to see if a theme requires 255 colors or true colors using the below functions:

{% code title="ThemeTools.cs" lineNumbers="true" %}
```csharp
// 255 colors
public static bool Is255ColorsRequired(string theme) { }
public static bool Is255ColorsRequired(ThemeInfo theme) { }
public static bool Is255ColorsRequired(Dictionary<KernelColorType, Color> colors) { }

// True colors
public static bool IsTrueColorRequired(string theme) { }
public static bool IsTrueColorRequired(ThemeInfo theme) { }
public static bool IsTrueColorRequired(Dictionary<KernelColorType, Color> colors) { }
```
{% endcode %}

Each function takes either a theme name, an instance of `ThemeInfo`, or a dictionary of each kernel color type with their color instance.

* The 255-color requirement functions check the `Color` instance for the type and return `true` if the type is 255 colors. Also, they return `true` if the theme requires true colors.
* The true color requirement functions check the `Color` instance for the type and return `true` if the type is true colors.

</details>

<details>

<summary>Getting and setting theme colors</summary>

You can get and set theme colors individually from the `ThemeInfo` instance using a specific type as an argument. Getting colors from a theme can be achieved using the `GetColor()` function, and setting colors in a theme can be achieved using the `SetColor()` function.

{% code title="ThemeInfo.cs" lineNumbers="true" %}
```csharp
public Color GetColor(ThemeColorType type) { }
public Color GetColor(string type) { }
public void SetColor(ThemeColorType type, Color color) { }
public void SetColor(string type, Color color) { }
```
{% endcode %}

Each function takes either a named theme color type or an enumerated type that is of type `ThemeColorType` to specify what theme color type you want to get the color from. The `SetColor()` functions require that the color be specified.

</details>

<details>

<summary>Converting colors</summary>

Your theme files can also support any specifier, as long as the specifier is supported by Colorimetry. You can consult the page below:

</details>

***

## <mark style="color:$primary;">Theme preview tools</mark>

In addition to the theme tools, you can also access the theme preview tools using the `ThemePreviewTools` class. Currently, the theme preview tools provides you two types of theme preview:

<details>

<summary>Simple preview</summary>

The simple theme preview shows you a wrapped list of color types and their examples, colored with the foreground color in the placeholder text. You can get access to this preview by calling these functions:

{% code title="ThemePreviewTools.cs" lineNumbers="true" %}
```csharp
public static void PreviewThemeSimple(string theme)  { }
public static void PreviewThemeSimple(ThemeInfo theme) { }
```
{% endcode %}

Each of these two functions get a list of color types known by the color management tools and their associated colors. Then, they show you a preview of the theme colors.

</details>

<details>

<summary>Interactive preview</summary>

The interactive theme preview shows you a full-screen colored box that changes its color according to the selected color type and the list of theme colors. You can get access to this preview by calling these functions:

{% code title="ThemePreviewTools.cs" lineNumbers="true" %}
```csharp
public static void PreviewTheme(string theme) { }
public static void PreviewTheme(ThemeInfo theme) { }
```
{% endcode %}

Each of these two functions get a list of kernel color types known by the color management tools and their associated colors. Then, they show you a preview of the theme colors.

</details>
