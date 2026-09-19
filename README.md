# Kelo Tester UI

A lightweight Roblox Luau UI library (`Lib.lua`) for building clean tester,
settings, debug, and playtest dashboards. It includes tabs, panels, common
controls, themes, customization, mouse/touch dragging, bottom-right resizing,
and automatic cleanup of an older copy of the same UI.

## Files

| File | Where it goes | Type |
|---|---|---|
| `Lib.lua` | `ReplicatedStorage` | ModuleScript |
| `README.md` | Your project/repository | Documentation |

A simple Roblox project can look like this:

```text
ReplicatedStorage/
  Lib                 (ModuleScript, from Lib.lua)

StarterPlayer/
  StarterPlayerScripts/
    Client             (LocalScript that requires Lib)
```

The library itself is client-side because it uses `Players.LocalPlayer` and
`PlayerGui`.

## Setup

1. **Create the `Lib` ModuleScript** inside `ReplicatedStorage`.
2. Paste the contents of `Lib.lua` into that ModuleScript.
3. Create a `LocalScript` in `StarterPlayerScripts`.
4. Require the library:

   ```lua
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local Lib = require(ReplicatedStorage:WaitForChild("Lib"))
   ```

5. Create a window, then add tabs, panels, and controls.

### Smallest working example

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

local Window = Lib:CreateWindow({
    Title = "Kelo Tester",
})

local Tab = Window:AddTab({
    Name = "Main",
    Subtitle = "Tester settings",
})

local Panel = Tab:AddPanel("Options")

Panel:AddToggle({
    Name = "Enabled",
    Default = false,
    Callback = function(value)
        print("Enabled:", value)
    end,
})
```

## How it works

- `Lib:CreateWindow()` creates the main `ScreenGui` and window.
- `Window:AddTab()` adds a page to the sidebar.
- `Tab:AddPanel()` creates a container for related controls.
- Panels can have an optional checkbox-style enable/disable header.
- Controls are added directly to a panel with methods such as
  `AddToggle`, `AddSlider`, `AddDropdown`, and `AddButton`.
- The first tab you add becomes the active tab automatically.
- The window can be dragged with mouse or touch input.
- A bottom-right resize handle lets the user resize the window.
- Creating another window with the same `GuiName` removes the old copy by
default, preventing stacked duplicate UIs.

## Creating the window

```lua
local Window = Lib:CreateWindow({
    Title = "Kelo Tester",
    ProfileName = "Player",
    ProfileTag = "v1.0",
    Size = UDim2.fromOffset(700, 500),
    SidebarWidth = 200,
    AccentColor = Color3.fromRGB(70, 145, 235),
    Theme = "Ocean",
    GuiName = "Lib",
})
```

### Window options

| Option | Type | Description |
|---|---|---|
| `Title` | string | Window title. Defaults to `Kelo`. |
| `ProfileName` | string | Name shown in the profile area. Defaults to the local player's username. |
| `ProfileTag` | string | Smaller profile text/tag. |
| `Size` | UDim2 | Starting window size. Defaults to `860x620`. |
| `SidebarWidth` | number | Width of the left sidebar. Defaults to `220`. |
| `AccentColor` | Color3 | Starting accent color. |
| `Theme` | string | Theme name from `Lib.Themes`. |
| `GuiName` | string | Name of the generated `ScreenGui`. Defaults to `Lib`. |

## Duplicate UI protection

The library checks `PlayerGui` before creating a new window.

By default:

```lua
Lib.Customization.Behavior.DestroyOldUI = true
```

So if this runs twice:

```lua
Lib:CreateWindow({ Title = "Kelo Tester" })
Lib:CreateWindow({ Title = "Kelo Tester" })
```

the second window removes the first one when both use the same `GuiName`.

If you want multiple independent UIs, give them different names:

```lua
local MainUI = Lib:CreateWindow({
    Title = "Main",
    GuiName = "MainUI",
})

