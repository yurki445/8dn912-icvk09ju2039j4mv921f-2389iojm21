Kelo UI

A lightweight Roblox Luau UI library (Lib.lua) for building clean tester,
debug, settings, and playtest dashboards.

The default layout is built around:

a left sidebar with icon boxes

rounded tab buttons with subtitles

two-column rounded panels

checkbox-style panel headers

Lucide-style icons

sliders, dropdowns, toggles, buttons, textboxes, keybinds, and graphs

mouse/touch dragging

bottom-right resizing

automatic duplicate-UI cleanup

named themes and full customization

The icon names follow the Lucide icon set from
https://lucide.dev/icons/. The library uses Roblox-hosted Lucide renditions so
the icons can be displayed by normal Roblox ImageLabel objects.

Files

File

Where it goes

Type

Lib.lua

ReplicatedStorage

ModuleScript

README.md

Your project/repository

Documentation

Recommended Roblox structure:

ReplicatedStorage/
  Lib                  (ModuleScript, from Lib.lua)

StarterPlayer/
  StarterPlayerScripts/
    Client             (LocalScript that requires Lib)

Lib.lua is client-side because it uses Players.LocalPlayer and
PlayerGui.

Setup

Create a ModuleScript named Lib inside ReplicatedStorage.

Paste the contents of Lib.lua into it.

Create a LocalScript inside StarterPlayerScripts.

Require the library:

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

Create a window and add your tabs/panels.

Smallest working example

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

local Window = Lib:CreateWindow({
    Title = "Kelo",
})

local Main = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
    Icon = "home",
})

local Panel = Main:AddPanel("Option")

Panel:AddSlider({
    Name = "Amount",
    Min = 0,
    Max = 100,
    Default = 65,
})

Panel:AddDropdown({
    Name = "Mode",
    Options = { "Normal", "Debug", "Stress" },
    Default = "Normal",
})

Default layout

The library is designed around the same general format as the example UI:

┌──────────────────────────────────────────────────────────────┐
│  Kelo              │   ┌─────────────┐  ┌─────────────┐     │
│                    │   │ □ Option    │  │ □ Option    │     │
│  [icon] Main       │   │             │  │             │     │
│  [icon] Visuals    │   │  slider     │  │  dropdown   │     │
│  [icon] Tools      │   │  [========] │  │  [────────] │     │
│  [icon] Custom     │   │             │  │             │     │
│  [icon] Misc       │   └─────────────┘  └─────────────┘     │
│                    │                                          │
│  profile           │                                          │
└──────────────────────────────────────────────────────────────┘

Panels automatically wrap horizontally, so multiple panels can sit beside
each other when there is enough room.

The default panel width is 304 pixels and can be changed with:

Lib.Customization.Size.PanelWidth = 304

Creating the window

local Window = Lib:CreateWindow({
    Title = "Kelo",
    ProfileName = "Player",
    ProfileTag = "v1.0",
    Size = UDim2.fromOffset(860, 620),
    SidebarWidth = 220,
    AccentColor = Lib.Presets.Green,
    Theme = "Default",
    GuiName = "Lib",
})

Window options

Option

Type

Description

Title

string

Main title. Defaults to Kelo.

ProfileName

string

Name shown in the profile area. Defaults to the local player's username.

ProfileTag

string

Small text/tag under the profile name.

Size

UDim2

Starting window size. Defaults to 860x620.

SidebarWidth

number

Left sidebar width. Defaults to 220.

AccentColor

Color3

Starting accent color.

Theme

string

Theme name from Lib.Themes.

GuiName

string

Generated ScreenGui name. Defaults to Lib.

Duplicate UI protection

The library automatically removes an older UI with the same GuiName.

Lib.Customization.Behavior.DestroyOldUI = true

This means running:

Lib:CreateWindow({
    Title = "Kelo",
    GuiName = "Lib",
})

Lib:CreateWindow({
    Title = "Kelo",
    GuiName = "Lib",
})

removes the first ScreenGui before creating the second one.

If you want multiple independent windows, give them different names:

local MainUI = Lib:CreateWindow({
    Title = "Main",
    GuiName = "MainUI",
})

