---
description: Talks about shell scripting and how it works
icon: scroll-old
---

# Shell Scripting

<figure><img src="../../../../.gitbook/assets/image (173).png" alt=""><figcaption></figcaption></figure>

The MESH shell contains scripting support. The shell scripts have the `.mesh` extension containing a subset of MESH commands inside it. A simple MESH script containing a command that sets a MESH variable is as follows:

```
set -set=hellotext Hello
echo <$:hellotext>
```

***

## <mark style="color:$primary;">Script parser</mark>

When this script file is executed, the following steps are taken:

{% stepper %}
{% step %}
### <mark style="color:$primary;">Initializes the variables</mark>

The MESH script parser skims the file for any possible `$variables` and initializes them with their default values using the `MESHVariables.InitializeVariable` function.
{% endstep %}

{% step %}
### <mark style="color:$primary;">Replaces the variables</mark>

The parser then attempts to skim the script lines for all the variables, and replaces them with the value.
{% endstep %}

{% step %}
### <mark style="color:$primary;">Replaces the placeholders</mark>

The parser also attempts to parse the script argument placeholders, defined with `{num}`; which `num` is the argument number, in case the ︎user executed the script with the arguments. For example, this script prints the first argument:

```
echo {0}
```
{% endstep %}

{% step %}
### <mark style="color:$primary;">Executes the final line</mark>

As soon as the parsing is done, the final line gets executed by the `GetLine()` command.
{% endstep %}
{% endstepper %}

***

## <mark style="color:$primary;">Variables</mark>

<figure><img src="../../../../.gitbook/assets/111-shell.png" alt=""><figcaption></figcaption></figure>

MESH provides the variable facility, which holds the variable as a key and the variable value as a value. Each variable starts with the dollar sign like `$var`, regardless of the platform.

When a variable gets initialized by `InitializeVariable()`, the variable name gets sanitized (`SanitizeVariableName()`) by appending the dollar sign in front of the variable name, which then gets initialized with the empty value.

The variable can be read from and written to by these respective functions: `GetVariable()` and `SetVariable()`. Additionally, an array of values can be initialized with one variable by `SetVariables()` to initialize `$var[n]` variables, which:

* `var`: A variable name
* `n`: How many values are there (count from 0)

{% hint style="warning" %}
Additionally, the variables can be uninitialized by the `RemoveVariable()` function. When the target variable is removed, it has to be re-initialized before it can be used again.
{% endhint %}

***

## <mark style="color:$primary;">Conditions</mark>

No scripting is complete with conditions, which control the execution of the command.

<details>

<summary>Available conditions</summary>

These conditions are currently available to be used: (`<value>` can either be a constant or a MESH `$variable`)

