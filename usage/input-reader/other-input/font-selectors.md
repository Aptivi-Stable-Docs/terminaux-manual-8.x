---
description: Select your favorite font from here!
icon: pen-nib
---

# Font Selectors

Font selectors are one of the other input modules that allow you to select a font from a wide list of fonts, whether it be Figlet or Cowsay, to make text more amusing.

To get started, select one of the font types below:

<details>

<summary>Cowsay</summary>

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

The Cowsay selector allows you to flexibly select a Cowsay font to make reading text more amusing. It provides virtual characters' ASCII art that speaks or thinks, and it shows you a live preview of the font to show how your selected font looks like prior to submission.

### <mark style="color:$primary;">Invoking the selector</mark>

You can simply invoke the selector on your interactive console application by calling the below function like so:

```csharp
var cowName = CowsaySelector.PromptForCowsay();
```

Usually, this call is followed by using the simple cyclic writer for Cowsay.

```csharp
var cowsayText = new CowsayText(cowName, "Hello world!");
TextWriterRaw.WriteRaw(cowsayText.Render());
```

### <mark style="color:$primary;">Controls</mark>

The following controls are available for the normal Cowsay font selector:

<table><thead><tr><th width="226">Key</th><th>Action</th></tr></thead><tbody><tr><td><code>ENTER</code></td><td>Accept font selection</td></tr><tr><td><code>ESC</code></td><td>Discard changes</td></tr><tr><td><code>H</code></td><td>Help page</td></tr><tr><td><code>LEFT</code> / <code>WHEEL UP</code></td><td>Previous font</td></tr><tr><td><code>RIGHT</code> / <code>WHEEL DOWN</code></td><td>Next font</td></tr><tr><td><code>S</code></td><td>Select font from the infobox</td></tr><tr><td><code>SHIFT</code> + <code>S</code></td><td>Write font name</td></tr></tbody></table>

</details>

<details>

<summary>Figlet</summary>

<figure><img src="../../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

The figlet font selector allows you to flexibly select a Figlet font provided by the Figlet part of Textify, powered by the [textual UI](../../console-tools/textual-ui/) feature. It shows you a live preview of the font to show you how your selected font looks like prior to submission.

{% hint style="info" %}
`FigletTextTools` provides default figlet font settings.
{% endhint %}

### <mark style="color:$primary;">Invoking the selector</mark>

You can simply invoke the selector on your interactive console application by calling the below function like so:

```csharp
string font = FigletSelector.PromptForFiglet();
```

Usually, this call is followed by getting a figlet font from the above variable and writing it using the `WriteFiglet()` function:

```csharp
var figlet = FigletTools.GetFigletFont(font);
FigletColor.WriteFiglet("Hello!", figlet, ConsoleColors.Green);
```

### <mark style="color:$primary;">Controls</mark>

The following controls are available for the normal figlet font selector:

<table><thead><tr><th width="226">Key</th><th>Action</th></tr></thead><tbody><tr><td><code>ENTER</code></td><td>Accept font selection</td></tr><tr><td><code>ESC</code></td><td>Discard changes</td></tr><tr><td><code>H</code></td><td>Help page</td></tr><tr><td><code>LEFT</code> / <code>WHEEL UP</code></td><td>Previous font</td></tr><tr><td><code>RIGHT</code> / <code>WHEEL DOWN</code></td><td>Next font</td></tr><tr><td><code>S</code></td><td>Select font from the infobox</td></tr><tr><td><code>SHIFT</code> + <code>S</code></td><td>Write font name</td></tr><tr><td><code>C</code></td><td>Shows the individual characters with the selected font</td></tr></tbody></table>

The following controls are available for the character showcase:

<table><thead><tr><th width="226">Key</th><th>Action</th></tr></thead><tbody><tr><td><code>ENTER</code></td><td>Go back</td></tr><tr><td><code>LEFT</code> / <code>WHEEL UP</code></td><td>Previous character</td></tr><tr><td><code>RIGHT</code> / <code>WHEEL DOWN</code></td><td>Next character</td></tr></tbody></table>

</details>
