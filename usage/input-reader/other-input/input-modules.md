---
description: What are input modules on Terminaux?
icon: command
---

# Input Modules

Input modules are components that describe how the input is going to be shown to users, and the actual implementation of the input, such as the method of telling users to provide an input.

***

## <mark style="color:$primary;">Definition</mark>

They are defined in a single abstract `InputModule` class that contains the necessary properties and functions that describe an input. Additionally, the derived classes may implement the two functions that work on rendering the input placeholder and processing user input, `RenderInput()` and `ProcessInput()`.

The following built-in input methods are available:

<table><thead><tr><th width="209.33331298828125">Module</th><th>Description</th></tr></thead><tbody><tr><td><code>ComboBoxModule</code></td><td>Select an item (single choice)</td></tr><tr><td><code>MaskedTextBoxModule</code></td><td>Text box with hidden characters as mask</td></tr><tr><td><code>MultiComboBoxModule</code></td><td>Select multiple items (multiple choices)</td></tr><tr><td><code>SliderBoxModule</code></td><td>Select a number visually</td></tr><tr><td><code>TextBoxModule</code></td><td>Text box</td></tr><tr><td><code>CharBoxModule</code></td><td>Text box that accepts one character</td></tr><tr><td><code>TimeBoxModule</code></td><td>Time box</td></tr><tr><td><code>DateBoxModule</code></td><td>Date box</td></tr></tbody></table>

***

## <mark style="color:$primary;">Usage</mark>

You can use an information box that supports input modules, called `InfoBoxMultiInputColor`. It allows you to specify an array of input modules. You can also make presentations use the input modules to customize the flow of the presentations according to the user input.

In order to be able to use those modules, you'll need to instantiate a class and to fill the necessary fields that are required for modules to be distinguishable. An example code shown below shows the definitions of the three types of input modules:

{% code expandable="true" %}
```csharp
var modules = new InputModule[]
{
    new TextBoxModule()
    {
        Name = "Text Box Integer",
        Description = "Type any integer to continue",
    },
    new TextBoxModule()
    {
        Name = "Text Box String",
        Description = "Type any string to continue",
    },
    new MaskedTextBoxModule()
    {
        Name = "Text Box Password",
        Description = "Type any password to continue",
    },
    new SliderBoxModule()
    {
        Name = "Choose a Number",
        Description = "Choose a number between 75 and 250. You'll start from 100.",
        MinPos = 75,
        MaxPos = 250,
        Value = 100,
    },
};
InfoBoxMultiInputColor.WriteInfoBoxMultiInput(modules, nameof(TestInputInfoBoxMultiInput), "Select an input module to test...");
```
{% endcode %}

For all input modules, `Name` and `Description` properties must be filled when instantiating such modules for reachability.

***

## <mark style="color:$primary;">Extra requirements</mark>

Some of the modules might require extra information to be supplied by the caller. Here are some of the built-in modules which require extra properties to be filled:

| Module                | Property  | Description                 |
| --------------------- | --------- | --------------------------- |
| `ComboBoxModule`      | `Choices` | Specifies a list of choices |
| `MultiComboBoxModule` | `Choices` | Specifies a list of choices |
| `SliderBoxModule`     | `MinPos`  | Specifies a minimum value   |
|                       | `MaxPos`  | Specifies a maximum value   |
|                       | `Value`   | Specifies the current value |

***

## <mark style="color:$primary;">Custom input modules</mark>

In order to make your own derived input module class, you must inherit from the base `InputModule` class, which will require implementing the following abstract functions:

* `RenderInput()`
* `ProcessInput()`

{% hint style="info" %}
It's recommended that `RenderInput()` respect the `width` field in the function parameter to maintain uniform look. Here's an example as to how to get a value and adjust the rendered input as per the `width` field:

{% code expandable="true" %}
```csharp
public override string RenderInput(int width)
{
    // Render an input text box with selected value and blanks as underscores.
    string valueString = Value?.ToString() ?? "";
    string[] wrappedValue = ConsoleMisc.GetWrappedSentencesByWords(valueString, width);
    valueString = wrappedValue[0];

    // Determine how many underscores we need to render
    int valueWidth = ConsoleChar.EstimateCellWidth(valueString);
    int diffWidth = width - valueWidth;
    string underscores = new('_', diffWidth);

    // Render the text box contents now
    string textBox =
        ColorTools.RenderSetConsoleColor(Foreground) +
        ColorTools.RenderSetConsoleColor(Background, true) +
        valueString +
        ColorTools.RenderSetConsoleColor(BlankForeground) +
        underscores +
        ColorTools.RenderRevertForeground() +
        ColorTools.RenderRevertBackground();
    return textBox;
}
```
{% endcode %}
{% endhint %}

### <mark style="color:$primary;">Input pop-over</mark>

`ProcessInput()` uses the `inputPopoverPos` and the `inputPopoverSize` fields to determine where to render the popover, which could either:

* Render over the rendered input placeholder with `RenderInput()`, or
* Render under the input placeholder, such as combo boxes with selection choices.

{% hint style="info" %}
Some of the components that use input modules handle extra popover height, which you can handle it yourself using the `ExtraPopoverHeight` property. You can override it when declaring a new input module.
{% endhint %}

### <mark style="color:$primary;">Customized rendering</mark>

In customized applications, you may have to render the input yourself using the `RenderInput()` function and to process the input yourself using the above function. Additionally, you may have to calculate the popover position yourself. It's usually located at the same position as the rendered input, but there are cases where you may need to render it just below the rendered input.

{% code expandable="true" %}
```csharp
// Make a box frame and a text box module instance
var boxFrame = new BoxFrame()
{
    Left = 4,
    Top = 2,
    Width = ConsoleWrapper.WindowWidth - 14,
    Height = 1,
};
var boxModule = new TextBoxModule();

// Render the input placeholder
TextWriterRaw.WriteRaw(
    boxFrame.Render() +
    ConsolePositioning.RenderChangePosition(5, 3) +
    boxModule.RenderInput(ConsoleWrapper.WindowWidth - 14)
);

// Let the user press any key
Input.ReadKey();

// Process the input using the popover method
var popoverPos = new Coordinate(5, 3);
var popoverSize = new Size(ConsoleWrapper.WindowWidth - 14, 1);
boxModule.ProcessInput(popoverPos, popoverSize);
```
{% endcode %}
