---
description: May I read what you've written, please?
icon: book-open
---

# Input Reader

This functionality is an important part of any interactive console application, because it gives users a chance to input what they want to write to the console.

In case you want to listen to mouse events, you can consult the below page:

{% content-ref url="pointer-events.md" %}
[pointer-events.md](pointer-events.md)
{% endcontent-ref %}

In case you want to use something other than the reader, you can consult the other input tools defined in the below page:

{% content-ref url="other-input/" %}
[other-input](other-input/)
{% endcontent-ref %}

{% hint style="info" %}
If you're making your own mod in Nitrocid KS, it's best to use its own `Input` class instead of Terminaux's `TermReader`, as the class there actually deals with the screensaver in most circumstances.
{% endhint %}

***

## <mark style="color:$primary;">The Input Reader</mark>

You can easily use this feature in any interactive console application that uses Terminaux. Just use the `Terminaux.Reader.TermReader` class that contains the `Read()` functions and their overloaded versions.

{% hint style="info" %}
The reader not only provides the static text version for input prompts, but also the dynamic text version. Just create a simple function delegate that generates a string as the first argument, like this:

```csharp
string input = TermReader.Read(() => $"{DateTime.Now} [{TermReaderState.CurrentState.CurrentTextPos}]\n> ", "Hello World!", false, false, false);
```
{% endhint %}

{% hint style="info" %}
Please note that they are interruptible by default. If you want the input to be non-interruptible, you can set the `interruptible` argument to false.

You can access the global reader settings by referencing the `GlobalReaderSettings` found in the `TermReader` class.
{% endhint %}

### <mark style="color:$primary;">`Read()`</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">and generic value types</mark>

You can use the `Read()` function with a generic type that resembles a simple type, such as integers, characters, and strings. This is to simplify type conversion that may be needed for certain inputs, such as entering a number or a fractional number.

The below example demonstrates how to use this feature:

{% code title="PromptNumber.cs" lineNumbers="true" %}
```csharp
int input = TermReader.Read<int>("Write a number: ", "5", false, false, false);
TextWriterColor.Write("Number is: " + input);
```
{% endcode %}

{% hint style="warning" %}
You can't use complex types as a generic type, such as JSON nodes.
{% endhint %}

***

## <mark style="color:$primary;">Keybindings</mark>

Terminal reader offers a set of built-in keybindings, as well as custom keybindings.

<details>

<summary>Built-in bindings</summary>

The terminal reader contains main keybindings, as well as customizable keybindings that you can define to your accord.

Any key will append the selected characters to the current text input, and `RETURN` will accept the input. You can also override the keybindings of one of the below actions, as long as they're not listed in red. The below keybindings are available:

