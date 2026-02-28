-- // ================================================
-- //   MUSTAFA HUB - Cheat Friendly
-- //   Support: PC | Mobile | Controller
-- //   GUI Style: Sidebar + Content Panel
-- // ================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

local IsMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled
local IsPC = UserInputService.KeyboardEnabled

local States = {
    Fly = false, Speed = false, Noclip = false,
    Invisible = false, Fullbright = false
}
local BodyVelocity, BodyGyro
local MobileUpHeld, MobileDownHeld = false, false

local function GetChar() return LocalPlayer.Character end
local function GetHRP() local c = GetChar() return c and c:FindFirstChild("HumanoidRootPart") end
local function GetHum() local c = GetChar() return c and c:FindFirstChildOfClass("Humanoid") end

local function Notify(title, text)
    pcall(function()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = title, Text = text, Duration = 3
        })
    end)
end

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MustafaHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.IgnoreGuiInset = true
pcall(function() ScreenGui.Parent = game.CoreGui end)
if not ScreenGui.Parent then ScreenGui.Parent = LocalPlayer.PlayerGui end

-- Window
local Window = Instance.new("Frame", ScreenGui)
Window.Size = UDim2.new(0, 560, 0, 380)
Window.Position = UDim2.new(0.5, -280, 0.5, -190)
Window.BackgroundColor3 = Color3.fromRGB(13, 13, 18)
Window.BorderSizePixel = 0
Window.Active = true
Window.Draggable = true
Instance.new("UICorner", Window).CornerRadius = UDim.new(0, 12)
local WinStroke = Instance.new("UIStroke", Window)
WinStroke.Color = Color3.fromRGB(60, 60, 100)
WinStroke.Thickness = 1.2

-- Top Bar
local TopBar = Instance.new("Frame", Window)
TopBar.Size = UDim2.new(1, 0, 0, 36)
TopBar.BackgroundColor3 = Color3.fromRGB(10, 10, 15)
TopBar.BorderSizePixel = 0
Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0, 12)

local TopTitle = Instance.new("TextLabel", TopBar)
TopTitle.Size = UDim2.new(1, -120, 1, 0)
TopTitle.Position = UDim2.new(0, 14, 0, 0)
TopTitle.BackgroundTransparency = 1
TopTitle.Text = "🌙  Mustafa Hub  |  Cheat Friendly"
TopTitle.Font = Enum.Font.GothamBold
TopTitle.TextSize = 13
TopTitle.TextColor3 = Color3.fromRGB(200, 200, 255)
TopTitle.TextXAlignment = Enum.TextXAlignment.Left

local VerLabel = Instance.new("TextLabel", TopBar)
VerLabel.Size = UDim2.new(0, 80, 1, 0)
VerLabel.Position = UDim2.new(1, -170, 0, 0)
VerLabel.BackgroundTransparency = 1
VerLabel.Text = "v1.0 BETA"
VerLabel.Font = Enum.Font.Gotham
VerLabel.TextSize = 10
VerLabel.TextColor3 = Color3.fromRGB(100, 100, 150)
VerLabel.TextXAlignment = Enum.TextXAlignment.Right

local function MakeTopBtn(pos, color, txt)
    local b = Instance.new("TextButton", TopBar)
    b.Size = UDim2.new(0, 18, 0, 18)
    b.Position = UDim2.new(1, pos, 0.5, -9)
    b.BackgroundColor3 = color
    b.Text = txt
    b.Font = Enum.Font.GothamBold
    b.TextSize = 10
    b.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", b).CornerRadius = UDim.new(1,0)
    return b
end
local CloseBtn = MakeTopBtn(-12, Color3.fromRGB(255,60,60), "✕")
local MinBtn   = MakeTopBtn(-36, Color3.fromRGB(255,180,0), "−")

-- Sidebar
local Sidebar = Instance.new("Frame", Window)
Sidebar.Size = UDim2.new(0, 145, 1, -36)
Sidebar.Position = UDim2.new(0, 0, 0, 36)
Sidebar.BackgroundColor3 = Color3.fromRGB(10, 10, 16)
Sidebar.BorderSizePixel = 0
Instance.new("UICorner", Sidebar).CornerRadius = UDim.new(0, 12)

local SideList = Instance.new("UIListLayout", Sidebar)
SideList.Padding = UDim.new(0, 2)
SideList.SortOrder = Enum.SortOrder.LayoutOrder

