---
description: Define this command for me!
icon: info
---

# Command Information

The shell provides a class that stores information about a specific command that you've created to make your own rules.

***

## <mark style="color:$primary;">Implementation of command information</mark>

To implement your command, you must make a new instance of the command information class that will store general and optional information about your command.

<details>

<summary>Implementing <code>CommandInfo</code></summary>

Each command you define in your shell must provide a new instance of the `CommandInfo` class holding details about the specified command. The new instance of the class can be made using one of the constructors defined below:

```csharp
public CommandInfo(string Command, string HelpDefinition, CommandArgumentInfo[] CommandArgumentInfo, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
public CommandInfo(string Command, string HelpDefinition, BaseCommand CommandBase, CommandFlags Flags = CommandFlags.None)
```

where:

| Variable              | Description                                                               |
| --------------------- | ------------------------------------------------------------------------- |
| `Command`             | The command                                                               |
| `HelpDefinition`      | The brief summary of what the command does                                |
| `CommandArgumentInfo` | Array of argument information about your command (can be omitted)         |
| `CommandBase`         | An instance of the `BaseCommand` containing command execution information |
| `CommandFlags`        | All command flags                                                         |

{% hint style="info" %}
You can omit the array definition of `CommandArgumentInfo` instances to create parameterless commands more easily.
{% endhint %}

</details>

<details>

<summary>Implementing <code>CommandArgumentInfo</code></summary>

To implement `CommandArgumentInfo`, call the constructor either with no parameters, which implies that there is no argument required to run this command, or with the following options listed below.

```csharp
public CommandArgumentInfo()
public CommandArgumentInfo(bool AcceptsSet)
public CommandArgumentInfo(bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(CommandArgumentPart[] Arguments)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, bool AcceptsSet)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(SwitchInfo[] Switches)
public CommandArgumentInfo(SwitchInfo[] Switches, bool AcceptsSet)
public CommandArgumentInfo(SwitchInfo[] Switches, bool AcceptsSet, bool infiniteBounds)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches, bool AcceptsSet)
public CommandArgumentInfo(CommandArgumentPart[] Arguments, SwitchInfo[] Switches, bool AcceptsSet, bool infiniteBounds)
```

where:

| Variable         | Description                                           |
| ---------------- | ----------------------------------------------------- |
| `Arguments`      | Defines the command arguments                         |
| `Switches`       | Defines the command switches                          |
| `AcceptsSet`     | Whether to accept the `-set` switch                   |
| `infiniteBounds` | Whether to accept infinite number of arguments or not |

</details>

<details>

<summary>Implementing <code>CommandArgumentPart</code></summary>

For `CommandArgumentPart` instances, consult the below constructor to create an array of `CommandArgumentPart` instances when defining your commands:

```csharp
public CommandArgumentPart(bool argumentRequired, string argumentExpression, Func<string[], string[]> autoCompleter = null, bool isNumeric = false, string[] exactWording = null, string argumentDesc = "")
```

where:

| Variable             | Description                                                                                                                                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `argumentRequired`   | Is this argument part required?                                                                                                                                                                                            |
| `argumentExpression` | Command argument expression                                                                                                                                                                                                |
| `autoCompleter`      | <p>Auto completion function delegate<br></p><ul><li>The first <code>string[]</code> denotes the list of last passed arguments</li><li>The second <code>string[]</code> (output) denotes the suggestions returned</li></ul> |
| `isNumeric`          | Whether this argument part accepts numeric values only                                                                                                                                                                     |
| `exactWording`       | If not empty, the user must write one of the words declared in this variable for this argument to be satisfied                                                                                                             |
| `argumentDesc`       | Argument description that shows up in the help entry                                                                                                                                                                       |

In case you want to expressively specify the options without having to use default values for all parameters to set a certain parameter, you can use the `CommandArgumentPartOptions` overload:

```csharp
public CommandArgumentPart(bool argumentRequired, string argumentExpression, CommandArgumentPartOptions options)
```

where:

| Variable             | Description                      |
| -------------------- | -------------------------------- |
| `argumentRequired`   | Is this argument part required?  |
| `argumentExpression` | Command argument expression      |
| `options`            | Options of command argument part |

</details>

<details>

<summary>Implementing stricter argument checker</summary>

For commands that require more than just simple argument checking as specified in the `CommandArgumentPart` instances, you can use the `ArgChecker` property to set it to a function delegate that checks all the arguments, with the command parameter info as the first argument.

Such functions must return 0 to continue execution. Else, the command execution will not continue and the last error code will be set to what the function returns.

This is an example for the `alarm` command:

<pre class="language-csharp" data-title="CommandInfo for alarm" data-line-numbers><code class="lang-csharp">new CommandInfo("alarm", /* Localizable */ "Manage your alarms",
    [
        new CommandArgumentInfo(
        [
            (...)
        ])
        {
<strong>            ArgChecker = (cp) => AlarmCommand.CheckArgument(cp, "start")
</strong>        },
        (...)
    ], new AlarmCommand(), CommandFlags.Strict),
</code></pre>

{% code title="Alarm command code" lineNumbers="true" %}
```csharp
internal static int CheckArgument(CommandParameters parameters)
{
    (...)
}
```
{% endcode %}

</details>

***

## <mark style="color:$primary;">Auto-completion for commands</mark>

Commands can have auto-completion set up, so that program users can use their TAB key as means to automatically complete the expression for a command, depending on argument positioning.

<details>

<summary>How the shell selects a completer</summary>

The shell, when TAB is pressed, will select one of the following completers:

* If the auto completer is specified, then, regardless of whether the expression represents the selection (expressions containing the slash `/` character) or not, the auto completer specified in the constructor will be called.
* If the auto completer is not specified, then it will go through the following completers:
  * The shell goes through the list of known completion expressions according to the argument expression, which are the following:
    * `cmd`, `command`: List of all available commands
    * `shell`: List of all available shells
    * `$variable`: List of all MESH variables
  * If the expression is not listed in any of the known expressions list, it'll check for the selection indicator characters (the slash `/` key).
    * For example, the `true/false` expression will generate an auto completer that completes the two words: `true` and `false`.
  * In case there is none, the shell will use the default auto completer, which fetches possible files and folders on your current working directory.

</details>

<details>

<summary>Manipulating with completion functions</summary>

The known expressions list can be manipulated, by registering and unregistering a completion expression. You can use one of the following functions found in the `CommandAutoCompletionList` class:

<table><thead><tr><th width="299.666748046875">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>RegisterCompletionFunction()</code></td><td>Registers the completion function using a name and a function that returns a list of possible completions.</td></tr><tr><td><code>UnregisterCompletionFunction()</code></td><td>Unregisters the completion function by name</td></tr><tr><td><code>IsCompletionFunctionRegistered()</code></td><td>Determines whether the completion function is registered or not</td></tr><tr><td><code>IsCompletionFunctionBuiltin()</code></td><td>Determines whether the completion function is registered as a built-in completer or not</td></tr></tbody></table>

</details>

<details>

<summary>Example of completion implementation</summary>

Here's a simple example as to how to define such completion function:

<pre class="language-csharp"><code class="lang-csharp"><strong>// Completion function registration
</strong><strong>CommandAutoCompletionList.RegisterCompletionFunction("text", (_) => { return ["Hello", "Hi"]; });
</strong>
ShellManager.RegisterShell("TestShell", new TestShellInfo());
ShellManager.StartShell("TestShell");
ShellManager.UnregisterShell("TestShell");

<strong>// Completion function unregistration
</strong><strong>CommandAutoCompletionList.UnregisterCompletionFunction("text");
</strong></code></pre>

</details>
