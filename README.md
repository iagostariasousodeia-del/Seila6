-- ================================================
-- LAZARUS UI LIBRARY - STANDALONE
-- ================================================

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")

local Library = {}
Library.__index = Library

local Themes = {
    Neon = {
        Background   = Color3.fromRGB(5, 5, 12),
        Secondary    = Color3.fromRGB(8, 8, 18),
        Accent       = Color3.fromRGB(0, 255, 180),
        AccentHover  = Color3.fromRGB(0, 230, 160),
        Text         = Color3.fromRGB(220, 255, 245),
        SubText      = Color3.fromRGB(100, 180, 160),
        Element      = Color3.fromRGB(12, 20, 28),
        ElementHover = Color3.fromRGB(18, 30, 40),
        Border       = Color3.fromRGB(0, 80, 60),
        Toggle_On    = Color3.fromRGB(0, 255, 180),
        Toggle_Off   = Color3.fromRGB(30, 50, 45),
        TopBar       = Color3.fromRGB(6, 10, 16),
    },
}

-- Funções Utilitárias
local function Tween(obj, props, dur)
    TweenService:Create(obj, TweenInfo.new(dur or 0.2), props):Play()
end

local function Corner(parent, radius)
    local c = Instance.new("UICorner", parent)
    c.CornerRadius = UDim.new(0, radius or 6)
    return c
end

local function Stroke(parent, color, thickness)
    local s = Instance.new("UIStroke", parent)
    s.Color = color
    s.Thickness = thickness or 1
    s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    return s
end

local function MakeDraggable(handle, frame)
    local dragging, dragStart, startPos
    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

function Library:MakeWindow(config)
    local T = Themes.Neon
    local Title = config.Title or "GUI"
    local Size = config.Size or {Width = 380, Height = 320}

    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "CustomGui"
    pcall(function() ScreenGui.Parent = CoreGui end)
    if not ScreenGui.Parent then ScreenGui.Parent = Players.LocalPlayer.PlayerGui end

    local MainFrame = Instance.new("Frame", ScreenGui)
    MainFrame.Size = UDim2.new(0, Size.Width, 0, Size.Height)
    MainFrame.Position = UDim2.new(0.5, -(Size.Width/2), 0.5, -(Size.Height/2))
    MainFrame.BackgroundColor3 = T.Background
    Corner(MainFrame, 10)
    Stroke(MainFrame, T.Border, 1.5)

    local TopBar = Instance.new("Frame", MainFrame)
    TopBar.Size = UDim2.new(1, 0, 0, 36)
    TopBar.BackgroundColor3 = T.TopBar
    Corner(TopBar, 10)
    MakeDraggable(TopBar, MainFrame)

    local TitleLabel = Instance.new("TextLabel", TopBar)
    TitleLabel.Size = UDim2.new(1, -10, 1, 0)
    TitleLabel.Position = UDim2.new(0, 10, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = Title
    TitleLabel.TextColor3 = T.Text
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

    local TabBar = Instance.new("Frame", MainFrame)
    TabBar.Size = UDim2.new(0, 95, 1, -36)
    TabBar.Position = UDim2.new(0, 0, 0, 36)
    TabBar.BackgroundColor3 = T.Secondary

    local TabList = Instance.new("ScrollingFrame", TabBar)
    TabList.Size = UDim2.new(1, 0, 1, 0)
    TabList.BackgroundTransparency = 1
    TabList.ScrollBarThickness = 0
    local list = Instance.new("UIListLayout", TabList)
    list.Padding = UDim.new(0, 4)

    local ContentArea = Instance.new("Frame", MainFrame)
    ContentArea.Size = UDim2.new(1, -96, 1, -36)
    ContentArea.Position = UDim2.new(0, 96, 0, 36)
    ContentArea.BackgroundTransparency = 1

    local WindowObj = {}
    local ActiveTab = nil

    function WindowObj:MakeTab(name)
        local TabBtn = Instance.new("TextButton", TabList)
        TabBtn.Size = UDim2.new(1, -10, 0, 30)
        TabBtn.BackgroundColor3 = T.Element
        TabBtn.Text = name
        TabBtn.TextColor3 = T.SubText
        TabBtn.Font = Enum.Font.GothamBold
        Corner(TabBtn, 6)

        local Page = Instance.new("ScrollingFrame", ContentArea)
        Page.Size = UDim2.new(1, 0, 1, 0)
        Page.Visible = false
        Page.BackgroundTransparency = 1
        Page.ScrollBarThickness = 2
        local pageList = Instance.new("UIListLayout", Page)
        pageList.Padding = UDim.new(0, 5)

        TabBtn.MouseButton1Click:Connect(function()
            if ActiveTab then
                ActiveTab.Page.Visible = false
                ActiveTab.Btn.BackgroundColor3 = T.Element
            end
            Page.Visible = true
            TabBtn.BackgroundColor3 = T.Accent
            ActiveTab = {Page = Page, Btn = TabBtn}
        end)

        local TabFuncs = {}
        
        function TabFuncs:MakeButton(text, callback)
            local Btn = Instance.new("TextButton", Page)
            Btn.Size = UDim2.new(1, -10, 0, 34)
            Btn.BackgroundColor3 = T.Accent
            Btn.Text = text
            Btn.Font = Enum.Font.GothamBold
            Corner(Btn, 6)
            Btn.MouseButton1Click:Connect(callback)
        end

        return TabFuncs
    end

    return WindowObj
end

-- ================================================
-- EXEMPLO DE USO:
-- ================================================

local Win = Library:MakeWindow({Title = "MEU MENU", Size = {Width = 400, Height = 300}})
local Tab1 = Win:MakeTab("Início")

Tab1:MakeButton("Clique Aqui", function()
    print("Botão clicado!")
end)