local SidePad = Instance.new("UIPadding", Sidebar)
SidePad.PaddingTop = UDim.new(0, 10)
SidePad.PaddingLeft = UDim.new(0, 8)
SidePad.PaddingRight = UDim.new(0, 8)

-- Content
local Content = Instance.new("Frame", Window)
Content.Size = UDim2.new(1, -153, 1, -44)
Content.Position = UDim2.new(0, 149, 0, 40)
Content.BackgroundColor3 = Color3.fromRGB(16, 16, 24)
Content.BorderSizePixel = 0
Instance.new("UICorner", Content).CornerRadius = UDim.new(0, 10)

local Pages = {}
local SideBtns = {}
local CurrentPage = nil

local function NewPage(name)
    local page = Instance.new("ScrollingFrame", Content)
    page.Name = name
    page.Size = UDim2.new(1, 0, 1, 0)
    page.BackgroundTransparency = 1
    page.BorderSizePixel = 0
    page.ScrollBarThickness = 3
    page.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 200)
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.Visible = false
    local layout = Instance.new("UIListLayout", page)
    layout.Padding = UDim.new(0, 6)
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    local pad = Instance.new("UIPadding", page)
    pad.PaddingTop = UDim.new(0, 10)
    pad.PaddingLeft = UDim.new(0, 10)
    pad.PaddingRight = UDim.new(0, 12)
    pad.PaddingBottom = UDim.new(0, 10)
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        page.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 20)
    end)
    Pages[name] = page
    return page
end

local function SideLabel(txt)
    local l = Instance.new("TextLabel", Sidebar)
    l.Size = UDim2.new(1, 0, 0, 22)
    l.BackgroundTransparency = 1
    l.Text = txt
    l.Font = Enum.Font.GothamBold
    l.TextSize = 9
    l.TextColor3 = Color3.fromRGB(70, 70, 120)
    l.TextXAlignment = Enum.TextXAlignment.Left
end

local function SideBtn(icon, label, pageName)
    local Btn = Instance.new("TextButton", Sidebar)
    Btn.Size = UDim2.new(1, 0, 0, 36)
    Btn.BackgroundTransparency = 1
    Btn.Text = ""
    Btn.AutoButtonColor = false
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 8)

    local IconL = Instance.new("TextLabel", Btn)
    IconL.Size = UDim2.new(0, 26, 1, 0)
    IconL.Position = UDim2.new(0, 4, 0, 0)
    IconL.BackgroundTransparency = 1
    IconL.Text = icon
    IconL.TextSize = 16
    IconL.Font = Enum.Font.GothamBold
    IconL.TextColor3 = Color3.fromRGB(130, 130, 200)

    local NameL = Instance.new("TextLabel", Btn)
    NameL.Size = UDim2.new(1, -34, 1, 0)
    NameL.Position = UDim2.new(0, 32, 0, 0)
    NameL.BackgroundTransparency = 1
    NameL.Text = label
    NameL.Font = Enum.Font.GothamSemibold
    NameL.TextSize = 12
    NameL.TextColor3 = Color3.fromRGB(170, 170, 200)
    NameL.TextXAlignment = Enum.TextXAlignment.Left

    local Indicator = Instance.new("Frame", Btn)
    Indicator.Size = UDim2.new(0, 3, 0.6, 0)
    Indicator.Position = UDim2.new(0, -8, 0.2, 0)
    Indicator.BackgroundColor3 = Color3.fromRGB(100, 100, 255)
    Indicator.Visible = false
    Instance.new("UICorner", Indicator).CornerRadius = UDim.new(1,0)

    local function SetActive(v)
        if v then
            TweenService:Create(Btn, TweenInfo.new(0.2), {BackgroundTransparency = 0, BackgroundColor3 = Color3.fromRGB(25,25,45)}):Play()
            NameL.TextColor3 = Color3.fromRGB(220,220,255)
            IconL.TextColor3 = Color3.fromRGB(150,150,255)
            Indicator.Visible = true
        else
            TweenService:Create(Btn, TweenInfo.new(0.2), {BackgroundTransparency = 1}):Play()
            NameL.TextColor3 = Color3.fromRGB(170,170,200)
            IconL.TextColor3 = Color3.fromRGB(130,130,200)
            Indicator.Visible = false
        end
    end

    SideBtns[pageName] = SetActive

    Btn.MouseButton1Click:Connect(function()
        if CurrentPage then
            Pages[CurrentPage].Visible = false
            if SideBtns[CurrentPage] then SideBtns[CurrentPage](false) end
        end
        CurrentPage = pageName
        Pages[pageName].Visible = true
        SetActive(true)
    end)

    Btn.MouseEnter:Connect(function()
        if CurrentPage ~= pageName then
            Btn.BackgroundTransparency = 0.6
            Btn.BackgroundColor3 = Color3.fromRGB(22,22,38)
        end
    end)
    Btn.MouseLeave:Connect(function()
        if CurrentPage ~= pageName then
            Btn.BackgroundTransparency = 1
        end
    end)
