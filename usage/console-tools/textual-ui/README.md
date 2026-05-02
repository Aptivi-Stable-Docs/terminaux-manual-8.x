---
description: More control in your TUIs!
icon: browser
---

# Textual UI

This feature is the new form of interactive TUIs that not only allow you to make an interactive TUI based on a set of data for either a single pane or two panes, but you can also make your own interactive applications seamlessly by separating the rendering code and the key handling code into small functions. This achieves the goal of making textual UI code maintenance easier, while providing the most dynamic UI setup along with support for resize thanks to the [screen](console-screen.md) feature.

{% hint style="info" %}
To get started using this awesome feature, you can create a new class that implements the `TextualUI` abstract class and overrides the `Render()` function. Inside the function, you can return a string that consists of VT sequences resulting from several console writers, especially the cyclic writers.
{% endhint %}

***

## <mark style="color:$primary;">Textual UI properties</mark>

In the class, there are several properties that you can modify at your discretion, such as the name and the refresh delay. This allows you to customize its behavior.

You can use the following properties:

<table><thead><tr><th width="139.99993896484375">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>Guid</code></td><td>Unique ID for the textual UI</td></tr><tr><td><code>State</code></td><td>State of the textual UI</td></tr><tr><td><code>Name</code></td><td>Display name of the textual UI</td></tr><tr><td><code>RefreshDelay</code></td><td>Sets the refresh delay in milliseconds. If set to zero or less than zero, this TUI doesn't refresh.</td></tr><tr><td><code>Keybindings</code></td><td>List of keybindings that you can use within the TUI</td></tr><tr><td><code>Fallback</code></td><td>Fallback binding in case a key doesn't bind to anything</td></tr><tr><td><code>Renderables</code></td><td>List of renderables that get rendered on top of what the <code>Render()</code> function has rendered</td></tr><tr><td><code>HelpPages</code></td><td>List of help pages for the interactive TUI</td></tr></tbody></table>

{% hint style="info" %}
For the help pages, you'll have to override the `HelpPages` property with an array of `InteractiveTuiHelpPage` classes, which consist of a title, a description, and a body text.
{% endhint %}

***

## <mark style="color:$primary;">Textual UI states</mark>

State of the textual UI can be one of the following:

<table><thead><tr><th width="109.333251953125">State</th><th>Description</th></tr></thead><tbody><tr><td><code>Ready</code></td><td>This textual UI is ready, but hasn't started yet.</td></tr><tr><td><code>Rendering</code></td><td>This textual UI is waiting for the render code to complete.</td></tr><tr><td><code>Waiting</code></td><td>This textual UI is waiting for user input.</td></tr><tr><td><code>Busy</code></td><td>This textual UI is busy because it's processing user input.</td></tr><tr><td><code>Bailing</code></td><td>This textual UI is about to exit and go back to the <code>Ready</code> state.</td></tr></tbody></table>

***

## <mark style="color:$primary;">Textual UI tools</mark>

Some of the textual UI tools are available to help build interactive applications. You can also access the `TextualUITools` class to perform the following operations:

<table><thead><tr><th width="169.3333740234375">Function</th><th></th></tr></thead><tbody><tr><td><code>RunTui()</code></td><td>Starts the TUI main loop, clears the screen, waits for user input, and processes it until the TUI has been requested to exit.</td></tr><tr><td><code>ExitTui()</code></td><td>Sets the state of the interactive TUI to <code>Bailing</code> so that the TUI can exit gracefully.</td></tr><tr><td><code>RequireRefresh()</code></td><td>Tells the TUI to refresh on the next render.</td></tr></tbody></table>

***

## <mark style="color:$primary;">Defining Keybindings</mark>

When defining keybindings, you can edit the `Keybindings` property to add your custom keybindings, but it's preferrable to either place them in a constructor or in the overridden value, and to define the delegates in separate private functions inside the UI class.

<details>

<summary>Defining keybindings</summary>

To define a keybinding in your interactive TUI, use the `Keybindings.Add()` statement to pass in a tuple of the following two variables:

* A [`Keybinding`](../../input-reader/other-input/keybindings.md) instance that matches the key and its modifiers (mouse or keyboard) that you want to bind a specific action to.
* An action delegate or lambda function that tells the interactive TUI what to do.

{% hint style="info" %}
The following four arguments are optional for lambda expressions, but required for delegates:

* Interactive TUI instance that specifies the running TUI.
* Keyboard press info defined by the [ConsoleKeyInfo](https://learn.microsoft.com/en-us/dotnet/api/system.consolekeyinfo) struct.
* Mouse event info defined by the [PointerEventContext](../../input-reader/pointer-events.md) class.
{% endhint %}

</details>

<details>

<summary>Removing keybindings</summary>

To remove a keybinding in your interactive TUI, use the `Keybindings.RemoveAt()` function.

To remove all keybindings in your interactive TUI, use the `Keybindings.Clear()` function.

</details>

{% hint style="info" %}
Make sure that keybindings don't conflict when adding a new keybinding.
{% endhint %}

***

## <mark style="color:$primary;">Examples</mark>

There are two examples as defined in the test application bundled with the source code of Terminaux:

{% tabs %}
{% tab title="Simple" %}
This example provides you with a simple interactive TUI showing three boxes that you can control using the keybindings defined in the constructor of the interactive TUI.

{% @github-files/github-code-block url="https://github.com/Aptivi/Terminaux/blob/main/Terminaux.Console/Fixtures/Cases/CaseData/TextualUiSimpleTestData.cs" %}

Here's the companion code to start the interactive TUI for the above class:

{% @github-files/github-code-block url="https://github.com/Aptivi/Terminaux/blob/main/Terminaux.Console/Fixtures/Cases/Tui/TestScreenPartVisibilityTui.cs" %}

Here's the resulting picture of the interactive TUI:

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Refreshing" %}
This example provides you with a refreshing interactive TUI showing three boxes, one of which is constantly changing color according to the number of frames rendered, that you can control using the keybindings defined in the constructor of the interactive TUI.

{% @github-files/github-code-block url="https://github.com/Aptivi/Terminaux/blob/main/Terminaux.Console/Fixtures/Cases/CaseData/TextualUiRefreshTestData.cs" %}

Here's the companion code that executes the above TUI class:

{% @github-files/github-code-block url="https://github.com/Aptivi/Terminaux/blob/main/Terminaux.Console/Fixtures/Cases/Tui/TestScreenPartVisibilityRefreshTui.cs" %}

Here's the resulting picture for the TUI:

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
