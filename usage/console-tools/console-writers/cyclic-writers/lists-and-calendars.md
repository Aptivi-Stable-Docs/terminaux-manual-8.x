---
description: Listing items in your console
icon: list-ul
---

# Lists and Calendars

List renderers allow you to list elements to the console, while calendar renderers print a full calendar to the console.

***

## <mark style="color:$primary;">List of renderers</mark>

Here are the renderers that you can use:

{% stepper %}
{% step %}
### <mark style="color:$primary;">Tables and Calendars</mark>

This allows you to render a table that consists of rows and columns to the terminal. You can use the cell options variable to configure various cells, such as colors. Calendars internally use the table renderer to render the core elements of a calendar, and they support non-Gregorian calendars.

<details>

<summary>Normal tables</summary>

```csharp
var Rows = new string[,]
{
    { "Ubuntu Version", "Release Date", "Support End", "ESM Support End" },
    { "12.04 (Precise Pangolin)", new DateTime(2012, 4, 26).ToString(), new DateTime(2017, 4, 28).ToString(), new DateTime(2019, 4, 28).ToString() },
    { "14.04 (Trusty Tahr)", new DateTime(2014, 4, 17).ToString(), new DateTime(2019, 4, 25).ToString(), new DateTime(2024, 4, 25).ToString() },
    { "16.04 (Xenial Xerus)", new DateTime(2016, 4, 21).ToString(), new DateTime(2021, 4, 30).ToString(), new DateTime(2026, 4, 30).ToString() },
    { "18.04 (Bionic Beaver)", new DateTime(2018, 4, 26).ToString(), new DateTime(2023, 4, 30).ToString(), new DateTime(2028, 4, 30).ToString() },
    { "20.04 (Focal Fossa)", new DateTime(2020, 4, 23).ToString(), new DateTime(2025, 4, 25).ToString(), new DateTime(2030, 4, 25).ToString() },
    { "22.04 (Jammy Jellyfish)", new DateTime(2022, 4, 26).ToString(), new DateTime(2027, 4, 25).ToString(), new DateTime(2032, 4, 25).ToString() }
};
var table = new Table()
{
    Rows = Rows,
    Left = 4,
    Top = 2,
    Width = ConsoleWrapper.WindowWidth - 7,
    Height = ConsoleWrapper.WindowHeight - 5,
    Header = true,
    Settings =
    [
        new CellOptions(2, 2)
        {
            CellColor = ConsoleColors.Red,
            CellBackgroundColor = ConsoleColors.DarkRed,
            ColoredCell = true
        }
    ]
};
TextWriterRaw.WriteRaw(table.Render());
```

<figure><img src="../../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Calendar (Gregorian)</summary>

```csharp
var calendar = new Calendars()
{
    Year = DateTime.Now.Year,
    Month = DateTime.Now.Month,
    Left = 4,
    Top = 2,
    Width = ConsoleWrapper.WindowWidth - 8,
    Height = ConsoleWrapper.WindowHeight - 4,
};
TextWriterRaw.WriteRaw(calendar.Render());
```

<figure><img src="../../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Calendar (Hijri)</summary>

```csharp
var culture = new CultureInfo("ar");
culture.DateTimeFormat.Calendar = new HijriCalendar();
var calendar = new Calendars()
{
    Year = DateTime.Now.Year,
    Month = DateTime.Now.Month,
    Left = 4,
    Top = 2,
    Width = ConsoleWrapper.WindowWidth - 8,
    Height = ConsoleWrapper.WindowHeight - 4,
    Culture = culture,
};
TextWriterRaw.WriteRaw(calendar.Render());
```

<figure><img src="../../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

</details>
{% endstep %}

{% step %}
### <mark style="color:$primary;">`ListEntry`</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">and</mark> <mark style="color:$primary;"></mark><mark style="color:$primary;">`Listing`</mark>

These renderables help you create a rendered list of items easily. `Listing` accepts any enumerable with optional functions that convert individual items to string representations, while `ListEntry` is used to render a single entry in the key and the value form.

<details>

<summary>Lists</summary>

```csharp
var misc = new Listing()
{
    Objects = new string[]
    {
        "One",
        "Two",
        "Three",
        "Four",
    }
};
TextWriterRaw.WriteRaw(misc.Render());
```

<figure><img src="../../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Dictionaries</summary>

```csharp
var misc = new Listing()
{
    Objects = new Dictionary<string, int>
    {
        { "Nitrocid KS", 264 },
        { "VisualCard", 221 },
        { "Terminaux", 214 },
    }
};
TextWriterRaw.WriteRaw(misc.Render());
```

<figure><img src="../../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Leveled lists</summary>