end

local function SectionHeader(page, txt)
    local L = Instance.new("TextLabel", page)
    L.Size = UDim2.new(1, 0, 0, 22)
    L.BackgroundTransparency = 1
    L.Text = txt
    L.Font = Enum.Font.GothamBold
    L.TextSize = 10
    L.TextColor3 = Color3.fromRGB(80, 80, 160)
    L.TextXAlignment = Enum.TextXAlignment.Left
end

local function ToggleRow(page, icon, title, subtitle, color, callback)
    local Row = Instance.new("Frame", page)
    Row.Size = UDim2.new(1, 0, 0, subtitle and 54 or 44)
    Row.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    Row.BackgroundTransparency = 0.3
    Instance.new("UICorner", Row).CornerRadius = UDim.new(0, 8)

    local IconL = Instance.new("TextLabel", Row)
    IconL.Size = UDim2.new(0, 30, 1, 0)
    IconL.Position = UDim2.new(0, 10, 0, 0)
    IconL.BackgroundTransparency = 1
    IconL.Text = icon
    IconL.TextSize = 18
    IconL.Font = Enum.Font.GothamBold
    IconL.TextColor3 = Color3.fromRGB(255,255,255)

    local TitleL = Instance.new("TextLabel", Row)
    TitleL.Size = UDim2.new(1, -90, 0, 22)
    TitleL.Position = UDim2.new(0, 44, 0, subtitle and 8 or 11)
    TitleL.BackgroundTransparency = 1
    TitleL.Text = title
    TitleL.Font = Enum.Font.GothamSemibold
    TitleL.TextSize = 13
    TitleL.TextColor3 = Color3.fromRGB(220, 220, 230)
    TitleL.TextXAlignment = Enum.TextXAlignment.Left

    if subtitle then
        local SubL = Instance.new("TextLabel", Row)
        SubL.Size = UDim2.new(1, -90, 0, 16)
        SubL.Position = UDim2.new(0, 44, 0, 28)
        SubL.BackgroundTransparency = 1
        SubL.Text = subtitle
        SubL.Font = Enum.Font.Gotham
        SubL.TextSize = 10
        SubL.TextColor3 = Color3.fromRGB(100, 100, 140)
        SubL.TextXAlignment = Enum.TextXAlignment.Left
    end

    local PillBG = Instance.new("Frame", Row)
    PillBG.Size = UDim2.new(0, 44, 0, 24)
    PillBG.Position = UDim2.new(1, -56, 0.5, -12)
    PillBG.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
    Instance.new("UICorner", PillBG).CornerRadius = UDim.new(1, 0)

    local PillDot = Instance.new("Frame", PillBG)
    PillDot.Size = UDim2.new(0, 18, 0, 18)
    PillDot.Position = UDim2.new(0, 3, 0.5, -9)
    PillDot.BackgroundColor3 = Color3.fromRGB(160, 160, 180)
    Instance.new("UICorner", PillDot).CornerRadius = UDim.new(1, 0)

    local toggled = false
    local function Toggle(state)
        toggled = state
        if state then
            TweenService:Create(PillBG, TweenInfo.new(0.2), {BackgroundColor3 = color or Color3.fromRGB(80,80,220)}):Play()
            TweenService:Create(PillDot, TweenInfo.new(0.2), {Position = UDim2.new(0, 23, 0.5, -9), BackgroundColor3 = Color3.fromRGB(255,255,255)}):Play()
        else
            TweenService:Create(PillBG, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40,40,60)}):Play()
            TweenService:Create(PillDot, TweenInfo.new(0.2), {Position = UDim2.new(0, 3, 0.5, -9), BackgroundColor3 = Color3.fromRGB(160,160,180)}):Play()
        end
    end

    local ClickArea = Instance.new("TextButton", Row)
    ClickArea.Size = UDim2.new(1, 0, 1, 0)
    ClickArea.BackgroundTransparency = 1
    ClickArea.Text = ""
    ClickArea.MouseButton1Click:Connect(function()
        toggled = not toggled
        Toggle(toggled)
        if callback then callback(toggled) end
    end)
