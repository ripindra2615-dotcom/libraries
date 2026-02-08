# WindUI Example Hub

A comprehensive example showcasing all features of the WindUI library for Roblox.

## Features

- 🏠 **Main Tab** - Character modifications (Speed, Jump Power, No Clip)
- ⚔️ **Combat Tab** - Combat features (Kill Aura, Reach, Target Priority)
- 👁️ **Visuals Tab** - Visual enhancements (ESP, Color pickers, Fullbright)
- ⚙️ **Settings Tab** - UI configuration (Keybinds, Themes, Config management)

## Installation


```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/ripindra2615-dotcom/libraries/refs/heads/main/WindUI/Source.lua"))()
```

## Full Code
```lua
-- Load WindUI Library
local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/ripindra2615-dotcom/libraries/refs/heads/main/WindUI/Source.lua"))()

-- Create Main Window
local Window = WindUI:CreateWindow({
    Title = "Example Hub",
    Icon = "zap",
    Author = "by Example Creator",
    Folder = "ExampleHub",
    Size = UDim2.fromOffset(580, 460),
    Transparent = true,
    Theme = "Dark",
    Resizable = true,
    SideBarWidth = 200,
    
    User = {
        Enabled = true,
        Anonymous = false,
        Callback = function()
            WindUI:Notify({
                Title = "Profile",
                Content = "User profile clicked!",
                Duration = 2,
                Icon = "user",
            })
        end,
    },
})

-- Create Tabs
local MainTab = Window:Tab({
    Title = "Main",
    Icon = "home",
})

local CombatTab = Window:Tab({
    Title = "Combat",
    Icon = "sword",
})

local VisualsTab = Window:Tab({
    Title = "Visuals",
    Icon = "eye",
})

local SettingsTab = Window:Tab({
    Title = "Settings",
    Icon = "settings",
})

-- Add version tag
Window:Tag({
    Title = "v1.0.0",
    Icon = "github",
    Color = Color3.fromHex("#30ff6a"),
    Radius = 8,
})

-- Main Tab Content
MainTab:Paragraph({
    Title = "Welcome to Example Hub!",
    Desc = "This is a fully functional example showcasing WindUI features. Explore the tabs to see different elements!",
    Color = "Blue",
    Image = "rbxassetid://0",
    ImageSize = 0,
})

MainTab:Divider()

local playerSpeed = 16
local playerJumpPower = 50

local SpeedSlider = MainTab:Slider({
    Title = "Walkspeed",
    Desc = "Adjust your character's walkspeed",
    Step = 1,
    Value = {
        Min = 16,
        Max = 200,
        Default = 16,
    },
    Callback = function(value)
        playerSpeed = value
        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
        end
    end
})

local JumpSlider = MainTab:Slider({
    Title = "Jump Power",
    Desc = "Adjust your character's jump power",
    Step = 5,
    Value = {
        Min = 50,
        Max = 300,
        Default = 50,
    },
    Callback = function(value)
        playerJumpPower = value
        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
        end
    end
})

MainTab:Space()

local NoClipEnabled = false
local NoClipToggle = MainTab:Toggle({
    Title = "No Clip",
    Desc = "Walk through walls",
    Icon = "ghost",
    Type = "Checkbox",
    Value = false,
    Callback = function(state)
        NoClipEnabled = state
        if state then
            WindUI:Notify({
                Title = "No Clip",
                Content = "No Clip enabled!",
                Duration = 2,
                Icon = "check",
            })
        end
    end
})

-- Combat Tab Content
CombatTab:Section({
    Title = "Combat Features",
    Opened = true,
})

local KillAuraToggle = CombatTab:Toggle({
    Title = "Kill Aura",
    Desc = "Automatically attack nearby enemies",
    Icon = "crosshair",
    Type = "Checkbox",
    Value = false,
    Callback = function(state)
        print("Kill Aura: " .. tostring(state))
    end
})

local ReachSlider = CombatTab:Slider({
    Title = "Reach Distance",
    Desc = "Extend your attack range",
    Step = 1,
    Value = {
        Min = 3,
        Max = 20,
        Default = 3,
    },
    Callback = function(value)
        print("Reach set to: " .. value)
    end
})

CombatTab:Divider()

local TargetDropdown = CombatTab:Dropdown({
    Title = "Target Priority",
    Desc = "Choose targeting priority",
    Values = { "Closest", "Lowest Health", "Highest Health", "Random" },
    Value = "Closest",
    Callback = function(option)
        print("Target priority: " .. option)
    end
})

-- Visuals Tab Content
VisualsTab:Section({
    Title = "ESP Settings",
    Opened = true,
})

local ESPToggle = VisualsTab:Toggle({
    Title = "Player ESP",
    Desc = "See players through walls",
    Icon = "eye",
    Type = "Checkbox",
    Value = false,
    Callback = function(state)
        print("ESP: " .. tostring(state))
    end
})

local ESPColor = VisualsTab:Colorpicker({
    Title = "ESP Color",
    Desc = "Choose ESP highlight color",
    Default = Color3.fromRGB(255, 0, 0),
    Transparency = 0,
    Callback = function(color)
        print("ESP Color: " .. tostring(color))
    end
})

VisualsTab:Space()

local AmbientColor = VisualsTab:Colorpicker({
    Title = "Ambient Color",
    Desc = "Change world ambient lighting",
    Default = Color3.fromRGB(255, 255, 255),
    Transparency = 0,
    Callback = function(color)
        game.Lighting.Ambient = color
    end
})

local FullbrightToggle = VisualsTab:Toggle({
    Title = "Fullbright",
    Desc = "Light up the world",
    Icon = "sun",
    Type = "Checkbox",
    Value = false,
    Callback = function(state)
        if state then
            game.Lighting.Brightness = 2
            game.Lighting.ClockTime = 14
            game.Lighting.FogEnd = 100000
        else
            game.Lighting.Brightness = 1
            game.Lighting.ClockTime = 12
            game.Lighting.FogEnd = 10000
        end
    end
})

-- Settings Tab Content
SettingsTab:Section({
    Title = "UI Settings",
    Opened = true,
})

local UIKeybind = SettingsTab:Keybind({
    Title = "Toggle UI Key",
    Desc = "Keybind to show/hide the UI",
    Value = "RightShift",
    Callback = function(v)
        Window:SetToggleKey(Enum.KeyCode[v])
        WindUI:Notify({
            Title = "Keybind Changed",
            Content = "UI toggle key set to: " .. v,
            Duration = 2,
            Icon = "key",
        })
    end
})

SettingsTab:Divider()

local ThemeDropdown = SettingsTab:Dropdown({
    Title = "Theme",
    Desc = "Select UI theme",
    Values = { "Dark", "Light", "Mocha" },
    Value = "Dark",
    Callback = function(option)
        Window:SetTheme(option)
    end
})

SettingsTab:Space()

local ConfigInput = SettingsTab:Input({
    Title = "Config Name",
    Desc = "Enter a name for your config",
    Value = "",
    Placeholder = "MyConfig",
    Type = "Input",
    Callback = function(input)
        print("Config name: " .. input)
    end
})

local SaveButton = SettingsTab:Button({
    Title = "Save Config",
    Desc = "Save current settings",
    Callback = function()
        WindUI:Notify({
            Title = "Config Saved",
            Content = "Your settings have been saved!",
            Duration = 3,
            Icon = "save",
        })
    end
})

local LoadButton = SettingsTab:Button({
    Title = "Load Config",
    Desc = "Load saved settings",
    Callback = function()
        WindUI:Notify({
            Title = "Config Loaded",
            Content = "Settings loaded successfully!",
            Duration = 3,
            Icon = "folder-open",
        })
    end
})

SettingsTab:Divider()

local DestroyButton = SettingsTab:Button({
    Title = "Destroy UI",
    Desc = "Completely remove the UI",
    Callback = function()
        local Dialog = Window:Dialog({
            Icon = "alert-triangle",
            Title = "Confirm Destruction",
            Content = "Are you sure you want to destroy the UI? This cannot be undone.",
            Buttons = {
                {
                    Title = "Yes, Destroy",
                    Callback = function()
                        Window:Destroy()
                    end,
                },
                {
                    Title = "Cancel",
                    Callback = function()
                        print("Cancelled")
                    end,
                },
            },
        })
        Dialog:Show()
    end
})

-- Show welcome notification
WindUI:Notify({
    Title = "Welcome!",
    Content = "Example Hub loaded successfully!",
    Duration = 4,
    Icon = "check-circle",
})

-- Select Main tab by default
MainTab:Select()

-- No Clip Loop (if enabled)
game:GetService("RunService").Stepped:Connect(function()
    if NoClipEnabled and game.Players.LocalPlayer.Character then
        for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)
```

