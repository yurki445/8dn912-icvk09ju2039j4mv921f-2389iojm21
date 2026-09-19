Lib - Roblox UI Library

A lightweight Roblox Luau UI library with tabs, panels, controls, themes, customization, dragging, resizing, and duplicate-UI cleanup.

Where to start

If you are new to the library, start with these three steps:

Put Lib.lua in a location your LocalScript can require.

Require the library with local Lib = require(path.to.Lib).

Create a window with Lib:CreateWindow(...), then add a tab, panel, and controls.

Smallest working example

local Lib = require(path.to.Lib)

local Window = Lib:CreateWindow({
    Title = "My UI",
})

local Tab = Window:AddTab({
    Name = "Main",
    Subtitle = "My controls",
})

local Panel = Tab:AddPanel("Options")

Panel:AddToggle({
    Name = "Enabled",
    Default = false,
    Callback = function(value)
        print("Enabled:", value)
    end,
})

That is the recommended place to start. Add more controls to the same Panel as needed.

Installation

1. Add the library

Create a ModuleScript named Lib and paste the contents of Lib.lua into it.

A common layout is:

ReplicatedStorage
└── Lib (ModuleScript)

StarterPlayer
└── StarterPlayerScripts
    └── Main (LocalScript)

2. Require it from a LocalScript

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

The UI is intended to be created from the client because it uses Players.LocalPlayer and PlayerGui.

CreateWindow

Create the main UI with:

local Window = Lib:CreateWindow({
    Title = "My UI",
    ProfileName = "Player",
    ProfileTag = "v1.0",
    Size = UDim2.fromOffset(700, 500),
    SidebarWidth = 200,
    AccentColor = Color3.fromRGB(70, 145, 235),
})

Window options

Option

Type

Description

Title

string

Window title. Default is Kelo.

ProfileName

string

Name displayed in the profile area. Defaults to the local player's username.

ProfileTag

string

Small text displayed below the profile name.

Size

UDim2

Starting window size.

SidebarWidth

number

Width of the left sidebar.

AccentColor

Color3

Starting accent color.

Theme

string

Name of a theme from Lib.Themes.

GuiName

string

Name used for the ScreenGui. Defaults to Lib.

Example with a theme:

local Window = Lib:CreateWindow({
    Title = "My UI",
    Theme = "Ocean",
    ProfileName = "MyName",
    ProfileTag = "v1.0",
})

Duplicate UI protection

The library automatically looks for an existing ScreenGui with the same GUI name before creating a new one.

With the default behavior, creating the UI again removes the previous UI first, so you do not end up with multiple copies stacked on top of each other.

The behavior is controlled by:

Lib.Customization.Behavior.DestroyOldUI = true

If you intentionally want to disable that behavior:

Lib.Customization.Behavior.DestroyOldUI = false

Dragging and resizing

The window supports mouse and touch dragging.

Drag areas

You can start moving the window from:

Top/header area

Left outer side

Right outer side

Bottom outer side

Outer corners/drag areas provided by the library

Bottom-right resize box

A resize handle is provided at the bottom-right corner of the window.

The current minimum resize size is approximately:

560 x 420

Drag customization

Lib.Customization.Drag.Enabled = true
Lib.Customization.Drag.Mouse = true
Lib.Customization.Drag.Touch = true

Destroy / unload the UI

You can manually remove the current window:

Window:Destroy()

You can also hide the root frame without destroying it:

Window.MainFrame.Visible = false

Show it again with:

Window.MainFrame.Visible = true

Tabs

Add a tab with:

local Tab = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
})

Tab options

Option

Type

Description

Name

string

Tab name.

Subtitle

string

Smaller text under the tab name.

Icon

string

Image asset ID/string used as the tab icon.

Example:

local CombatTab = Window:AddTab({
    Name = "Combat",
    Subtitle = "Combat settings",
    Icon = "rbxassetid://123456789",
})