end

local function ActionRow(page, icon, title, subtitle, color, callback)
    local Row = Instance.new("TextButton", page)
    Row.Size = UDim2.new(1, 0, 0, subtitle and 54 or 44)
    Row.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    Row.BackgroundTransparency = 0.3
    Row.Text = ""
    Row.AutoButtonColor = false
    Instance.new("UICorner", Row).CornerRadius = UDim.new(0, 8)

    local IconL = Instance.new("TextLabel", Row)
    IconL.Size = UDim2.new(0, 30, 1, 0)
    IconL.Position = UDim2.new(0, 10, 0, 0)
    IconL.BackgroundTransparency = 1
    IconL.Text = icon
    IconL.TextSize = 18
    IconL.Font = Enum.Font.GothamBold
    IconL.TextColor3 = Color3.fromRGB(255,255,255)

    local TitleL = Instance.new("TextLabel", Row)
    TitleL.Size = UDim2.new(1, -70, 0, 22)
    TitleL.Position = UDim2.new(0, 44, 0, subtitle and 8 or 11)
    TitleL.BackgroundTransparency = 1
    TitleL.Text = title
    TitleL.Font = Enum.Font.GothamSemibold
    TitleL.TextSize = 13
    TitleL.TextColor3 = Color3.fromRGB(220, 220, 230)
    TitleL.TextXAlignment = Enum.TextXAlignment.Left

    if subtitle then
        local SubL = Instance.new("TextLabel", Row)
        SubL.Size = UDim2.new(1, -70, 0, 16)
        SubL.Position = UDim2.new(0, 44, 0, 28)
        SubL.BackgroundTransparency = 1
        SubL.Text = subtitle
        SubL.Font = Enum.Font.Gotham
        SubL.TextSize = 10
        SubL.TextColor3 = Color3.fromRGB(100, 100, 140)
        SubL.TextXAlignment = Enum.TextXAlignment.Left
    end

    local Arrow = Instance.new("TextLabel", Row)
    Arrow.Size = UDim2.new(0, 24, 1, 0)
    Arrow.Position = UDim2.new(1, -30, 0, 0)
    Arrow.BackgroundTransparency = 1
    Arrow.Text = "›"
    Arrow.Font = Enum.Font.GothamBold
    Arrow.TextSize = 20
    Arrow.TextColor3 = color or Color3.fromRGB(90,90,200)

    Row.MouseEnter:Connect(function()
        TweenService:Create(Row, TweenInfo.new(0.15), {BackgroundTransparency = 0.1, BackgroundColor3 = Color3.fromRGB(28,28,42)}):Play()
    end)
    Row.MouseLeave:Connect(function()
        TweenService:Create(Row, TweenInfo.new(0.15), {BackgroundTransparency = 0.3, BackgroundColor3 = Color3.fromRGB(20,20,30)}):Play()
    end)
    Row.MouseButton1Click:Connect(function()
        if callback then callback() end
    end)
end

-- SIDEBAR SETUP
SideLabel("  MAIN")
SideBtn("🏠", "Home", "Home")
SideBtn("🚶", "Movement", "Movement")
SideBtn("👤", "Player", "Player")
SideBtn("🔧", "Utility", "Utility")
SideLabel("  INFO")
SideBtn("ℹ️", "About", "About")

-- HOME PAGE
local HomePage = NewPage("Home")
SectionHeader(HomePage, "  WELCOME")

local WelcomeBox = Instance.new("Frame", HomePage)
WelcomeBox.Size = UDim2.new(1, 0, 0, 75)
WelcomeBox.BackgroundColor3 = Color3.fromRGB(20,20,40)
WelcomeBox.BackgroundTransparency = 0.2
Instance.new("UICorner", WelcomeBox).CornerRadius = UDim.new(0,10)
local WStroke = Instance.new("UIStroke", WelcomeBox)
WStroke.Color = Color3.fromRGB(80,80,200) WStroke.Thickness = 1 WStroke.Transparency = 0.5

