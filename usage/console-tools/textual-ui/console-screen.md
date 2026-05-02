---
description: Your screen in front of you.
icon: display
---

# Console Screen

Terminaux offers the console screen feature, which allows you to define a screen for your interactive console application. This guarantees you a dynamic terminal sequence generation that you can print to the console, including VT sequences.

***

## <mark style="color:$primary;">`Screen`</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">Instance</mark>

You can get started by making a new instance of the `Screen` class and using it to add a new `ScreenPart` instance with its name to make a layer for your rendering sequences. This facilitates buffering the screens to the console.

The screen part instance allows you to add text and sequences in different ways:

<table><thead><tr><th width="170">Function/property</th><th>Description</th></tr></thead><tbody><tr><td><code>AddText()</code></td><td>Adds a simple and static text to the buffer</td></tr><tr><td><code>AddTextLine()</code></td><td>Adds a simple and static text to the buffer with the extra new line</td></tr><tr><td><code>AddDynamicText()</code></td><td>This is the key of the Screen feature. It allows you to define a function delegate that generates text dynamically. It's useful for displaying anything that changes.</td></tr><tr><td><code>Clear()</code></td><td>Clears the whole buffer</td></tr><tr><td><code>GetBuffer()</code></td><td>Gets the resulting buffer</td></tr><tr><td><code>RequireRefresh()</code></td><td>Tells the screen system to require a refresh</td></tr><tr><td><code>Visible</code></td><td>Whether this part is visible or not</td></tr></tbody></table>

{% hint style="info" %}
You can require refresh using the `RequireRefresh()` function on the screen instance, and your renderer can use `NeedsRefresh` to check to see whether we need to refresh the entire screen or not.

During the render process, if `NeedsRefresh` was true, the refresh manager would set the "refresh was done" flag to true, causing all dynamic text renderers to understand that the refresh was done. From there, you can use the `RefreshWasDone` flag to change the behavior of the renderer function.
{% endhint %}

***

## <mark style="color:$primary;">The structure</mark>

The screen feature contains the following classes for different purposes:

<details>

<summary>Screen</summary>

The screen class hosts an array of screen parts that you can add, edit, or remove using the following functions:

* `AddBufferedPart()`
* `EditBufferedPart()`
* `RemoveBufferedPart()`

You are required to provide a name or a GUID for the specific screen part. This is to easily identify different screen parts, because if you're just looking for final buffer results, you might not be able to tell which part was for which.

You can use either the buffered part index number or the part name when using any of the following querying functions (in addition to the last two functions):

* `GetBufferedPart()`
* `CheckBufferedPart()`

You can also clear the entire buffered parts queue using the `RemoveBufferedParts()` function.

{% hint style="info" %}
By default, when the screen feature detects a resize, it'll re-render the current screen. It will, by default, reset the resize state, causing some programs to behave incorrectly.

If you want Terminaux not to reset the resize state when the screen is resized, use the `ResetResize` property to set it to `false`.
{% endhint %}

</details>

<details>

<summary>Screen Part</summary>

The screen part hosts a list of dynamic buffers to generate a working text sequence for the console plain writer to write it to the terminal to show you its contents. You can also call the screen parts as "layers" to more easily understand the motive behind these "layers."

{% hint style="info" %}
In order to uniquely identify each screen part without any ambiguity, you can use the `Id` property that is populated each time you make a new screen part.
{% endhint %}

### Adding simple or dynamic text

You can use the following functions to add simple or dynamic text to the buffer queue:

* `AddText()`: Adds a simple text without the newline.
* `AddTextLine()`: Adds a simple text with the newline.
* `AddDynamicText()`: This is the key of all the resizable console TUIs. You need to provide a lambda or a function that hosts the entire rendering sequence.

{% hint style="info" %}
If you want to clear the queue list without printing the buffers to the console, you can clear the list of dynamic buffers using the `Clear()` function. You can also control its visibility using the Visible property.
{% endhint %}

### Ordering screen parts

You can order them using the `Order` property when creating them. However, TermRead renders all the screen parts from the least important (the smallest `Order` values) to the most important (the largest `Order` values). This is useful for layering if you don't feel comfortable adding dynamic texts that represent layers.

```csharp
var orderedPart = new ScreenPart() { Order = 1 };
var importantPart = new ScreenPart() { Order = 100 };
```

</details>

***

## <mark style="color:$primary;">Screen Tools</mark>

The screen feature contains a set of tools that allow you to manipulate with a screen and its associated parts, along with extra features.

