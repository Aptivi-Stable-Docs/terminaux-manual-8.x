---
description: Presenting your things to the kernel!
icon: presentation-screen
---

# Presentation System

This API provides you the presentation system used for presenting something to your users in the full-screen view. It's like a presentation in steroids.

***

## <mark style="color:$primary;">How to present</mark>

To present your presentation to your users, the presentation system must know how to render the presentation using a set of elements.

<details>

<summary>Implementing a presentation class instance</summary>

To implement a `Presentation` class instance, you must call its constructor with the following variables:

<table><thead><tr><th width="120">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>Name</code></td><td>Presentation name</td></tr><tr><td><code>Pages</code></td><td>Presentation pages (List of <code>PresentationPage</code> instances)</td></tr></tbody></table>

</details>

<details>

<summary>Implementing a presentation page instance</summary>

To implement the `PresentationPage` instances, you must call its constructor with the following variables:

<table><thead><tr><th width="120">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>Name</code></td><td>Presentation name</td></tr><tr><td><code>Elements</code></td><td>Presentation page elements (List of <code>IElement</code> instances)</td></tr></tbody></table>

{% hint style="info" %}
You can make cyclic presentation pages by assigning a non-zero value to the `CycleFrequency` property in your presentation page. To learn more about how it works, consult the below page:

<a href="textual-ui/console-screen.md" class="button primary">Console Screen</a>
{% endhint %}

</details>

<details>

<summary>Implementing presentation elements</summary>

To implement the page elements, make new instances of the elements. You can also make your custom `IElement` in your code, and no registration is needed.

The following elements are available:

<table><thead><tr><th width="190.3333740234375">Element</th><th width="130">Description</th><th>Parameters</th></tr></thead><tbody><tr><td><code>TextElement</code></td><td>Static text</td><td>The first argument in the element <code>Arguments</code> is the string to be printed.</td></tr><tr><td><code>DynamicTextElement</code></td><td>Dynamic text</td><td>The first argument in the element <code>Arguments</code> is the action which generates the string.</td></tr></tbody></table>

</details>

<details>

<summary>Implementing input</summary>

You can add input to your presentation to interact with your users more. Just create a new instance of the `PresentationInputInfo` class and provide the title, the description, and a new instance of the input module class that implements the `InputModule` class.

```csharp
internal static PresentationInputInfo input =
    new("Second multiple choice", "Asks the user to select one or more of the names (larger)",
        new MultiComboBoxModule()
        {
            Name = "Second multiple choice",
            Description = "Ultricies mi eget mauris pharetra sapien et ligula:",
            Choices = GetCategories(data2)
        }, true
    );
```

Then, in the `PresentationPage` constructor, you must add your input instance to the second argument that represents an array of `InputInfo` class instances, like this:

```csharp
new PresentationPage("Fifth page - Debugging choice input",
    [
        new TextElement()
        {
            Arguments = [
                "Tincidunt nunc pulvinar sapien et ligula ullamcorper malesuada proin."
            ]
        },
    ],
    [
        input2,
        input3,
    ]
),
```

{% hint style="info" %}
In order to be able to get the input, it's recommended to put the `PresentationInputInfo` instances in their own variables so that you can act according to the input value.

You can also specify whether the input is required in the constructor. Currently, the input is not required, but you can pass `true` to the fourth argument to make it required, such as in the example above.
{% endhint %}

### <mark style="color:$primary;">Input requirement</mark>

The presentation system checks the page to see if there are any input instances. If true, you'll be presented with an informational box telling you to select an input to fill.

The asterisk next to the number denotes the required input. This means that users should fill in such input before being able to go on. Those without the asterisk means that it's fully optional.

{% hint style="info" %}
To learn more about input modules, consult the below page:

<a href="../input-reader/other-input/input-modules.md" class="button primary">Input Modules</a>
{% endhint %}

</details>

<details>

<summary>Customizing appearance</summary>

You can customize how your slideshow looks using the following properties:

<table><thead><tr><th width="169.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>BorderSettings</code></td><td>Customizes your presentation's borders</td></tr><tr><td><code>FrameColor</code></td><td>Customizes your presentation's border frame color</td></tr><tr><td><code>BackgroundColor</code></td><td>Customizes your presentation's background color</td></tr></tbody></table>

</details>

***

## <mark style="color:$primary;">Controls</mark>

The presentation viewer has the following controls:

<table><thead><tr><th width="248.66668701171875">Keybinding</th><th></th></tr></thead><tbody><tr><td><code>ENTER</code> / <code>Left-click (mouse)</code></td><td>Advances to the next page</td></tr><tr><td><code>ESC</code></td><td>Bails out from the presentation</td></tr></tbody></table>

{% hint style="info" %}
Please note that `ESC` has no effect on kiosk and modal presentations.
{% endhint %}