local DebugUI = Lib:CreateWindow({
    Title = "Debug",
    GuiName = "DebugUI",
})
```

Or disable automatic cleanup:

```lua
Lib.Customization.Behavior.DestroyOldUI = false
```

## Dragging and resizing

The window supports both mouse and touch dragging.

### Dragging

Dragging can be enabled or disabled through:

```lua
Lib.Customization.Drag.Enabled = true
Lib.Customization.Drag.Mouse = true
Lib.Customization.Drag.Touch = true
```

The library provides outer drag areas around the window so the UI can be
moved without depending on only one small header area.

### Bottom-right resize

The bottom-right resize box lets the user change the window size manually.

The current minimum size is approximately:

```text
560 x 420
```

The starting size is controlled by:

```lua
Lib.Customization.Size.Window = UDim2.fromOffset(860, 620)
```

## Destroy / unload

You can manually unload a window with:

```lua
Window:Destroy()
```

This destroys the generated `ScreenGui`.

If you only want to hide it without destroying it:

```lua
Window.MainFrame.Visible = false
```

Show it again with:

```lua
Window.MainFrame.Visible = true
```

## Tabs

Add a tab with:

```lua
local Tab = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
})
```

### Tab options

| Option | Type | Description |
|---|---|---|
| `Name` | string | Tab name. |
| `Subtitle` | string | Smaller text shown under the tab name. |
| `Icon` | string | Roblox image asset ID/string used as the tab icon. |

Example:

```lua
local CombatTab = Window:AddTab({
    Name = "Combat",
    Subtitle = "Combat settings",
    Icon = "rbxassetid://123456789",
})
```

The first tab added is selected automatically.

The window also exposes:

```lua
Window.Tabs
Window.ActiveTab
```

`Window.Tabs` is the array of tabs in the order they were created.

## Panels

Panels group related controls together.

### Default panel

```lua
local Panel = Tab:AddPanel("Options")
```

This creates a panel with a title and checkbox-style header.

### Title without a toggle

```lua
local Panel = Tab:AddPanel({
    Title = "Information",
    Toggleable = false,
})
```

### Box with no header

```lua
local Panel = Tab:AddPanel({
    Toggleable = false,
})
```

### Panel callback/default state

A toggleable panel can also use `Default` and `Callback`:

```lua
local Panel = Tab:AddPanel({
    Title = "Advanced",
    Default = true,
    Callback = function(enabled)
        print("Panel enabled:", enabled)
    end,
})
```

You can check its current state with:

```lua
print(Panel:IsEnabled())
```

## Controls

All controls below are added to a panel.

### Label

```lua
Panel:AddLabel({
    Text = "Section notes",
    Muted = true,
    Bold = false,
})
```

### Divider

```lua
Panel:AddDivider()
```

### Toggle

```lua
Panel:AddToggle({
    Name = "Enable thing",
    Default = false,
    Callback = function(on)
        print("Enabled:", on)
    end,
})
```

### Slider

```lua
Panel:AddSlider({
    Name = "Amount",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Amount:", value)
    end,
})
```

### Dropdown

```lua
Panel:AddDropdown({
    Name = "Mode",
    Options = { "A", "B", "C" },
    Default = "A",
    Callback = function(value)
        print("Mode:", value)
    end,
})
```

### Button

```lua
Panel:AddButton({
    Name = "Run test",
    Accent = true,
    Callback = function()
        print("Test started")
    end,
})
```

`Accent = true` uses the current accent color for the button.

### Textbox

```lua
Panel:AddTextbox({
    Name = "Tester note",
    Placeholder = "Type something...",
    Callback = function(text, enterPressed)
        print("Text:", text)
        print("Enter pressed:", enterPressed)
    end,
})
```

### Keybind

```lua
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
```

Click the keybind box to listen for a new keyboard key. `Escape` cancels the
new binding and keeps the previous key.

### Graph

```lua
local Graph = Panel:AddGraph({
    Name = "FPS",
    Points = 30,
    Min = 0,
    Suffix = " fps",
})

Graph:Push(60)
```

Call `Graph:Push(value)` repeatedly to feed new values into the graph.

## Themes

The library includes named themes that can be selected when creating the
window:

```lua
local Window = Lib:CreateWindow({
    Title = "Kelo Tester",
    Theme = "Ocean",
})
```

Available themes:

```text
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
```

You can also change the theme after the window has been created:

```lua
Window:SetTheme("Crimson")
```

The available theme definitions are exposed through:

```lua
Lib.Themes
```

## Accent colors

Built-in accent presets are exposed through `Lib.Presets`:

```lua
Lib.Presets.Green
Lib.Presets.Blue
Lib.Presets.Purple
Lib.Presets.Red
```

The starting accent can be set when creating a window:

```lua
local Window = Lib:CreateWindow({
    AccentColor = Lib.Presets.Blue,
})
```

Or with a raw `Color3`:

```lua
local Window = Lib:CreateWindow({
    AccentColor = Color3.fromRGB(70, 145, 235),
})
```

## Full customization

The library exposes its customization tables publicly:

```lua
Lib.Colors
Lib.Customization
Lib.Transparency
Lib.Style
Lib.Themes
Lib.Presets
```

The main customization table is split into these sections:

```text
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
```

### Colors

Colors are exposed through `Lib.Colors`, and the customization table also
contains color settings intended for the main parts of the UI.

Example:

```lua
Lib.Customization.Colors.White = Color3.fromRGB(255, 255, 255)
Lib.Customization.Colors.Black = Color3.fromRGB(0, 0, 0)
```

The library also exposes the base color table directly:

```lua
Lib.Colors
```

### Transparency

```lua
Lib.Customization.Transparency.Main = 0
Lib.Customization.Transparency.Panel = 0
Lib.Customization.Transparency.Button = 0
Lib.Customization.Transparency.Tab = 0
```

Roblox transparency values use `0` for fully visible and `1` for fully
transparent.

### Sizes

```lua
Lib.Customization.Size.Window = UDim2.fromOffset(860, 620)
Lib.Customization.Size.SidebarWidth = 220
Lib.Customization.Size.HeaderHeight = 50
Lib.Customization.Size.TabHeight = 44
Lib.Customization.Size.ButtonHeight = 32
Lib.Customization.Size.InputHeight = 32
Lib.Customization.Size.Padding = 14
Lib.Customization.Size.Spacing = 12
```

`Lib.Style` is an alias for `Lib.Customization.Size`.

### Corners

```lua
Lib.Customization.Corners.Main = 14
Lib.Customization.Corners.Panel = 10
Lib.Customization.Corners.Button = 6
Lib.Customization.Corners.Input = 6
Lib.Customization.Corners.Tab = 8
Lib.Customization.Corners.Small = 4
```

### Text sizes and fonts

```lua
Lib.Customization.TextSize.Title = 18
Lib.Customization.TextSize.Header = 14
Lib.Customization.TextSize.Normal = 13
Lib.Customization.TextSize.Small = 11

