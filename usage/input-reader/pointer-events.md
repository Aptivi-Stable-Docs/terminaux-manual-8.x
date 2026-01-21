---
description: Mouse on your console!
icon: computer-mouse
---

# Pointer Events

Terminaux not only provides keyboard-based input, but it also provides mouse-based input. This adds flexibility to your already-flexible interactive console user interfaces by making them behave as if they are graphical user interface applications, but in the form of text.

{% hint style="success" %}
Terminaux is the **#1** console manipulation library that proudly features console mouse pointer support; something that competitors like `Spectre.Console` don't provide!
{% endhint %}

Do you want to enable it in your application? If so, you'll need to set the `MouseEnabled` property value to `true` that is found in the `Input` class. Once started, the application that waits for mouse events respond to every single mouse-based event based on the following conditions:

* If the user has moved their mouse and the movement events are acknowledged according to the `EnableMovementEvents` property, a `PointerEventContext` is made with `ButtonPress` being `Moved`.
* If the user has clicked anywhere on the console, a `PointerEventContext` is made with `ButtonPress` being `Clicked`. Also, the `Button` property indicates what mouse button was being pressed at the time. As soon as the user has let go of the same button, another context is made with `ButtonPress` being `Released`.
* If the user has clicked anywhere and moved the pointer without releasing any button, a `PointerEventContext` is made with `ButtonPress` being `Moved` and with `Dragging` being `True`. This enables applications to indicate that the user was dragging the mouse while clicking a button at the same time.
* If the user has used scrolling wheels in their mouse, a `PointerEventContext` is made with `ButtonPress` being `Scrolled` and `Button` indicating whether the user has scrolled up or down.

All pointer events include whether any of CTRL, ALT, and SHIFT keys were pressed at the time of the event or not. This is indicated in the `Modifiers` property. Such events also indicate the position of the mouse where the event occurred, which is a very important aspect to handling mouse click events in console applications. You can access this information using the `Coordinates` property that gives you two variables: `x` and `y`. The coordinates start from zero.

You can also access the button click tier information when the `PointerButtonPress` value is `Released`. Button click tier 1 means a single click, 2 means a double click, and so on. You can access this information using the `ClickTier` property.

{% hint style="info" %}
Here are some notes to consider before implementing pointer support to your Terminaux console application:

* Each platform handles console mouse pointer events differently. While Linux, macOS, and Android use [VT sequences](https://www.xfree86.org/current/ctlseqs.html#Mouse%20Tracking), Windows uses its own API to fetch console events as seen in the [`MOUSE_EVENT_RECORD`](https://learn.microsoft.com/en-us/windows/console/mouse-event-record-str) structure. We're trying to polish the relationship across Terminaux releases to make applications that use mouse event handling behave more consistently.
* On Android, you'll have to connect your external wireless mouse to your phone or your tablet in order to be able to click anywhere. Movement handling is not supported there.
* On Linux, macOS, and Android, calling `ConsoleWrapper.CursorLeft` or `CursorTop` may have a negative performance impact, depending on how often they get called. In most cases, you don't even need this information.
* On Linux, macOS, and Android, Terminaux uses SGR protocol by default due to the older X10 protocol limitations.
{% endhint %}

{% hint style="info" %}
All of the input methods that use the whole screen, such as selection and [your interactive TUI](../console-tools/textual-ui/interactive-tui.md) apps, support mouse.
{% endhint %}

{% hint style="danger" %}
Never call any of `Console.*` functions directly when mouse support or raw mode has been enabled in Linux systems. Otherwise, you'll get I/O errors and your application may crash unexpectedly!
{% endhint %}

## Helper functions

Terminaux's mouse feature provides you with a wide assortment of helper functions and properties to enable your Terminaux application to listen to the mouse events. The following functions and properties are available:

* `Input.InvertScrollYAxis`: Inverts the Y axis for vertical scrolling
* `Input.SwapLeftRightButtons`: Swaps the left/right mouse buttons
* `Input.DoubleClickTimeout`: Specifies the double clock timeout in a time span
* `Input.EnableMovementEvents`: Enables or disables the mouse movement events
* `Input.PointerEncoding`: Specifies the pointer encoding to use
* `Input.ReadPointerOrKey()`: Reads the next input event synchronously
* `Input.ReadPointerOrKeyNoBlock()`: Reads the next input event asynchronously
* `Input.ReadKey()`: Reads a single key synchronously
* `Input.ReadKeyTimeout()`: Reads a single key synchronously with timeout
* `Input.InvalidateInput()`: Invalidates all input events

The easiest way to listen to both the mouse and the keyboard events is this:

```csharp
// ...your code, usually screen rendering
InputEventInfo data = Input.ReadPointerOrKey();
var mouse = data.PointerEventContext;
if (mouse is not null)
{
    // Mouse input received.
    switch (mouse.Button)
    {
        // Insert case statements here...
    }
}
else if (data.ConsoleKeyInfo is ConsoleKeyInfo cki && !Input.PointerActive))
{
    // Keyboard input received.
    switch (cki.Key)
    {
        // Insert case statements here...
    }
}
```

The topmost `if` statement is the mouse event, and the bottommost `if` statement is the keyboard event. However, you may want to filter some events based on the button press state. For example, if you want to listen to left-clicks but don't want to listen to anything except when released, you can use this conditional:

<pre class="language-csharp"><code class="lang-csharp">switch (mouse.Button)
{
    case PointerButton.Left:
<strong>        if (mouse.ButtonPress != PointerButtonPress.Released)
</strong><strong>            break;
</strong>        // ...
        break;
    // ...
}
</code></pre>

{% hint style="warning" %}
You must not directly stop the listener right after a mouse click has been detected without listening to the `Released` event, or Windows's listener might still think that a mouse button has been pressed when it's not.
{% endhint %}

## Pointer tools

In addition to that, Terminaux provides the pointer tools that allow you to perform various operations to make building your console TUI applications easier. You can find the pointer tools in the `PointerTools` class, which are listed below:

* `PointerWithinPoint()`: This function returns `true` if the mouse pointer (click, movement, etc.) is found within a single point. For example, it returns `true` if your mouse pointer at `(22, 4)` matches the provided point position `(22, 4)`.
* `PointerWithinRange()`: This function returns `true` if the mouse pointer is found within a block range of the two points that form an invisible rectangle. For example, if you've specified `(22, 4)` and `(33, 6)` as two point ranges, and your mouse pointer has clicked on position `(25, 5)`, this function returns `true`.

## Pointer Hitboxes

You can define an invisible rectangle that resembles a mouse hitbox that allows your application to react to a mouse event that is within the hitbox. Using the `PointerHitbox` class, you can use the constructor that allows you to specify a hitbox in one of the following ways:

* Point: A single terminal cell in which to make it react to the mouse event.
* Start and End positions: Defines two positions in which to make a rectangle using the upper left corner (start) and the lower right corner (end).
* Start and Size positions: Defines a start position that resembles the upper left corner and a size of a rectangle.

{% hint style="info" %}
A callback is required to be specified, but you can choose between a delegate that returns void and a delegate that returns an object.
{% endhint %}

Later, you can set the following properties:

* `Button`: Specifies which button(s) to listen to (default: the mouse left button)
* `ButtonPress`: Specifies how the button is pressed (default: the released state)
* `Modifiers`: Specifies what modifiers (SHIFT, ALT, CTRL) to listen to (default: none)
* `Dragging`: Specifies whether mouse dragging is required (default: `false`)
* `ProcessTier`: Specifies whether to process the tier for double-clicks and other tiered clicks (default: `false`)
* `ClickTier`: Specifies the exact number of clicks required to trigger the action if `ProcessTier` is on (default: 0)

Useful functions, such as `ProcessPointer()`, can be used when processing the mouse event to perform an action when certain requirements of a hitbox are met. The following functions are available:

* `IsPointerWithin()`: Specifies whether the mouse event context describes a mouse position that is within the boundaries of the hitbox.
* `IsPointerModifierMatch()`: Specifies whether the hitbox requirements are met by the mouse event, such as modifiers, button, and button press state.
* `ProcessPointer()`: Checks for boundaries and for requirements before executing the function delegate that was passed to the constructor.

Here's how you can easily process a pointer from a hitbox in the mouse event processing code:

```csharp
// Obtain the start and the end coordinates...

// Make a new hitbox instance
var hitbox = new PointerHitbox(
    new(startX, startY),
    new Coordinate(endX, endY),
    (pec) => DoAction(pec)
)
{
    Button = PointerButton.WheelUp | PointerButton.WheelDown,
    ButtonPress = PointerButtonPress.Scrolled
};

// Some processing code...

// Process the pointer event
if (hitbox.IsPointerWithin(mouse))
{
    hitbox.ProcessPointer(mouse, out bool status);
    // If the IsPointerWithin if conditional is omitted, you can check the status
    // variable to determine whether the pointer is processed.
}
```

{% hint style="info" %}
Usually, a call to `IsPointerModifierMatch()` is not needed, since `ProcessPointer()` calls it implicitly.
{% endhint %}

### Hitboxes and Selection Renderer

The selection cyclic writer can now provide you with the available hitboxes for you to process them in custom interactive applications. Internally, Terminaux uses this method to determine the item number to process in selection infoboxes, in selection style TUIs, and in interactive selector TUIs when a mouse event has been received. You can get this information for each choice using the `GenerateSelectionHitboxes()` function (you can optionally specify the zero-based choice index), which gives you an array of a tuple that contains the following values:

* `hitbox`: A generated hitbox that is connected to the text representation of either a category, a group, or a choice.
* `type`: One of `Category`, `Group`, or `Choice`.
* `related`: A one-based number of a choice that is processed. No incrementation occurs in categories and groups, but it increases for every choice.

{% hint style="info" %}
`related` doesn't describe the choice number in a group or a category, but it describes the choice number out of all choices extracted from groups and categories.
{% endhint %}

You can also use the following functions:

* `GenerateSelectionHitbox()`: Generates a list of hitboxes according to either the current choice position or a specified choice index, and returns a specified hitbox using an index.
* `CanGenerateSelectionHitbox()`: Checks to see whether a call to `GenerateSelectionHitbox()` is possible by checking the indexes as described above.