The first tab added becomes the active tab automatically.

The window also exposes:

Window.Tabs
Window.ActiveTab

Window.Tabs is the array of tabs in the order they were created.

Panels

Panels keep related controls together.

Simple panel

local Panel = Tab:AddPanel("Options")

By default, a panel has a checkbox-style enable header.

Panel with configuration

local Panel = Tab:AddPanel({
    Title = "Info",
    Toggleable = false,
})

Headerless panel

local Panel = Tab:AddPanel({
    Toggleable = false,
})

Panel options

Option

Type

Description

Title

string

Panel header text.

Toggleable

boolean

Whether the checkbox header is shown.

Default

boolean

Initial enabled state when the panel is toggleable.

Callback

function

Called when the panel checkbox changes.

Panel methods include:

Panel:IsEnabled()
Panel:AddLabel(...)
Panel:AddDivider()
Panel:AddToggle(...)
Panel:AddSlider(...)
Panel:AddDropdown(...)
Panel:AddButton(...)
Panel:AddTextbox(...)
Panel:AddKeybind(...)
Panel:AddGraph(...)

Label

Use a label for information or instructions.

Panel:AddLabel({
    Text = "This is a normal label.",
})

Label options

Panel:AddLabel({
    Text = "Important information",
    Muted = false,
    Bold = true,
    Size = 14,
    Color = Color3.fromRGB(255, 255, 255),
})

Option

Description

Text

Text to display.

Muted

Uses the secondary text color.

Bold

Uses the bold font.

Size

Text size.

Color

Overrides the text color.

Divider

Add a separator line:

Panel:AddDivider()

Toggle

Create an on/off switch:

Panel:AddToggle({
    Name = "God Mode",
    Default = false,
    Callback = function(enabled)
        print("God Mode:", enabled)
    end,
})

Toggle options

Option

Type

Description

Name

string

Toggle label.

Default

boolean

Initial state.

Callback

function

Called with the new boolean state.

Slider

Create a numeric slider:

Panel:AddSlider({
    Name = "WalkSpeed",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("WalkSpeed:", value)
    end,
})

Slider options

Option

Type

Description

Name

string

Slider label.

Min

number

Minimum value.

Max

number

Maximum value.

Default

number

Starting value.

Callback

function

Receives the slider value.

The current implementation displays the slider value as a whole-number percentage-style value.

Dropdown

Create a list of options:

Panel:AddDropdown({
    Name = "Mode",
    Options = {
        "Normal",
        "Fast",
        "Safe",
    },
    Default = "Normal",
    Callback = function(value)
        print("Selected:", value)
    end,
})

Dropdown options

Option

Type

Description

Name

string

Dropdown label.

Options

table

List of selectable values.

Default

string

Initial selected value.

Callback

function

Receives the selected option.

Button

Create a clickable button:

Panel:AddButton({
    Name = "Execute",
    Callback = function()
        print("Button clicked")
    end,
})

For an accent-colored button:

Panel:AddButton({
    Name = "Execute",
    Accent = true,
    Callback = function()
        print("Accent button clicked")
    end,
})

Button options

Option

Type

Description

Name

string

Button text.

Accent

boolean

Uses the current accent color.

Callback

function

Called when clicked.

Textbox

Create a text input:

Panel:AddTextbox({
    Name = "Username",
    Placeholder = "Enter username...",
    Default = "",
    Callback = function(text, enterPressed)
        print("Text:", text)
        print("Enter pressed:", enterPressed)
    end,
})

Textbox options

Option

Type

Description

Name

string

Input label.

Placeholder

string

Placeholder text.

Default

string

Starting text.

Callback

function

Receives the text and whether Enter caused focus loss.

Callback format:

function(text, enterPressed)
end

Keybind

Create a configurable keyboard bind:

Panel:AddKeybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Callback = function()
        Window.MainFrame.Visible = not Window.MainFrame.Visible
    end,
    OnBind = function(key)
        print("New key:", key.Name)
    end,
})

