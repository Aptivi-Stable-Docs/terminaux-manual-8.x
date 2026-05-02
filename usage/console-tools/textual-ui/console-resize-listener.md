---
description: We're listening for any size changes!
icon: maximize
---

# Console Resize Listener

Terminaux provides a fast and stable console resize listener that allows your console application, especially the interactive ones, to listen to every single console resize event for all the platforms. It works on Windows, macOS, Linux, and Android.

***

## <mark style="color:$primary;">Starting and stopping</mark>

You can start and stop the resize listener using the following functions:

<table><thead><tr><th width="209.99993896484375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>StartResizeListener()</code></td><td>Starts the resize listener with either the default resize handler or a custom resize handler that we'll explain later in this page.</td></tr><tr><td><code>StopResizeListener()</code></td><td>Stops the resize listener if it's running.</td></tr></tbody></table>

{% hint style="info" %}
You can check the state of the resize listener by calling the `IsListening` property.
{% endhint %}

***

## <mark style="color:$primary;">Other Functions</mark>

The console resize listener contains several of the convenience functions that allow you to control the listener.

<details>

<summary>Convenience functions</summary>

<table><thead><tr><th width="229.99993896484375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>WasResized()</code></td><td>This function tells you if the console has been resized before or not. If it has been resized before, it returns <code>true</code> and sets the cached resized state to <code>false</code>.</td></tr><tr><td><code>GetCurrentConsoleSize()</code></td><td>This function gives you a tuple that represents the cached window width and the window height.</td></tr></tbody></table>

{% hint style="info" %}
Upon calling `WasResized()`, it sets the cached resized state to false. You need to either store this value to a local variable or you need to pass `false` to the function to tell it not to reset the state.

The second function gets the console size from the terminal if the listener hasn't started yet. Otherwise, it returns the cached window width and the height in case it's run on a loop, directly or indirectly.
{% endhint %}

</details>

<details>

<summary>Custom handlers</summary>

The console resize listener contains a facility that allows you to set a custom resize handler to change the way how your console application responds to console resizes. This allows you to specify your custom action that gets invoked if the console is resized.

When you start the console resize listener, you'll have an opportunity to specify a custom handler. This way, Terminaux calls your handler if it detects that the console has been resized.

```csharp
public static void StartResizeListener(Action customHandler = null) { }
```

Your custom handler usually does necessary operations to adapt your application to the resized console size. It gets called every time the resize listener detects that your console has been resized.

{% hint style="info" %}
Your custom handler should satisfy these conditions:

* The handler is static.
* The handler's return value is void.
* The handler's signature should not take any arguments.
*   The handler should, at the very least, re-render the current screen with the following code:

    ```csharp
    if (ScreenTools.CurrentScreen is not null)
        ScreenTools.Render();
    ```
* The handler should be fast in the order of milliseconds (minimum) or microseconds (recommended) in response to the resize.
{% endhint %}

You can also enable or disable the essential handler to be able to control if Terminaux is allowed to run the essential handler that contains the screen refresh code or not.

This is useful for situations where you might not want to refresh the screen unless a specific condition is met. `RunEssentialHandler` provides you with this kind of control.

{% hint style="info" %}
You might still want to put code that refreshes the current screen with `ScreenTools.Render()` in your custom resize handler if you've turned off `RunEssentialHandler`. For this very reason, it's turned **on** by default.
{% endhint %}

</details>