local DebugUI = Lib:CreateWindow({
    Title = "Debug",
    GuiName = "DebugUI",
})

Or disable the behavior:

Lib.Customization.Behavior.DestroyOldUI = false

Dragging

The entire window supports mouse and touch dragging.

The library has draggable areas around the outer edges, so you do not have to
use only one tiny header area.

Lib.Customization.Drag.Enabled = true
Lib.Customization.Drag.Mouse = true
Lib.Customization.Drag.Touch = true

Resizing

A resize box is placed in the bottom-right corner of the window.

Drag it to change the window size.

The current minimum size is approximately:

560 x 420

The starting size is controlled by:

Lib.Customization.Size.Window = UDim2.fromOffset(860, 620)

Unload / destroy

Destroy the whole UI:

Window:Destroy()

Only hide it:

Window.MainFrame.Visible = false

Show it again:

Window.MainFrame.Visible = true

Tabs

Add tabs with:

local Main = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
    Icon = "home",
})

Tab options

Option

Type

Description

Name

string

Tab title.

Subtitle

string

Smaller text below the title.

Icon

string/number

Lucide icon name, lucide:name, or Roblox asset ID.

The first tab is selected automatically.

Default tab icons

If Icon is omitted, common tab names automatically receive an icon:

Tab name

Default Lucide icon

Main / Home

home

Visual / Visuals

eye

Tool / Tools

wrench

Custom

code

Misc / Settings

settings

Example:

Window:AddTab({
    Name = "Visuals",
    Subtitle = "Visual customization",
})

This automatically uses the Lucide eye icon.

Icons

The library uses Lucide icon names.

Browse the available icons at:

https://lucide.dev/icons/

Examples:

Icon = "home"
Icon = "eye"
Icon = "wrench"
Icon = "code"
Icon = "settings"
Icon = "sliders"
Icon = "key"
Icon = "search"
Icon = "check"
Icon = "chevron-down"

You can also explicitly prefix an icon with lucide::

Icon = "lucide:home"

Or provide a Roblox asset ID:

Icon = "rbxassetid://123456789"

Or a numeric asset ID:

Icon = 123456789

The public icon map is available through:

Lib.Icons

Example:

print(Lib.Icons.home)
print(Lib.Icons.settings)

The built-in icon assets are Roblox-hosted Lucide renditions, allowing them to
work as normal Roblox image assets instead of trying to load an SVG directly
from the Lucide website.

Panels

Panels group related controls together.

Normal panel

local Panel = Main:AddPanel("Option")

This creates the rounded box with a checkbox-style header shown in the example
layout.

Panel without a toggle

local Panel = Main:AddPanel({
    Title = "Information",
    Toggleable = false,
})

Empty panel

local Panel = Main:AddPanel({
    Toggleable = false,
})

Panel default state and callback

local Panel = Main:AddPanel({
    Title = "Advanced",
    Default = true,
    Callback = function(enabled)
        print("Panel enabled:", enabled)
    end,
})

Check its current state:

print(Panel:IsEnabled())

Controls

All controls are added directly to a panel.

Label

Panel:AddLabel({
    Text = "Section notes",
    Muted = true,
    Bold = false,
})

Divider

Panel:AddDivider()

Toggle

Panel:AddToggle({
    Name = "Enable thing",
    Default = false,
    Callback = function(enabled)
        print("Enabled:", enabled)
    end,
})

Slider

Panel:AddSlider({
    Name = "Amount",
    Min = 0,
    Max = 100,
    Default = 65,
    Callback = function(value)
        print("Amount:", value)
    end,
})

Dropdown

The dropdown uses a Lucide chevron-down icon.

Panel:AddDropdown({
    Name = "Mode",
    Options = { "A", "B", "C" },
    Default = "A",
    Callback = function(value)
        print("Mode:", value)
    end,
})

Button

Panel:AddButton({
    Name = "Run test",
    Accent = true,
    Callback = function()
        print("Test started")
    end,
})

Buttons can also have icons:

Panel:AddButton({
    Name = "Run test",
    Icon = "play",
    Accent = true,
    Callback = function()
        print("Test started")
    end,
})