Lib.Customization.Fonts.Main = Enum.Font.GothamMedium
Lib.Customization.Fonts.Bold = Enum.Font.GothamBold
Lib.Customization.Fonts.Code = Enum.Font.Code
```

### Stroke

```lua
Lib.Customization.Stroke.Enabled = true
Lib.Customization.Stroke.Thickness = 1
Lib.Customization.Stroke.Transparency = 0
```

### Animation

```lua
Lib.Customization.Animation.Enabled = true
Lib.Customization.Animation.Speed = 0.15
Lib.Customization.Animation.HoverSpeed = 0.12
```

### Behavior

```lua
Lib.Customization.Behavior.DestroyOldUI = true
Lib.Customization.Behavior.CloseButton = true
Lib.Customization.Behavior.AutoCenter = true
```

## Recommended project structure

For a normal Roblox project, keep the library separate from the script that
uses it:

```text
ReplicatedStorage/
  Lib                         (ModuleScript)

StarterPlayer/
  StarterPlayerScripts/
    TesterClient              (LocalScript)
```

Then your client script can stay focused on the actual tester/debug features,
while `Lib.lua` remains reusable across different projects and dashboards.

## Complete example

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lib = require(ReplicatedStorage:WaitForChild("Lib"))

local Window = Lib:CreateWindow({
    Title = "Kelo Tester UI",
    ProfileName = "Tester",
    ProfileTag = "Playtest",
    Size = UDim2.fromOffset(860, 620),
    SidebarWidth = 220,
    AccentColor = Lib.Presets.Blue,
    Theme = "Ocean",
    GuiName = "KeloTesterUI",
})

local MainTab = Window:AddTab({
    Name = "Main",
    Subtitle = "Tester settings",
})

local Settings = MainTab:AddPanel({
    Title = "Settings",
    Default = true,
})

Settings:AddLabel({
    Text = "Use these controls to configure the current test.",
    Muted = true,
})

Settings:AddDivider()

Settings:AddToggle({
    Name = "Enable test mode",
    Default = false,
    Callback = function(enabled)
        print("Test mode:", enabled)
    end,
})

Settings:AddSlider({
    Name = "Test amount",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Test amount:", value)
    end,
})

Settings:AddDropdown({
    Name = "Mode",
    Options = { "Normal", "Debug", "Stress" },
    Default = "Normal",
    Callback = function(mode)
        print("Mode:", mode)
    end,
})

Settings:AddButton({
    Name = "Run test",
    Accent = true,
    Callback = function()
        print("Running test...")
    end,
})

local DebugTab = Window:AddTab({
    Name = "Debug",
    Subtitle = "Live information",
})

local GraphPanel = DebugTab:AddPanel({
    Title = "Performance",
    Toggleable = false,
})

local FPSGraph = GraphPanel:AddGraph({
    Name = "FPS",
    Points = 30,
    Min = 0,
    Suffix = " fps",
})

FPSGraph:Push(60)

-- Change the theme later if needed:
-- Window:SetTheme("Crimson")

-- Unload the UI manually if needed:
-- Window:Destroy()
```

## API reference

### Library

```lua
Lib:CreateWindow(config)
Lib:SetAccentColor(colorOrPresetName)
Lib.Colors
Lib.Presets
Lib.Themes
Lib.Customization
Lib.Transparency
Lib.Style
```

### Window

```lua
Window:AddTab(config)
Window:SetTheme(themeName)
Window:Destroy()
Window.MainFrame
Window.Tabs
Window.ActiveTab
```

### Tab

```lua
Tab:AddPanel(titleOrConfig)
```

### Panel

```lua
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
```

## Notes

- `Lib.lua` is a ModuleScript and should be required from a client-side
  LocalScript.
- The default `GuiName` is `Lib`.
- If `DestroyOldUI` is enabled, a new window with the same `GuiName` removes
  the previous one first.
- The bottom-right resize handle currently enforces a minimum size of about
  `560x420`.
- Mouse and touch dragging can be controlled independently.
- `Window:Destroy()` removes the generated `ScreenGui` completely.
- `Window.MainFrame.Visible = false` only hides the UI; it does not unload it.
- The first created tab is selected automatically.
- Use `Lib.Themes` and `Lib.Presets` when you want to inspect the available
  built-in theme/accent definitions from your own script.