<table><thead><tr><th width="196.5">Keybinding</th><th>Action</th></tr></thead><tbody><tr><td><code>ENTER</code></td><td><mark style="color:red;">Accepts input</mark></td></tr><tr><td><code>Ctrl</code>+<code>C</code></td><td><mark style="color:red;">Cancels reading (if <code>TreatCtrlCAsInput</code> is enabled)</mark></td></tr><tr><td><code>Ctrl</code>+<code>A</code> / <code>HOME</code></td><td>Beginning of line</td></tr><tr><td><code>Ctrl</code>+<code>E</code> / <code>END</code></td><td>End of line</td></tr><tr><td><code>Ctrl</code>+<code>B</code> / <code>←</code></td><td>Backward one character</td></tr><tr><td><code>Ctrl</code>+<code>F</code> / <code>→</code></td><td>Forward one character</td></tr><tr><td><code>BACKSPACE</code></td><td>Remove one character from the left</td></tr><tr><td><code>UP ARROW</code></td><td>Get the older input</td></tr><tr><td><code>DOWN ARROW</code></td><td>Get the newer input</td></tr><tr><td><code>DELETE</code></td><td>Remove one character in current position</td></tr><tr><td><code>ALT</code>+<code>B</code></td><td>One word backward</td></tr><tr><td><code>ALT</code>+<code>F</code></td><td>One word forward</td></tr><tr><td><code>TAB</code></td><td>Next auto-completion entry (if there is one)</td></tr><tr><td></td><td>Insert four spaces (if no autocompletions)</td></tr><tr><td><code>SHIFT</code>+<code>TAB</code></td><td>Previous auto-completion entry</td></tr><tr><td><code>CTRL</code>+<code>U</code></td><td>Cut to the start of the line</td></tr><tr><td><code>CTRL</code>+<code>K</code></td><td>Cut to the end of the line</td></tr><tr><td><code>CTRL</code>+<code>W</code></td><td>Cut to the end of the previous word</td></tr><tr><td><code>ALT</code>+<code>D</code></td><td>Cut to the end of the next word</td></tr><tr><td><code>CTRL</code>+<code>Y</code></td><td>Yank the cut content</td></tr><tr><td><code>Alt</code>+<code>L</code></td><td>Make word lowercase</td></tr><tr><td><code>Alt</code>+<code>U</code></td><td>Make word UPPERCASE</td></tr><tr><td><code>Ctrl</code>+<code>Alt</code>+<code>L</code></td><td>Make input lowercase</td></tr><tr><td><code>Ctrl</code>+<code>Alt</code>+<code>U</code></td><td>Make input UPPERCASE</td></tr><tr><td><code>Alt</code>+<code>C</code></td><td>Make character uppercase and move to the end of word</td></tr><tr><td><code>Alt</code>+<code>V</code></td><td>Make character lowercase and move to the end of word</td></tr><tr><td><code>Alt</code>+<code>S</code></td><td>Shows all suggestions in the style akin to the Bourne Again SHell (bash)</td></tr><tr><td><code>Alt</code>+<code>R</code></td><td>Refreshes the prompt, the text input, and the current cursor position.</td></tr><tr><td><code>Insert</code></td><td>Text append mode (Insert or append)</td></tr><tr><td><code>CTRL</code>+<code>L</code></td><td>Clears the screen and refreshes the prompt.</td></tr><tr><td><code>ALT</code>+<code>\</code></td><td>Cut the whitespaces before and after the character.</td></tr><tr><td><code>CTRL</code>+<code>T</code></td><td>Substitutes two characters</td></tr><tr><td><code>ALT</code>+<code>T</code></td><td>Substitutes two words</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>#</code></td><td>Makes your current input text a comment (visual only, but ignores your text on submit)</td></tr><tr><td><code>ALT</code>+<code>TAB</code> / <code>CTRL</code>+<code>I</code></td><td>Forces the tab character to be written. Writes as spaces.</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>C</code></td><td>Temporarily conceals or reveals the whole input in normal prompts.</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>&#x3C;</code></td><td>Goes to the first history entry</td></tr><tr><td><code>ALT</code>+<code>SHIFT</code>+<code>></code></td><td>Goes to the last history entry</td></tr><tr><td><code>CTRL</code>+<code>SHIFT</code>+<code>_</code></td><td>Undos a change</td></tr><tr><td><code>ALT</code>+<code>R</code></td><td>Undos all changes</td></tr><tr><td><code>ALT</code>+<code>0-9</code></td><td>Argument value</td></tr><tr><td><code>ALT</code>+<code>-</code></td><td>Argument negation</td></tr></tbody></table>

{% hint style="warning" %}
**Warning:** Some of the keys conflict with the terminal emulator and/or the operating system keybindings.
{% endhint %}

</details>

<details>

<summary>Custom bindings</summary>

The input reader supports custom bindings, which you can assign your `BaseBinding` class containing the following functions you must override:

| Property/function | Description                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| `BoundKeys`       | This holds all the keys to bind your custom action to.                                                            |
| `DoAction()`      | This is the heart of your key binding. You can do anything with the text using the current terminal reader state. |

You can also override these:

| Property/function            | Description                                                                                                                                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `IsExit`                     | If `true`, causes the reader to assume that the input is done after executing the action that this binding implements.                                                                     |
| `BindMatched()`              | Specify your own method on how to check to see if the input key matched all the bound keys (`BoundKeys`) in your custom key binding.                                                       |
| `ResetSuggestionsTextPos`    | Specifies whether to reset the text position saved for the suggestions. This is usually enabled for custom bindings that have to do with the suggestions.                                  |
| `IsBindingOverridable`       | Specifies whether applications can override the keybinding with custom keybindings or not. You can call `Override()` and `RemoveOverride()` to perform this action.                        |
| `AppendsChangesList`         | Specifies whether the keybinding appends the changes to the list of changes from the reader state.                                                                                         |
| `ArgumentNumberIsRepetition` | Specifies whether the argument number represents repeated action. If false, you must either handle the argument number using the `ArgumentNumber` property, or ignore the argument number. |

</details>

<details>

<summary>Principles of custom bindings</summary>

Your keybinding must follow the below principles:

* For text positioning, you must use any function in the `PositioningTools` class.
* For manual console manipulation, you must use any function in the `ConsoleWrapper` class.
* Your bound key must not be already bound to a key that was already bound by either a base or another custom binding, or two bindings execute at the same time, potentially causing conflict.
* To manipulate with text, you must use the `state.CurrentText` property. You must refresh the prompt thereafter.
* To refresh the prompt, you must use the `TermReaderTools.Refresh()` function.

At the end, your base class must look like this at minimum:

```csharp
using System;
using Terminaux.Reader;
using Terminaux.Reader.Tools;

namespace MyApp
{
    internal class MyBinding : BaseBinding, IBinding
    {
        public override ConsoleKeyInfo[] BoundKeys { get; } = 
        {
            // All keys listed below will lead to the below DoAction being executed.
            new ConsoleKeyInfo('\0', ConsoleKey.Key, false, false, false)
        };

        public override void DoAction(TermReaderState state)
        {
            // Your action.
            state.RefreshRequired = true;
        }
    }
}
```

where:

* `ConsoleKey.Key`
  * Any console key. Consult the [documentation](https://learn.microsoft.com/en-us/dotnet/api/system.consolekey) for more info.
* `\0`
  * A character that must match the corresponding `ConsoleKey.Key`.

{% hint style="info" %}
If you're assigning a key containing `CTRL`, you must assign a character number starting from `0x0`. For example, `CTRL+Y` is `\u0019`.
{% endhint %}

</details>

<details>

<summary>How to bind and unbind a custom binding</summary>

Once you created a base class as mentioned above, you can finally use the `BindingsTools` class to call the `Bind(BaseBinding)` function to add your own binding to the custom binding store, like this:

```csharp
BindingsTools.Bind(new MyBinding());
```

{% hint style="warning" %}
**Warning:** You must call this function once. It does nothing if your binding is already installed.
{% endhint %}

To remove binding, you must use the `Unbind(ConsoleKeyInfo)` command.

</details>

***

## <mark style="color:$primary;">History tools</mark>

Terminaux applications can access tools that are related to the shell history using the `HistoryTools` class found in the `Terminaux.Reader.History` namespace. It contains a variety of tools that are easy to use. This class contains functions that allow you to manipulate with the history.

<details>

<summary>Loading, saving, and unloading</summary>

You can load the history instances using one of the following functions:

<table><thead><tr><th width="180.33331298828125">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>LoadFromFile()</code></td><td>This loads the histories from a JSON file</td></tr><tr><td><code>LoadFromJson()</code></td><td>This loads the histories from a JSON string</td></tr><tr><td><code>LoadFromInstance()</code></td><td>This loads the histories from a <code>HistoryInfo</code> instance</td></tr></tbody></table>

Once history instances get loaded, you can use the histories that may contain history entries for the terminal reader by setting the history name in the terminal reader settings using the following properties:

<table><thead><tr><th width="149.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>HistoryEnabled</code></td><td>Whether the history is enabled or not</td></tr><tr><td><code>HistoryName</code></td><td>Select what history to use</td></tr></tbody></table>

***

You can save the history instances to a JSON representation using one of the following functions:

<table><thead><tr><th width="240.3333740234375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>SaveToString(string)</code></td><td>Saves the history instance to a JSON string using the history name</td></tr><tr><td><code>SaveToString(HistoryInfo)</code></td><td>Saves the history instance to a JSON string using the history info instance</td></tr></tbody></table>

***

Once you're done with those instances, you can unload them using one of the following functions:

<table><thead><tr><th width="240.3333740234375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>Unload(string)</code></td><td>Removes the history instance from the list of registered instances using the history name</td></tr><tr><td><code>Unload(HistoryInfo)</code></td><td>Removes the history instance from the list of registered instances using the history info instance</td></tr></tbody></table>

</details>

<details>

<summary><code>HistoryInfo</code> instance</summary>

The history info class can be constructed using a name and a list of history entries that will be pre-installed. This class contains the following properties:

<table><thead><tr><th width="149.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>HistoryName</code></td><td>Name of the history</td></tr><tr><td><code>HistoryEntries</code></td><td>A list of history entries</td></tr></tbody></table>

</details>

<details>

<summary>History entry manipulation</summary>

In addition to the above tools, you can also manipulate with the history entry list using one of the following functions and properties:

<table><thead><tr><th width="209.6666259765625">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>HistoryNames</code></td><td>Returns the name of all the history instances</td></tr><tr><td><code>GetHistoryEntries()</code></td><td>Gets an array of history entries that are added to the history instance</td></tr><tr><td><code>IsHistoryRegistered()</code></td><td>Checks to see whether a history instance is registered or not</td></tr><tr><td><code>Switch()</code></td><td>Replaces the list of history entries with a new set of entries</td></tr><tr><td><code>Append()</code></td><td>Appends a new history entry at the end of the list</td></tr><tr><td><code>Insert()</code></td><td>Inserts a new history entry somewhere in the list using a specified index</td></tr><tr><td><code>Remove()</code></td><td>Removes a history entry using a specified index</td></tr><tr><td><code>Clear()</code></td><td>Clears the list of history entries in a history instance</td></tr></tbody></table>

{% hint style="info" %}
The above functions require that a target history entry be registered, with the exception of `HistoryNames` and `IsHistoryRegistered()`.
{% endhint %}

</details>

***

## <mark style="color:$primary;">Extra functions</mark>

If you want to explore deeper than the surface, expand a section.

<details>

<summary>The reader state</summary>

Each one of these functions creates a reader state, `TermReaderState`, that contains essential information about the current reader state, including, but not limited to:

* Current text
* Input prompt text
* Current text position
* Kill buffer
* Reader settings

You can also check to see if the console reader facility is busy getting input or not. The property, `Busy`, indicates this by returning `true` if there is input to be entered by the user.

You can learn more about the reader state here:

<a href="reader-state.md" class="button primary">Reader State</a>

{% hint style="info" %}
The property, `IsReaderBusy`, only checks to see if the input reader is in use, while the `Busy` property checks to see if the code is waiting for input anywhere.

If you want to wait for user input to finish, you can call the `WaitForInput()` function in the `TermReaderTools` class.
{% endhint %}

</details>

<details>

<summary>Positioning tools</summary>

Your custom bindings can now change the cursor positioning when manipulating with text so that it becomes easier to make your custom bindings that use positioning tools.

Here are the functions you can use:

<table><thead><tr><th width="150.33331298828125">Function</th><th></th></tr></thead><tbody><tr><td><code>GoLeftmost()</code></td><td>Changes the word position to the leftmost position, that is, the first letter.</td></tr><tr><td><code>GoRightmost()</code></td><td>Changes the word position to the rightmost position, that is, the last letter.</td></tr><tr><td><code>GoForward()</code></td><td>Changes the word position a step or a specified number of steps closer to the end of the text.</td></tr><tr><td><code>GoBack()</code></td><td>Changes the word position a step or a specified number of steps closer to the beginning of the text.</td></tr><tr><td><code>SeekTo()</code></td><td>Changes the word position to the selected zero-based position.</td></tr></tbody></table>

Once you're done changing positions, if you need to verify that you've changed the position to the correct position, you can use the `Commit()` function.

{% hint style="info" %}
It's not necessary to use the `Commit()` function at the end of each custom binding, because the reader uses this function automatically based on whether to update the position or not.
{% endhint %}

</details>

<details>

<summary>Writers and writing</summary>

You can use the text writers with the current reader settings by using `TextWriterColor`'s `WriteForReader()` and its siblings, passing it the reader settings to take care of the margins.

### <mark style="color:$primary;">Writing tools</mark>

You can also append or remove a string in the `TermReaderTools` class. This means that you can either append text to the end of the input, insert text to the current position, or remove text from either the current position or from a specific character index from the input string.

These are the functions that you can use:

<table><thead><tr><th width="229.6666259765625">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>GetMaximumInputLength()</code></td><td>Gets the maximum input length according to available space</td></tr><tr><td><code>InsertNewText()</code></td><td>Inserts a new text to the input</td></tr><tr><td><code>RemoveText()</code></td><td>Removes text from the input</td></tr></tbody></table>

</details>

<details>

<summary>Conditionals</summary>

In addition to all the above features, you can also make your key binding beep under certain circumstances, such as if the current text position is at the start of the text and we're trying to move left, using one of the two conditional functions from the `ConditionalTools` class:

<table><thead><tr><th width="130.3333740234375">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>ShouldNot()</code></td><td>Specifies that the specified condition should not be true</td></tr><tr><td><code>Should()</code></td><td>Specifies that the specified condition should not be false</td></tr></tbody></table>

{% hint style="info" %}
If either of these functions' assertions have failed, either your computer or your speakers emits a beep sound upon going back to the input mode.
{% endhint %}

{% hint style="info" %}
In order to set `OperationWasInvalid` to true, you must put a conditional return by referring to the `ConditionalTools` and negating the `ShouldNot` condition like this:

```csharp
// If we're at the start of the text, bail.
if (!ConditionalTools.ShouldNot(state.CurrentTextPos == 0, state))
    return;
```
{% endhint %}

</details>
