---
description: Breaking changes for API v8.0
icon: up
---

# API v8.0

Here is a list of breaking changes that happened during the API v8.0 period when differing versions of Terminaux introduced breaking changes.

## From 7.0.x to 8.0.x

Between the 7.0.x and 8.0.x version range, we've made the following breaking changes:

### BassBoom library path settings moved

{% code title="TermReaderSettings.cs" lineNumbers="true" %}
```csharp
public string BassBoomLibraryPath
{
    get => bassBoomLibraryRoot ?? "";
    set => bassBoomLibraryRoot = value;
}
```
{% endcode %}

The BassBoom library path is supposed to be set once, because BassBoom loads the library once before being functional. To reflect BassBoom's behavior, we've decided to move the above property to the static class of `Input`.

{% hint style="info" %}
Update all references to this property to use the new location.
{% endhint %}

### General console checker removed

{% code title="ConsoleChecker.cs" lineNumbers="true" %}
```csharp
public static void CheckConsole() {}
public static void AddToCheckWhitelist(Assembly asm) {}
public static void RemoveFromCheckWhitelist(Assembly asm) {}
```
{% endcode %}

We have removed the general console checker as part of an ongoing effort to properly support file redirects and pipes for Terminaux CLI applications that use the provided console writing functions that support color. This will allow your application to use those functions without having to write a separate code path for dumb consoles.

{% hint style="danger" %}
If you are still using the console checker, you'll either have to downgrade to Terminaux 7.x, or you'll have to re-implement the checker yourself according to your requirements.
{% endhint %}

### Console filter removed

{% code title="ConsoleFilter*.cs" lineNumbers="true" %}
```csharp
public static class ConsoleFilter {}
public class ConsoleFilterInfo {}
public enum ConsoleFilterSeverity {}
public enum ConsoleFilterType {}
```
{% endcode %}

We have removed the console filter feature as part of an ongoing effort to properly support file redirects and pipes for Terminaux CLI applications that use the provided console writing functions that support color. This will allow your application to use those functions without having to write a separate code path for dumb consoles.

{% hint style="danger" %}
If you are still using the console filter, you'll either have to downgrade to Terminaux 7.x, or you'll have to re-implement the filter yourself according to your requirements.
{% endhint %}