<details>

<summary>Setting the current screen</summary>

Setting a current screen requires you to provide `SetCurrent()` with a console screen instance once you're done making your own screen and filling it with necessary printing strings, such as an interactive TUI or a simple two rulers resizable application as provided in the example at the top of the page.

Once the current screen is set, `CurrentScreen` returns your screen instance. This means that you've made that screen the current screen. You can easily access the list of screens via the `Screens` property, with the current screen being the last one.

When this is done, the screensaver manager and the console resize listener will refresh and redraw your screen, taking new console window dimensions to account. This reaction makes your interactive console applications that use screens responsive to the resize events.

{% hint style="info" %}
`CurrentScreen` returns `null` upon calling it with no screen set. Therefore, it's advisable to check for this case when trying to access it if you don't set more than one screen as a default. The easy way to perform such a check is using `IsOnScreen`.
{% endhint %}

</details>

<details>

<summary>Rendering the screen</summary>

You can render your screen using:

* `Render()`: Renders your current screen. Throws if you didn't set your current screen before calling this version of the function.
* `Render(screen)`: Renders your screen instance to the console.

However, for `Render()` to work, you need to add your screen instance to the list of tracked screens in the screen manager. This can be done by calling the `SetCurrent()` function on your screen instance.

</details>

<details>

<summary>Unsetting the current screen</summary>

To unset the screen, you'll have to call `UnsetCurrent()` with the screen instance that you've set it as the default before. This removes the screen from the list of tracked screens.

{% hint style="info" %}
If you've called the above function with two or more screens, `CurrentScreen` doesn't return `null`, but returns the last screen found in the list.
{% endhint %}

{% hint style="warning" %}
Don't forget to remove your screen from the list of tracked screens once you're done using it. You can call `UnsetCurrent()` to make this happen.

If your application contains a main loop wrapped in a `try` and `catch` block, you must create a `finally` block (if not done) and call `UnsetCurrent()` on your screen instance. It may be necessary to move your screen instance declaration in your code outside the `try` and `catch` block.
{% endhint %}

</details>

<details>

<summary>Screen overlays</summary>

Screen overlays render on top of whatever gets rendered on the screen to act as an overlay, and uses a completely independent screen part as the overlay, but there are two types of screen overlays:

* Screen-specific overlays (`OverlayPart`): These overlays are specific to a screen instance, and you can choose a screen part that will act as an overlay.
* Global screen overlays (`GlobalOverlayPart`): These overlays are just a single overlay for all screen instances, and there can only be one screen part that can act as this overlay. Global screen overlays also act as an overlay on top of screen-specific overlays.

You can set a screen overlay by setting any of the two properties in the screen instance, knowing that you'll have to refer to a base screen class for the global overlay.

```csharp
// Screen-specific overlays
stickScreen.OverlayPart = pinkOverlayBox;

// Global screen overlays
Screen.GlobalOverlayPart = cyanOverlayBox;
```

</details>

<details>

<summary>Cyclic screens</summary>

Cyclic screens are screens that get rendered for each specified amount of milliseconds. This is useful for screens that show dynamic data that change, while holding Terminaux responsibility for the refresh of the screen.

You can make the screen cyclic by specifying the amount of milliseconds that describes the frequency of the cyclic screen. This is achieved using the `CycleFrequency` property.

Before the cyclic screen gets rendered in a loop, you must first set it as a default cyclic screen using the `SetCurrentCyclic()` function and render it using the `StartCyclicScreen()` function instead of the usual `Render()` function.

When you're done rendering this screen, for example, exiting from a specific screen, use the `StopCyclicScreen()` function. Afterwards, you can use the `UnsetCurrentCyclic()` function to unset the current cyclic screen.

{% hint style="info" %}
You can't override the current cyclic screen until the thread that renders the chosen cyclic screen stops. You can check to see if this thread is running using the `CyclicScreenRunning()` function.
{% endhint %}

</details>

***

## <mark style="color:$primary;">An example</mark>

The interactive testing system allows you to try the demonstration of this feature out to show you the concept of what happens when you try to resize the console.

The screen instance in question shows you two rulers:

* A horizontal ruler that shows you the width of the console window
* A vertical ruler that shows you the height of the console window

{% @github-files/github-code-block url="https://github.com/Aptivi/Terminaux/blob/main/private/Terminaux.Console/Fixtures/Cases/Screens/TestScreen.cs" visible="true" %}