Click the keybind box to listen for a new key.

Press Escape while rebinding to cancel and keep the previous key.

Keybind options

Option

Type

Description

Name

string

Keybind label.

Default

KeyCode

Starting key.

Callback

function

Runs when the assigned key is pressed.

OnBind

function

Runs when a new key is assigned.

Graph

Create a small live graph:

local Graph = Panel:AddGraph({
    Name = "FPS",
    Points = 30,
    Min = 0,
    Max = 120,
    Suffix = " fps",
})

Graph:Push(60)

You can repeatedly push values into the graph:

while task.wait(0.1) do
    Graph:Push(math.random(40, 100))
end

Graph options

Option

Type

Description

Name

string

Graph title.

Points

number

Number of displayed data points. Default is 30.

Min

number

Optional fixed minimum.

Max

number

Optional fixed maximum.

Height

number

Optional graph height.

Color

Color3

Graph bar color override.

Suffix

string

Text appended to the displayed latest value.

ShowValue

boolean

Set to false to hide the latest value.

Round

number

Number of decimal places used for the displayed value.

Graph API

Graph:Push(value)

Themes

The library includes named themes:

Default
Ocean
AmberGlow
Light
Amethyst
Green
Bloom
DarkBlue
Crimson
Monochrome

Start with a theme

local Window = Lib:CreateWindow({
    Title = "My UI",
    Theme = "Ocean",
})

Change a theme after creating the window

Window:SetTheme("Crimson")

See available themes

print(Lib.Themes)

The full theme definitions are exposed through:

Lib.Themes

Each theme contains detailed colors for text, background, tabs, elements, sliders, toggles, dropdowns, inputs, and notifications.

Accent colors

The library has built-in accent presets:

Lib.Presets.Green
Lib.Presets.Blue
Lib.Presets.Purple
Lib.Presets.Red

Set an accent using a preset:

Lib:SetAccentColor("Blue")

Or pass a Color3:

Lib:SetAccentColor(Color3.fromRGB(255, 0, 0))

Accent-aware elements update when the accent changes.

Colors

The base color table is exposed as:

Lib.Colors

Colors are stored as Roblox Color3 values using Color3.fromRGB().

Example:

local Color = Color3.fromRGB(0, 255, 0)

The library's color section contains named colors for the UI. Use Lib.Colors when you need access to the library's current base colors.

Full customization

The main customization table is:

Lib.Customization

It is divided into several sections.

Colors

Lib.Customization.Colors

Controls the base UI color assignments such as background, panel, sidebar, accent, text, buttons, inputs, sliders, checkbox, graph, and avatar colors.

Transparency

Lib.Customization.Transparency

Available transparency settings include:

Main
Panel
Sidebar
Header
Footer
Button
ButtonHover
Tab
Input
Slider
SliderFill
Checkbox
Graph
Border

Example:

Lib.Customization.Transparency.Panel = 0.15

Size

Lib.Customization.Size

Available size settings include:

Window
SidebarWidth
HeaderHeight
TabHeight
ButtonHeight
InputHeight
Padding
Spacing

Corners

Lib.Customization.Corners

Available corner sizes:

Main
Panel
Button
Input
Tab
Small

TextSize

Lib.Customization.TextSize

Available text-size settings:

Title
Header
Normal
Small
Button
Label

Fonts

Lib.Customization.Fonts

Available font slots:

Main
Bold
Code

Stroke

Lib.Customization.Stroke

Settings:

Enabled
Thickness
Transparency

Animation

Lib.Customization.Animation

Settings:

Enabled
Speed
HoverSpeed

Drag

Lib.Customization.Drag

Settings:

Enabled
Mouse
Touch

Behavior

Lib.Customization.Behavior

Settings:

DestroyOldUI
CloseButton
AutoCenter

Shortcut exports

These are also available directly from the library:

Lib.Colors
Lib.Customization
Lib.Transparency
Lib.Style
Lib.Themes
Lib.Presets

Lib.Style points to the size customization table.

Useful Window properties

The window object exposes useful references:

Window.ScreenGui
Window.MainFrame
Window.TabContainer
Window.ContentArea
Window.Tabs
Window.ActiveTab

Example:

Window.MainFrame.Visible = false

Complete example

This example shows a typical starting project using most of the library's controls:

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

local Window = Lib:CreateWindow({
    Title = "My Script",
    ProfileName = "Player",
    ProfileTag = "v1.0",
    Theme = "Ocean",
    Size = UDim2.fromOffset(860, 620),
})

local Main = Window:AddTab({
    Name = "Main",
    Subtitle = "Main settings",
})

local Panel = Main:AddPanel("Options")

Panel:AddLabel({
    Text = "Configure your settings below.",
    Muted = true,
})

Panel:AddDivider()

Panel:AddToggle({
    Name = "Enabled",
    Default = false,
    Callback = function(value)
        print("Enabled:", value)
    end,
})

Panel:AddSlider({
    Name = "Power",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Power:", value)
    end,
})

Panel:AddDropdown({
    Name = "Mode",
    Options = {"Normal", "Fast", "Safe"},
    Default = "Normal",
    Callback = function(value)
        print("Mode:", value)
    end,
})

Panel:AddButton({
    Name = "Run",
    Accent = true,
    Callback = function()
        print("Run clicked")
    end,
})

Panel:AddTextbox({
    Name = "Message",
    Placeholder = "Type something...",
    Callback = function(text, enterPressed)
        print(text, enterPressed)
    end,
})

Panel:AddKeybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Callback = function()
        Window.MainFrame.Visible = not Window.MainFrame.Visible
    end,
})

local Graph = Panel:AddGraph({
    Name = "Values",
    Points = 30,
    Min = 0,
    Max = 100,
})

task.spawn(function()
    while Window.ScreenGui and Window.ScreenGui.Parent do
        Graph:Push(math.random(0, 100))
        task.wait(0.2)
    end
end)

Recommended project structure

ReplicatedStorage
├── Lib (ModuleScript)
└── OtherModules

StarterPlayer
└── StarterPlayerScripts
    └── Main (LocalScript)

Keep the UI setup in a LocalScript and keep Lib as a ModuleScript.

Updating the library

When replacing the library with a newer version:

Replace the old Lib ModuleScript with the new Lib.lua.

Keep your existing LocalScript code unless the release notes for the new version say otherwise.

Read this README for the current API and examples.

The duplicate-UI protection helps prevent old copies from stacking when your script creates a new window.

Quick reference

Lib:CreateWindow(config)

Window:AddTab(config)
Window:SetTheme(name)
Window:Destroy()
Window.MainFrame
Window.Tabs
Window.ActiveTab

Tab:AddPanel(titleOrConfig)

Panel:IsEnabled()
Panel:AddLabel(config)
Panel:AddDivider()
Panel:AddToggle(config)
Panel:AddSlider(config)
Panel:AddDropdown(config)
Panel:AddButton(config)
Panel:AddTextbox(config)
Panel:AddKeybind(config)
Panel:AddGraph(config)

Graph:Push(value)

Lib:SetAccentColor(Color3)
Lib:SetAccentColor("Green")

Lib.Presets
Lib.Themes
Lib.Colors
Lib.Customization
Lib.Transparency
Lib.Style

Notes

The library uses Roblox Luau.

UI creation depends on Players.LocalPlayer and PlayerGui.

The first added tab is selected automatically.

The window can be moved with mouse/touch drag zones.

The bottom-right resize handle changes the window size.

Creating another UI with the same GUI name removes the old UI when DestroyOldUI is enabled.

Theme definitions are stored in Lib.Themes.

Base customization is stored in Lib.Customization.