## Components Demonstrated

### Window Elements
- ✅ Window creation with custom settings
- ✅ Multiple tabs with icons
- ✅ Version tag
- ✅ User profile button

### UI Elements
- ✅ **Paragraph** - Welcome message with description
- ✅ **Slider** - Walkspeed and Jump Power controls
- ✅ **Toggle** - Checkbox-style toggles for features
- ✅ **Section** - Organized content sections
- ✅ **Dropdown** - Selection menus
- ✅ **Colorpicker** - Color selection with transparency
- ✅ **Keybind** - Customizable key bindings
- ✅ **Input** - Text input fields
- ✅ **Button** - Action buttons
- ✅ **Dialog** - Confirmation dialogs
- ✅ **Divider** - Visual separators
- ✅ **Space** - Vertical spacing
- ✅ **Notify** - Toast notifications

## Customization

Feel free to modify:
- Window title, icon, and author
- Tab names and icons
- Slider min/max values
- Default colors
- Keybinds
- Any functionality to suit your needs

## Credits

- **WindUI Library** - [DeividComSono](https://github.com/DeividComSono/Wind-UI)
- **Example Code** - Created as a comprehensive demonstration

## License

This example is provided as-is for educational purposes. Modify and use as needed.

---

**Note:** This is an example hub. Actual game-specific functionality would need to be implemented based on the target game's structure.