local WIcon = Instance.new("TextLabel", WelcomeBox)
WIcon.Size = UDim2.new(0,50,1,0) WIcon.Position = UDim2.new(0,10,0,0)
WIcon.BackgroundTransparency = 1 WIcon.Text = "🌙" WIcon.TextSize = 30
WIcon.Font = Enum.Font.GothamBold WIcon.TextColor3 = Color3.fromRGB(200,200,255)

local WText = Instance.new("TextLabel", WelcomeBox)
WText.Size = UDim2.new(1,-70,1,0) WText.Position = UDim2.new(0,65,0,0)
WText.BackgroundTransparency = 1
WText.Text = "Mustafa Hub  v1.0 BETA\nCheat Friendly\n" .. (IsMobile and "📱 Mobile Mode" or "💻 PC Mode")
WText.Font = Enum.Font.GothamSemibold WText.TextSize = 12
WText.TextColor3 = Color3.fromRGB(200,200,230) WText.TextXAlignment = Enum.TextXAlignment.Left

SectionHeader(HomePage, "  QUICK TOGGLE")
ToggleRow(HomePage, "🛸", "Fly", "WASD+Space | Mobile Btn", Color3.fromRGB(100,100,255), function(v)
    States.Fly = v
    if v then
        local hrp = GetHRP() if not hrp then return end
        BodyVelocity = Instance.new("BodyVelocity") BodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5) BodyVelocity.Parent = hrp
        BodyGyro = Instance.new("BodyGyro") BodyGyro.MaxTorque = Vector3.new(1e5,1e5,1e5) BodyGyro.P = 1e4 BodyGyro.Parent = hrp
    else
        if BodyVelocity then BodyVelocity:Destroy() BodyVelocity = nil end
        if BodyGyro then BodyGyro:Destroy() BodyGyro = nil end
    end
    Notify("🛸 Fly", v and "Aktif!" or "Dimatikan")
end)
ToggleRow(HomePage, "⚡", "Speed Hack", "WalkSpeed: 80", Color3.fromRGB(255,200,0), function(v)
    States.Speed = v
    local hum = GetHum() if hum then hum.WalkSpeed = v and 80 or 16 end
    Notify("⚡ Speed", v and "Aktif!" or "Dimatikan")
end)

-- MOVEMENT PAGE
local MovePage = NewPage("Movement")
SectionHeader(MovePage, "  MOVEMENT")
ToggleRow(MovePage, "🛸", "Fly", "WASD+Space/Shift | Mobile UP/DN | Controller LS", Color3.fromRGB(100,100,255), function(v)
    States.Fly = v
    if v then
        local hrp = GetHRP() if not hrp then return end
        BodyVelocity = Instance.new("BodyVelocity") BodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5) BodyVelocity.Parent = hrp
        BodyGyro = Instance.new("BodyGyro") BodyGyro.MaxTorque = Vector3.new(1e5,1e5,1e5) BodyGyro.P = 1e4 BodyGyro.Parent = hrp
    else
        if BodyVelocity then BodyVelocity:Destroy() BodyVelocity = nil end
        if BodyGyro then BodyGyro:Destroy() BodyGyro = nil end
    end
end)
ToggleRow(MovePage, "⚡", "Speed Hack", "WalkSpeed 16 → 80", Color3.fromRGB(255,200,0), function(v)
    States.Speed = v
    local hum = GetHum() if hum then hum.WalkSpeed = v and 80 or 16 end
end)
ToggleRow(MovePage, "👻", "Noclip", "Menembus dinding/objek", Color3.fromRGB(150,0,255), function(v)
    States.Noclip = v
    Notify("👻 Noclip", v and "Aktif!" or "Dimatikan")
end)
ActionRow(MovePage, "📍", "TP Tool", "Klik objek apapun untuk teleport", Color3.fromRGB(0,255,150), function()
    local ex = LocalPlayer.Backpack:FindFirstChild("TP Tool") or (GetChar() and GetChar():FindFirstChild("TP Tool"))
    if ex then ex:Destroy() end
    local Tool = Instance.new("Tool") Tool.Name = "TP Tool" Tool.RequiresHandle = false Tool.Parent = LocalPlayer.Backpack
    Tool.Activated:Connect(function()
        local Mouse = LocalPlayer:GetMouse() local hrp = GetHRP()
        if hrp and Mouse.Target then hrp.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0,3,0)) end
    end)
    Notify("📍 TP Tool","Equipped!")