<table><thead><tr><th width="120">Condition</th><th width="319.666748046875">Description</th><th>Usage</th></tr></thead><tbody><tr><td><code>eq</code></td><td>The value is equal to the value</td><td><code>&#x3C;value> eq &#x3C;value></code></td></tr><tr><td><code>neq</code></td><td>The value is not equal to the value</td><td><code>&#x3C;value> neq &#x3C;value></code></td></tr><tr><td><code>les</code></td><td>The number is less than another number</td><td><code>&#x3C;value> les &#x3C;value></code></td></tr><tr><td><code>lesoreq</code></td><td>The number is less than or equal to another number</td><td><code>&#x3C;value> lesoreq &#x3C;value></code></td></tr><tr><td><code>gre</code></td><td>The number is greater than another number</td><td><code>&#x3C;value> gre &#x3C;value></code></td></tr><tr><td><code>greoreq</code></td><td>The number is greater than or equal to another number</td><td><code>&#x3C;value> greoreq &#x3C;value></code></td></tr><tr><td><code>fileex</code></td><td>The file exists</td><td><code>fileex &#x3C;value></code></td></tr><tr><td><code>filenex</code></td><td>The file doesn't exist</td><td><code>filenex &#x3C;value></code></td></tr><tr><td><code>direx</code></td><td>The directory exists</td><td><code>direx &#x3C;value></code></td></tr><tr><td><code>dirnex</code></td><td>The directory doesn't exist</td><td><code>dirnex &#x3C;value></code></td></tr><tr><td><code>has</code></td><td>The specified string contains a substring</td><td><code>&#x3C;value> has &#x3C;value></code></td></tr><tr><td><code>hasno</code></td><td>The specified string doesn't contain a substring</td><td><code>&#x3C;value> hasno &#x3C;value></code></td></tr><tr><td><code>ispath</code></td><td>The specified path is valid</td><td><code>&#x3C;value> ispath</code></td></tr><tr><td><code>isnotpath</code></td><td>The specified path is invalid</td><td><code>&#x3C;value> isnotpath</code></td></tr><tr><td><code>isfname</code></td><td>The specified file name is valid</td><td><code>&#x3C;value> isfname</code></td></tr><tr><td><code>isnotfname</code></td><td>The specified file name is invalid</td><td><code>&#x3C;value> isnotfname</code></td></tr><tr><td><code>is</code></td><td>The variable is of the appropriate type</td><td><code>&#x3C;value> is &#x3C;type</code></td></tr><tr><td><code>isnot</code></td><td>The variable is not of the appropriate type</td><td><code>&#x3C;value> isnot &#x3C;type</code></td></tr><tr><td><code>isplat</code></td><td>The host is running a selected platform</td><td><code>isplat &#x3C;platform></code></td></tr><tr><td><code>isnotplat</code></td><td>The host is not running a selected platform</td><td><code>isnotplat &#x3C;platform></code></td></tr></tbody></table>

{% hint style="info" %}
`<type>` can be one of the following:

* `null`
* `string, fullstring`
* `numeric`
* `byte, i8, ubyte, u8`
* `int16, short, i16, uint16, ushort, u16`
* `int32, integer, i32, uint32, uinteger, u32`
* `int64, long, i64, uint64, ulong, u64`
* `float, f32, double, f64, decimal`
* `bool`
* `regex`

`<platform>` can be one of the following:

* `win`: Windows platforms (Windows 10, 11, ...)
* `mac`: Macintosh platforms (macOS Catalina, Big Sur, ...)
* `unix`: Unix flavors (Linux, ...)
* `android`: Android phones and tablets (Android 13, 14, ...)
{% endhint %}

</details>

<details>

<summary>Implementation</summary>

The conditions all have their base condition class and their interface to be implemented like below:

```csharp
public class YourCondition : BaseCondition, ICondition
```

Basically, you must override all the variables, where:

* `ConditionName`: A condition name without spaces to be included in the expression

```csharp
public override string ConditionName => "dirnex";
```

* `ConditionPosition`: Which word number starting from 1 should the expression be found?

```csharp
public override int ConditionPosition { get; } = 1;
```

* `ConditionRequiredArguments`: How many arguments are required? Starting from 1.

```csharp
public override int ConditionRequiredArguments { get; } = 2;
```

Choose one of the two method overloads to override, depending on your condition:

* `IsConditionSatisfied(string FirstVariable, string SecondVariable)`
  * This function checks the two variables to see if they satisfy a condition

```csharp
public override bool IsConditionSatisfied(string FirstVariable, string SecondVariable)
```

* `IsConditionSatisfied(string[] Variables)`
  * This function checks any number of variables to see if they satisfy a condition

```csharp
public override bool IsConditionSatisfied(string[] Variables)
```

You can call `ConditionSatisfied()` to test any built-in or custom condition. Give it any expression and test it with `true`.

The final class file should look like this:

{% code lineNumbers="true" %}
```csharp
using Terminaux.Shell.Scripting.Conditions;

public class MyCondition : BaseCondition, ICondition
{
    public override string ConditionName => "custom";
    public override int ConditionPosition { get; } = 2;
    public override int ConditionRequiredArguments { get; } = 3;

    public override bool IsConditionSatisfied(string FirstVariable, string SecondVariable)
    {
        // Your condition satisfying code here (for one or two variables)
        // return true;
    }
    
    public bool IsConditionSatisfied(string[] Variables)
    {
        // Your condition satisfying code here (for more than two variables, counting from zero)
        // return true;
    }
}
```
{% endcode %}

</details>

<details>

<summary>Registering a condition</summary>

To register your condition, you must call `MESHConditional.RegisterCondition()` in your mod initialization code to add your condition with your needed code to the list of custom conditions. After that, the MESH script parser will be able to parse your custom condition.

</details>

<details>

<summary>Unregistering a condition</summary>

To unregister your condition, you must call `MESHConditional.UnregisterCondition()` in your mod cleanup code to remove your condition from the list of custom conditions. After that, you won't be able to use scripts that use your custom condition.

</details>

***

## <mark style="color:$primary;">Conditional blocks and Loops</mark>

The conditional blocks and loops are one of the most essential scripting features that control the script flow based on the conditions and conditional loops. These are currently supported:

* `if <condition>`
* `while <condition>`
* `until <condition>`

After the script parser detects one of these, it checks for the new block stack in the next line, like this:

```
if $test2 eq y
|set $test3 n
```

{% hint style="danger" %}
The new block stack must be defined with one extra `|` character directly after lines that start with one of the above conditional block statements. Otherwise, parsing will fail.
{% endhint %}

If defined correctly, the script parser walks through the commands defined in the new stack. However, if the condition is not satisfied, the whole block stack for the first conditional block that doesn't satisfy the condition will be skipped and the parser will continue executing commands that are defined in the current stack.

For example, consider this:

{% code lineNumbers="true" %}
```
choice -m $test2 y/n "Found any bugs?"
if $test2 eq y
|set $test3 n
|until $test3 eq y
||choice -m $test3 y/n "Exit?"
||echo Current: $test3
|echo out of until
echo out of if
```
{% endcode %}

This script first checks to see if the user has answered `y` in the first line. The following will happen:

* If the user answered `y`, the script parser enters the new stack defined by the `if` condition in line 2.
* If the user answered `n`, the script parser skips the new stack defined by the `if` condition and continues parsing the commands from line 8.

`while` and `until` blocks require the new stack to be defined. In addition to this, the script parser checks to see if the condition is no longer satisfied after the stack that these blocks defined.

* If the condition is satisfied, the commands after the `while` or `until` blocks get executed.
* If the condition is not satisfied, the commands after the `while` or `until` blocks get skipped and the script parser continues parsing the commands.

***

## <mark style="color:$primary;">Error codes</mark>

The error code variable, `MESHErrorCode`, holds information about the last process error code, whether it's a success (a zero value) or a failure (non-zero value). Currently, these values are supported:

<details>

<summary>Error codes that come from the parser</summary>

<table><thead><tr><th width="90.33331298828125">Code</th><th>Description</th></tr></thead><tbody><tr><td><code>0</code></td><td>Indicates success</td></tr><tr><td><code>-1</code></td><td>Indicates that the command is not found</td></tr><tr><td><code>-2</code></td><td>Indicates that the command is not found and that the file is not found under any path lookup directories</td></tr></tbody></table>

</details>

<details>

<summary>Error codes that come from the commands</summary>

<table><thead><tr><th width="230.33319091796875">Code</th><th>Description</th></tr></thead><tbody><tr><td><code>0</code></td><td>Indicates success</td></tr><tr><td><code>-5</code></td><td>Indicates that the command is interrupted unexpectedly</td></tr><tr><td><code>-6</code></td><td>Indicates that the user didn't provide required arguments</td></tr><tr><td><code>Exception.GetHashCode()</code></td><td>Some commands throw this value from any exception. Indicates that the command failed to perform its operation.</td></tr></tbody></table>

</details>

Consult your application's manual for further information about other error codes emitted from the commands.