If the icon is not in the built-in map, pass a Roblox asset ID instead.

Textbox

Panel:AddTextbox({
    Name = "Tester note",
    Placeholder = "Type something...",
    Callback = function(text, enterPressed)
        print("Text:", text)
        print("Enter pressed:", enterPressed)
    end,
})

Keybind

Panel:AddKeybind({
    Name = "Open menu",
    Default = Enum.KeyCode.E,
    Callback = function()
        print("Keybind pressed")
    end,
    OnBind = function(key)
        print("New key:", key.Name)
    end,
})

Click the keybind box to listen for a new keyboard key. Escape cancels the
new binding and keeps the previous key.

Graph

local Graph = Panel:AddGraph({
    Name = "FPS",
    Points = 30,
    Min = 0,
    Suffix = " fps",
})

Graph:Push(60)

Call Graph:Push(value) repeatedly to feed values into the graph.

Themes

Create a window with a named theme:

local Window = Lib:CreateWindow({
    Title = "Kelo",
    Theme = "Ocean",
})

Available themes:

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

Change the theme later:

Window:SetTheme("Crimson")

Theme definitions are exposed through:

Lib.Themes

Accent colors

Built-in accent presets:

Lib.Presets.Green
Lib.Presets.Blue
Lib.Presets.Purple
Lib.Presets.Red

Set an accent when creating the window:

local Window = Lib:CreateWindow({
    AccentColor = Lib.Presets.Green,
})

Change it after the window exists:

Window:SetAccentColor("Blue")

Or use a raw Color3:

Window:SetAccentColor(Color3.fromRGB(70, 145, 235))

The library also supports:

Lib:SetAccentColor("Blue")

Colors

The base colors are available through:

Lib.Colors

The default palette includes named colors such as:

Lib.Colors.Green
Lib.Colors.EerieBlack
Lib.Colors.RaisinBlack
Lib.Colors.DarkGray
Lib.Colors.CharlestonGreen
Lib.Colors.DarkSlateGray
Lib.Colors.DarkGreen
Lib.Colors.Gainsboro
Lib.Colors.SpanishGray
Lib.Colors.Charcoal
Lib.Colors.DavyGray

Example:

Lib.Customization.Colors.Background =
    Color3.fromRGB(10, 12, 11)

Lib.Customization.Colors.Accent =
    Color3.fromRGB(0, 255, 0)

Full customization

The public customization tables are:

Lib.Colors
Lib.Icons
Lib.Customization
Lib.Transparency
Lib.Style
Lib.Themes
Lib.Presets

Lib.Customization contains:

Colors
Transparency
Size
Corners
TextSize
Fonts
Stroke
Animation
Drag
Behavior

Size

Lib.Customization.Size.Window = UDim2.fromOffset(860, 620)
Lib.Customization.Size.SidebarWidth = 220
Lib.Customization.Size.HeaderHeight = 50
Lib.Customization.Size.TabHeight = 44
Lib.Customization.Size.PanelWidth = 304
Lib.Customization.Size.IconBox = 30
Lib.Customization.Size.Icon = 16
Lib.Customization.Size.ButtonHeight = 32
Lib.Customization.Size.InputHeight = 32
Lib.Customization.Size.Padding = 14
Lib.Customization.Size.Spacing = 12

Lib.Style is an alias for Lib.Customization.Size.

Corners

Lib.Customization.Corners.Main = 14
Lib.Customization.Corners.Panel = 10
Lib.Customization.Corners.Button = 6
Lib.Customization.Corners.Input = 6
Lib.Customization.Corners.Tab = 8
Lib.Customization.Corners.Small = 4

Transparency

Roblox uses:

0 = fully visible
1 = fully transparent

Example:

Lib.Customization.Transparency.Main = 0
Lib.Customization.Transparency.Panel = 0
Lib.Customization.Transparency.Button = 0
Lib.Customization.Transparency.Tab = 0

Text sizes

Lib.Customization.TextSize.Title = 18
Lib.Customization.TextSize.Header = 14
Lib.Customization.TextSize.Normal = 13
Lib.Customization.TextSize.Small = 11
Lib.Customization.TextSize.Button = 13
Lib.Customization.TextSize.Label = 12