end)

-- PLAYER PAGE
local PlayerPage = NewPage("Player")
SectionHeader(PlayerPage, "  PLAYER")
ToggleRow(PlayerPage, "🌫️", "Invisible", "Tidak terlihat oleh player lain", Color3.fromRGB(0,200,255), function(v)
    States.Invisible = v
    local char = GetChar()
    if char then
        for _, p in pairs(char:GetDescendants()) do
            if p:IsA("BasePart") or p:IsA("Decal") then p.Transparency = v and 1 or 0 end
        end
    end
    Notify("🌫️ Invisible", v and "Tidak terlihat!" or "Visible kembali")
end)
ToggleRow(PlayerPage, "☀️", "Fullbright", "Hapus kegelapan di map", Color3.fromRGB(255,220,0), function(v)
    States.Fullbright = v
    local L = game:GetService("Lighting")
    L.Brightness = v and 10 or 1
    L.FogEnd = v and 9e9 or 100000
    Notify("☀️ Fullbright", v and "Aktif!" or "Dimatikan")
end)
SectionHeader(PlayerPage, "  ACTIONS")
ActionRow(PlayerPage, "💀", "Reset Character", "Matikan karakter kamu", Color3.fromRGB(255,60,60), function()
    local hum = GetHum() if hum then hum.Health = 0 end
end)

-- UTILITY PAGE
local UtilPage = NewPage("Utility")
SectionHeader(UtilPage, "  SERVER")
ActionRow(UtilPage, "🔄", "Rejoin", "Masuk ulang ke server ini", Color3.fromRGB(100,180,255), function()
    Notify("🔄 Rejoin","Rejoining...") task.wait(1)
    game:GetService("TeleportService"):Teleport(game.PlaceId, LocalPlayer)
end)
ActionRow(UtilPage, "📋", "Copy Server ID", "Salin ID server saat ini", Color3.fromRGB(180,180,255), function()
    local id = game.JobId pcall(function() setclipboard(id) end)
    Notify("📋 Server ID", id ~= "" and "Copied!" or "Tidak tersedia")
end)
ActionRow(UtilPage, "🔁", "Respawn", "Paksa respawn karakter", Color3.fromRGB(0,255,180), function()
    LocalPlayer:LoadCharacter()
end)

-- ABOUT PAGE
local AboutPage = NewPage("About")
local ABox = Instance.new("Frame", AboutPage)
ABox.Size = UDim2.new(1,0,0,150) ABox.BackgroundColor3 = Color3.fromRGB(18,18,30)
ABox.BackgroundTransparency = 0.2 Instance.new("UICorner", ABox).CornerRadius = UDim.new(0,10)
local AStroke = Instance.new("UIStroke",ABox) AStroke.Color = Color3.fromRGB(80,80,200) AStroke.Thickness=1 AStroke.Transparency=0.5
local AText = Instance.new("TextLabel",ABox)
AText.Size = UDim2.new(1,-20,1,-20) AText.Position = UDim2.new(0,10,0,10)
AText.BackgroundTransparency = 1
AText.Text = "🌙  Mustafa Hub\n\nVersi: v1.0 BETA\nStatus: Cheat Friendly\nSupport: PC | Mobile | Controller\n\n• Fly  • Speed  • Noclip\n• Invisible  • TP Tool  • Fullbright\n• Rejoin  • Reset  • Copy ID"
AText.Font = Enum.Font.GothamSemibold AText.TextSize = 12
AText.TextColor3 = Color3.fromRGB(180,180,220)
AText.TextXAlignment = Enum.TextXAlignment.Left AText.TextYAlignment = Enum.TextYAlignment.Top