```csharp
var misc = new ListEntry()
{
    Entry = "Commit count for September 2024",
    Value = "699",
};
var misc2 = new ListEntry()
{
    Entry = "Nitrocid KS",
    Value = "264",
    Indentation = 1,
};
var misc3 = new ListEntry()
{
    Entry = "VisualCard",
    Value = "221",
    Indentation = 1,
};
var misc4 = new ListEntry()
{
    Entry = "Terminaux",
    Value = "214",
    Indentation = 1,
};
var misc5 = new ListEntry()
{
    Entry = "Commit count for October 2024",
    Value = "536",
};
var misc6 = new ListEntry()
{
    Entry = "Terminaux",
    Value = "216",
    Indentation = 1,
};
var misc7 = new ListEntry()
{
    Entry = "VisualCard",
    Value = "208",
    Indentation = 1,
};
var misc8 = new ListEntry()
{
    Entry = "Textify",
    Value = "112",
    Indentation = 1,
};
TextWriterRaw.WritePlain(misc.Render());
TextWriterRaw.WritePlain(misc2.Render());
TextWriterRaw.WritePlain(misc3.Render());
TextWriterRaw.WritePlain(misc4.Render());
TextWriterRaw.WritePlain(misc5.Render());
TextWriterRaw.WritePlain(misc6.Render());
TextWriterRaw.WritePlain(misc7.Render());
TextWriterRaw.WritePlain(misc8.Render());
```

<figure><img src="../../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

</details>
{% endstep %}

{% step %}
### <mark style="color:$primary;">Selection</mark>

You can render a list of selection elements using this renderable. This uses a part of the selection style renderer code.

<details>

<summary>Single</summary>

```csharp
var finalSelections = new InputChoiceInfo[]
{
	new("Choice one", "The first choice"),
	new("Choice two", "The second choice"),
	new("Choice three", "The third choice"),
	new("Choice four", "The fourth choice"),
	new("Choice five", "The fifth choice"),
	new("Choice six", "The sixth choice"),
	new("Choice seven", "The seventh choice"),
	new("Choice eight", "The eighth choice"),
};
var selections = new Selection(finalSelections)
{
	Left = 2,
	Top = 1,
	CurrentSelection = 2,
	Height = 7,
	Width = 30,
};
TextWriterRaw.WriteRaw(selections.Render());
```

<figure><img src="../../../../.gitbook/assets/image (144).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Multiple</summary>

```csharp
var finalSelections = new InputChoiceInfo[]
{
	new("Choice one", "The first choice"),
	new("Choice two", "The second choice"),
	new("Choice three", "The third choice"),
	new("Choice four", "The fourth choice"),
	new("Choice five", "The fifth choice"),
	new("Choice six", "The sixth choice"),
	new("Choice seven", "The seventh choice"),
	new("Choice eight", "The eighth choice"),
};
var selections = new Selection(finalSelections)
{
	Left = 2,
	Top = 1,
	CurrentSelection = 2,
	CurrentSelections = [0, 2, 4, 6],
	Height = 7,
	Width = 30,
};
TextWriterRaw.WriteRaw(selections.Render());
```

<figure><img src="../../../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Radio</summary>

```csharp
var finalSelections = new InputChoiceInfo[]
{
	new("Choice one", "The first choice"),
	new("Choice two", "The second choice"),
	new("Choice three", "The third choice"),
	new("Choice four", "The fourth choice"),
	new("Choice five", "The fifth choice"),
	new("Choice six", "The sixth choice"),
	new("Choice seven", "The seventh choice"),
	new("Choice eight", "The eighth choice"),
};
var selections = new Selection(finalSelections)
{
	Left = 2,
	Top = 1,
	CurrentSelection = 2,
	SelectedChoice = 3,
	Height = 7,
	Width = 30,
	ShowRadioButtons = true,
};
TextWriterRaw.WriteRaw(selections.Render());
```

<figure><img src="../../../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

</details>
{% endstep %}

{% step %}
### <mark style="color:$primary;">Simple Selection</mark>

You can render a list of selection elements using this renderable. This renders choices one by one, while still maintaining flexibility of the regular selections, making it suitable for CLIs.

<details>

<summary>No Alt Choices</summary>

```csharp
var finalSelections = new InputChoiceInfo[]
{
	new("Choice one", "The first choice"),
	new("Choice two", "The second choice"),
	new("Choice three", "The third choice"),
	new("Choice four", "The fourth choice"),
	new("Choice five", "The fifth choice"),
	new("Choice six", "The sixth choice"),
	new("Choice seven", "The seventh choice"),
	new("Choice eight", "The eighth choice"),
};
var selections = new PassiveSelection(finalSelections);
TextWriterRaw.WriteRaw(selections.Render());
```

<figure><img src="../../../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>With Alt Choices</summary>

```csharp
var finalSelections = new InputChoiceInfo[]
{
	new("Choice one", "The first choice"),
	new("Choice two", "The second choice"),
	new("Choice three", "The third choice"),
	new("Choice four", "The fourth choice"),
	new("Choice five", "The fifth choice"),
	new("Choice six", "The sixth choice"),
	new("Choice seven", "The seventh choice"),
	new("Choice eight", "The eighth choice"),
};
var selections = new PassiveSelection(finalSelections)
{
	AltChoicePos = 5
};
TextWriterRaw.WriteRaw(selections.Render());
```

<figure><img src="../../../../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>

</details>
{% endstep %}
{% endstepper %}
