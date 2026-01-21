---
description: Checking to see if you're working on the right console...
icon: badge-check
---

# Console Checker

Sometimes, your console application might need some of the additional features, such as VT color sequences, that might not be available to all of the terminal emulators installed on your system. In this case, Terminaux provides you with a group of console checkers to ease the process of checking the features.

## Checking

There are various checkers you'll be able to use with Terminaux:

* Dumb console checker
* 256 color support checker
* Size requirement checker
* ConHost checker

### Dumb console

Dumb consoles, in general, don't understand the concept of colors, positioning, erasing, and features other than writing to the console. However, some of them may be provided with basic color support and various other features that are simple.

The dumb console checker checks to see if we can call the `CursorLeft` property and the `WindowWidth` property inside the exception handler. Usually, this property throws an `IOException` if it can't query the terminal for the cursor left position, but this happens when the console doesn't understand the concept of positioning, which happens on dumb consoles.

If the `isatty()` check on Linux fails (that is, the function doesn't return 1), the console is considered to be a dumb console. Additionally, redirected output and/or error standard streams are what makes Terminaux assume that it's a dumb console.

If checks pass, the console is not considered as dumb. This means that Terminaux is able to use the color features and all the other features.

### 256-color support checker

This checker is a very simple checker, because it only queries the terminal type for a string containing `-256col`. Any terminal type that doesn't have this string is assumed to be not supporting 256-color feature, but that doesn't necessarily mean that the console doesn't support this feature, depending on your console application. Use `IsConsole256Colors()` to use this checker.

### Size checker

The console size requirement checking has been recently added to the latest version of Terminaux. This assists your application in determining your minimum console size requirements, with the default being the standard [80x24](https://softwareengineering.stackexchange.com/a/148765) console size for historical reasons.

For console applications which require a specific console size, such as Nitrocid KS which requires the above console size, you can call the `CheckConsoleSize()` function. You can also make it check for sizes other than 80x24, such as 120x30, by passing the width and the height to the same function.

```csharp
public static bool CheckConsoleSize(int minimumWidth = 80, int minimumHeight = 24)
```

This function is found in the `ConsoleChecker` public class so that you can access this function easily.

If you call this function, it first checks to see if there are edge cases regarding running such console application on terminal multiplexers.

* If you're running your console application on TMUX, it queries the TMUX settings to get how many status lines are there, and subtracts the required height by the number of status lines.
* If you're running your console application on GNU Screen, it subtracts the minimum height by one, since it doesn't use more than one line to print its status if enabled.
* If you're running your console application without a terminal multiplexer, it doesn't subtract the minimum height and stays as-is.

Once the terminal multiplexer check is done, it queries the current window width and the window height and compares them to the required width and height. If the requirement is not satisfied, the function returns false. Otherwise, it returns true as the requirements have been satisfied.

The `CheckConsoleSizePrompt()` function checks to see if the console size requirements have been satisfied. If the requirements aren't satisfied, the application tells you to resize your console window to have a better experience and to press `ENTER` when resized to the required size.

### ConHost checker

This checker is a very simple checker that checks to see if your Terminaux application is being run in a Windows console that uses ConHost as the console rendering backend. Note that it always returns false on non-Windows systems. Use `IsConHost()` to use this checker.
