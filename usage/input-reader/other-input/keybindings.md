---
description: Press any key!
icon: keyboard
---

# Keybindings

Individual keybindings are defined using the `Keybinding` class found in the `Terminaux.Writer.CyclicWriters.Renderer.Tools` namespace. They define what key or key combination, whether it's a mouse or a keyboard event, corresponds to what action that is described using text.

***

## <mark style="color:$primary;">Definition</mark>

You can define them using one of the following constructors:

* Console key without any modifiers
  * `string bindingName, ConsoleKey bindingKeyName`
* Console key with modifiers
  * `string bindingName, ConsoleKey bindingKeyName, ConsoleModifiers bindingKeyModifiers`
* Mouse pointer button without button press state and modifier
  * `string bindingName, PointerButton bindingPointerButton`
* Mouse pointer button with button press state, but without modifier
  * `string bindingName, PointerButton bindingPointerButton, PointerButtonPress bindingPointerButtonPress`
* Mouse pointer button with button press state and modifier
  * `string bindingName, PointerButton bindingPointerButton, PointerButtonPress bindingPointerButtonPress, PointerModifiers bindingButtonModifiers`