-- FLY LOOP
RunService.RenderStepped:Connect(function()
    if States.Fly and BodyVelocity and BodyGyro then
        local Camera = workspace.CurrentCamera
        local Move = Vector3.zero
        if IsPC then
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then Move += Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then Move -= Camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then Move -= Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then Move += Camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then Move += Vector3.new(0,1,0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then Move -= Vector3.new(0,1,0) end
        end
        local GP = UserInputService:GetConnectedGamepads()
        if #GP > 0 then
            for _, axis in pairs(UserInputService:GetGamepadState(GP[1])) do
                if axis.KeyCode == Enum.KeyCode.Thumbstick1 then
                    Move += Camera.CFrame.LookVector * axis.Position.Y
                    Move += Camera.CFrame.RightVector * axis.Position.X
                end
            end
            if UserInputService:IsGamepadButtonDown(GP[1], Enum.KeyCode.ButtonA) then Move += Vector3.new(0,1,0) end
            if UserInputService:IsGamepadButtonDown(GP[1], Enum.KeyCode.ButtonB) then Move -= Vector3.new(0,1,0) end
        end
        if MobileUpHeld then Move += Vector3.new(0,1,0) end
        if MobileDownHeld then Move -= Vector3.new(0,1,0) end
        BodyVelocity.Velocity = Move * 60
        BodyGyro.CFrame = Camera.CFrame
    end
end)

RunService.Stepped:Connect(function()
    if States.Noclip then
        local char = GetChar()
        if char then
            for _, p in pairs(char:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide = false end
            end
        end
    end
end)

-- MOBILE FLY BUTTONS
if IsMobile then
    local MF = Instance.new("Frame", ScreenGui)
    MF.Size = UDim2.new(0,120,0,65) MF.Position = UDim2.new(1,-135,1,-85)
    MF.BackgroundColor3 = Color3.fromRGB(13,13,20) MF.BackgroundTransparency = 0.3
    Instance.new("UICorner", MF).CornerRadius = UDim.new(0,10)
    local MU = Instance.new("TextButton", MF)
    MU.Size = UDim2.new(0.48,0,1,-8) MU.Position = UDim2.new(0,4,0,4)
    MU.BackgroundColor3 = Color3.fromRGB(60,60,180) MU.Text = "▲"
    MU.Font = Enum.Font.GothamBold MU.TextSize = 18 MU.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", MU).CornerRadius = UDim.new(0,8)
    local MD = Instance.new("TextButton", MF)
    MD.Size = UDim2.new(0.48,0,1,-8) MD.Position = UDim2.new(0.52,0,0,4)
    MD.BackgroundColor3 = Color3.fromRGB(180,60,60) MD.Text = "▼"
    MD.Font = Enum.Font.GothamBold MD.TextSize = 18 MD.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", MD).CornerRadius = UDim.new(0,8)
    MU.MouseButton1Down:Connect(function() MobileUpHeld = true end)
    MU.MouseButton1Up:Connect(function() MobileUpHeld = false end)
    MD.MouseButton1Down:Connect(function() MobileDownHeld = true end)
    MD.MouseButton1Up:Connect(function() MobileDownHeld = false end)
end

-- OPEN BUTTON
local OpenBtn = Instance.new("TextButton", ScreenGui)
OpenBtn.Size = UDim2.new(0,46,0,46) OpenBtn.Position = UDim2.new(0,8,0.5,-23)
OpenBtn.BackgroundColor3 = Color3.fromRGB(15,15,28) OpenBtn.BackgroundTransparency = 0.1
OpenBtn.Text = "🌙" OpenBtn.Font = Enum.Font.GothamBold OpenBtn.TextSize = 20
OpenBtn.TextColor3 = Color3.fromRGB(150,150,255) OpenBtn.Visible = false
Instance.new("UICorner", OpenBtn).CornerRadius = UDim.new(1,0)
local OStroke = Instance.new("UIStroke", OpenBtn)
OStroke.Color = Color3.fromRGB(90,90,255) OStroke.Thickness = 1.5
OpenBtn.MouseButton1Click:Connect(function()
    Window.Visible = true OpenBtn.Visible = false
end)

-- CLOSE & MINIMIZE
CloseBtn.MouseButton1Click:Connect(function()
    Window.Visible = false OpenBtn.Visible = true
end)

local Minimized = false
MinBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    TweenService:Create(Window, TweenInfo.new(0.3), {Size = Minimized and UDim2.new(0,560,0,36) or UDim2.new(0,560,0,380)}):Play()
end)

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        Window.Visible = not Window.Visible
        OpenBtn.Visible = not Window.Visible
    end
end)

UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.ButtonSelect then
        Window.Visible = not Window.Visible
        OpenBtn.Visible = not Window.Visible
    end
end)

-- DEFAULT PAGE
Pages["Home"].Visible = true
CurrentPage = "Home"
if SideBtns["Home"] then SideBtns["Home"](true) end

Notify("🌙 Mustafa Hub", "Loaded! " .. (IsMobile and "📱 Mobile" or "💻 PC") .. " Mode")