Fonts

Lib.Customization.Fonts.Main = Enum.Font.GothamMedium
Lib.Customization.Fonts.Bold = Enum.Font.GothamBold
Lib.Customization.Fonts.Code = Enum.Font.Code

Stroke

Lib.Customization.Stroke.Enabled = true
Lib.Customization.Stroke.Thickness = 1
Lib.Customization.Stroke.Transparency = 0

Animation

Lib.Customization.Animation.Enabled = true
Lib.Customization.Animation.Speed = 0.15
Lib.Customization.Animation.HoverSpeed = 0.12

Drag

Lib.Customization.Drag.Enabled = true
Lib.Customization.Drag.Mouse = true
Lib.Customization.Drag.Touch = true

Behavior

Lib.Customization.Behavior.DestroyOldUI = true
Lib.Customization.Behavior.CloseButton = true
Lib.Customization.Behavior.AutoCenter = true

Complete example

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

local Window = Lib:CreateWindow({
    Title = "Kelo",
    ProfileName = "Tester",
    ProfileTag = "Playtest",
    Size = UDim2.fromOffset(860, 620),
    SidebarWidth = 220,
    AccentColor = Lib.Presets.Green,
    Theme = "Default",
    GuiName = "KeloUI",
})

local Main = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
    Icon = "home",
})

local Visuals = Window:AddTab({
    Name = "Visuals",
    Subtitle = "Visual customization",
    Icon = "eye",
})

local Tools = Window:AddTab({
    Name = "Tools",
    Subtitle = "Useful tools",
    Icon = "wrench",
})

local Panel = Main:AddPanel("Option")

Panel:AddSlider({
    Name = "Amount",
    Min = 0,
    Max = 100,
    Default = 65,
    Callback = function(value)
        print("Amount:", value)
    end,
})

Panel:AddSlider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 12,
    Callback = function(value)
        print("Speed:", value)
    end,
})

Panel:AddDropdown({
    Name = "Mode",
    Options = { "Normal", "Debug", "Stress" },
    Default = "Normal",
    Callback = function(value)
        print("Mode:", value)
    end,
})

Panel:AddButton({
    Name = "Run test",
    Icon = "play",
    Accent = true,
    Callback = function()
        print("Running test...")
    end,
})

local TogglePanel = Visuals:AddPanel({
    Title = "Visuals",
    Default = true,
})

TogglePanel:AddToggle({
    Name = "Enable visuals",
    Default = true,
    Callback = function(enabled)
        print("Visuals:", enabled)
    end,
})

-- Change the theme later:
-- Window:SetTheme("Crimson")

-- Change the accent later:
-- Window:SetAccentColor("Blue")

-- Hide the UI:
-- Window.MainFrame.Visible = false

-- Completely unload the UI:
-- Window:Destroy()

API reference

Library

Lib:CreateWindow(config)

Lib:SetAccentColor(colorOrPresetName)

Lib.Colors
Lib.Icons
Lib.Presets
Lib.Themes
Lib.Customization
Lib.Transparency
Lib.Style

Window

Window:AddTab(config)

Window:SetTheme(themeName)

Window:SetAccentColor(colorOrPresetName)

Window:Destroy()

Window.MainFrame
Window.Tabs
Window.ActiveTab

Tab

Tab:AddPanel(titleOrConfig)

Panel

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

Notes

Lib.lua is a ModuleScript and should be required from a client-side
LocalScript.

The default generated ScreenGui name is Lib.

DestroyOldUI removes the previous UI before creating a new one with the
same GuiName.

The first tab created is selected automatically.

Tabs use Lucide-style icon boxes.

Common tab names receive default Lucide icons automatically.

The dropdown arrow and panel check mark also use Lucide icons.

Window:Destroy() removes the generated ScreenGui.

Window.MainFrame.Visible = false only hides the UI.

The bottom-right resize handle currently enforces a minimum size of about
560x420.

Mouse and touch dragging can be controlled independently.

The icon names are based on the Lucide icon set at
https://lucide.dev/icons/.
