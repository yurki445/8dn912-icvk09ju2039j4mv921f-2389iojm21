# Kelo Tester UI

A Roblox UI library (`Lib.lua`) plus a tester dashboard (`Client.lua` /
`Server.lua`) for giving playtesters a menu to tweak settings and report
bugs — no key/login system, the UI just loads on join.

## Files

| File         | Where it goes           | Type         |
|--------------|--------------------------|--------------|
| `Lib.lua`    | `ReplicatedStorage`       | ModuleScript |
| `Server.lua` | `ServerScriptService`     | Script       |
| `Client.lua` | `StarterPlayerScripts`    | LocalScript  |

```
ReplicatedStorage/
  Lib               (ModuleScript, from Lib.lua)
ServerScriptService/
  Server            (Script, from Server.lua)
StarterPlayerScripts/
  Client            (LocalScript, from Client.lua)
```

## Setup

1. Create the three instances above and paste each file's contents in.
2. Press Play. The UI appears immediately — no key entry, no prompt.

No GitHub repo, DataStore, or HTTP requests needed for this version.

## How it works

- `Client.lua` builds the whole UI (tabs, panels, controls) on join.
- The **Report Bug** tab sends whatever a tester types to the server
  (`BugReportRemote`, set up in `Server.lua`), printed to the Output
  window tagged with the tester's Roblox username and a timestamp.

## Look & feel

- The window is slightly transparent by default (`Transparency = 0.12` in
  `Client.lua`'s `CreateWindow` call — the library itself defaults to fully
  opaque, `0`).
- The profile avatar shows the tester's real Roblox thumbnail with no
  circular mask or colored background box — just the square image. If the
  thumbnail fails to load, it falls back to a colored initial badge.
- Tabs get an automatic icon based on their name (Main → home, Visuals →
  eye, tools → wrench, custom → code, Misc → settings) or you can pass a
  custom one with `Icon = "info"` (see `Lib.Icons` for the built-in set, or
  pass any `rbxassetid://...`).
- Ten named themes are available (`Default`, `Ocean`, `AmberGlow`, `Light`,
  `Amethyst`, `Green`, `Bloom`, `DarkBlue`, `Crimson`, `Monochrome`) —
  switchable live from Visuals → Rendering → Theme, or via
  `Window:SetTheme("Ocean")`.
- The window can be dragged from any edge (not just the header) and
  resized from the bottom-right corner.
- `Window:Destroy()` fully unloads the UI — wired to Misc → Menu →
  "Close UI completely" as an example.

## Customizing the tabs

All the actual UI content (tabs, panels, controls) lives in `Client.lua`.
Add, remove, or rewrite panels there — `Lib.lua` itself is a
general-purpose library, not tied to this specific layout.

## Lib.lua API reference

```lua
local Lib = require(path.to.Lib)

local Window = Lib:CreateWindow({
    Title = "My UI",                      -- optional, default "Kelo"
    ProfileName = "Player",                -- optional, defaults to your username
    ProfileTag = "v1.0",                   -- optional, small tag next to your name
    Size = UDim2.fromOffset(700, 500),     -- optional, default 860x620
    SidebarWidth = 200,                    -- optional, default 220
    AccentColor = Color3.fromRGB(70, 145, 235), -- optional starting accent
    Theme = "Ocean",                       -- optional, one of Lib.Themes
    Transparency = 0.12,                   -- optional, default 0 (opaque); number or a per-element table
    GuiName = "Lib",                       -- optional; a second CreateWindow with the same name replaces the first
})

local Tab = Window:AddTab({
    Name = "Main",
    Subtitle = "Most Popular",
    Icon = "home", -- optional; auto-picked for common names (Main/Visuals/tools/custom/Misc)
})
```

**Panels** group related controls. Three ways to make one:

```lua
Tab:AddPanel("Option")                              -- title + checkbox header (default)
Tab:AddPanel({ Title = "Info", Toggleable = false }) -- title only, no checkbox
Tab:AddPanel({ Toggleable = false })                 -- no header at all, just a box
```

Check a panel's checkbox state: `Panel:IsEnabled()`.

**Controls** (add to any panel):

```lua
Panel:AddLabel({ Text = "Section notes", Muted = true, Bold = false })
Panel:AddDivider()
Panel:AddToggle({ Name = "Enable thing", Default = false, Callback = function(on) end })
Panel:AddSlider({ Name = "Amount", Min = 0, Max = 100, Default = 50, Callback = function(v) end })
Panel:AddDropdown({ Name = "Mode", Options = {"A", "B"}, Default = "A", Callback = function(v) end })
Panel:AddButton({ Name = "Do it", Icon = "play", Accent = true, Callback = function() end })
Panel:AddTextbox({ Name = "Label", Placeholder = "...", Callback = function(text, enterPressed) end })
Panel:AddKeybind({ Name = "Bind", Default = Enum.KeyCode.E, Callback = function() end, OnBind = function(key) end })

local Graph = Panel:AddGraph({ Name = "FPS", Points = 30, Min = 0, Suffix = " fps" })
Graph:Push(60) -- call repeatedly (e.g. every frame) to feed the graph
```

**Theme & accent:**

```lua
Window:SetTheme("Crimson")            -- one of Lib.Themes
Window:SetAccentColor("Blue")         -- or Window:SetAccentColor(Color3.fromRGB(...))
-- Presets: Lib.Presets.Green / .Blue / .Purple / .Red
```

**Icons:** pass a name from `Lib.Icons` (e.g. `"home"`, `"eye"`, `"wrench"`,
`"code"`, `"settings"`, `"key"`, `"play"`, `"info"`, `"search"`, `"sliders"`,
`"check"`, `"chevron-down"`, `"circle"`, `"x"`), `"lucide:name"` explicitly,
a raw `"rbxassetid://..."`, or a numeric asset ID.

**Misc:**

```lua
Window.MainFrame        -- root Frame; Window.MainFrame.Visible = false hides the whole UI
Window.Tabs              -- array of tabs, in the order added, e.g. Window.Tabs[2]
Window:Destroy()         -- fully removes the UI's ScreenGui
```

**Deep customization**, if you want to reskin beyond the config options
above — all of these are editable before or after `CreateWindow`:

```lua
Lib.Colors            -- base named colors
Lib.Icons             -- the Lucide icon → rbxassetid map
Lib.Themes            -- the 10 named theme tables
Lib.Presets           -- accent color presets
Lib.Customization      -- sizes, corners, text sizes, fonts, stroke, animation, drag, behavior
Lib.Customization.Behavior.DestroyOldUI = true -- a second CreateWindow with the same GuiName replaces the first
```
