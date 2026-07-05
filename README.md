-- [[ MATRIX HUB v3.8 - MENU CIANO ]]
local Library = {}

-- Tema Ciano
local Themes = {
    Cyan = {
        Background = Color3.fromRGB(5, 5, 5),
        Accent = Color3.fromRGB(0, 255, 255),
        Element = Color3.fromRGB(15, 15, 15),
        Text = Color3.fromRGB(255, 255, 255),
        SubText = Color3.fromRGB(0, 200, 200)
    }
}

-- Funções Visuais
local function Corner(p, r) Instance.new("UICorner", p).CornerRadius = UDim.new(0, r or 8) end
local function Stroke(p, c) 
    local s = Instance.new("UIStroke", p)
    s.Color = c; s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border; s.Thickness = 1.2 
end

-- Criador de Janela
function Library:MakeWindow(title)
    local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
    local main = Instance.new("Frame", sg)
    main.Size = UDim2.new(0, 380, 0, 320)
    main.Position = UDim2.new(0.5, -190, 0.5, -160)
    main.BackgroundColor3 = Themes.Cyan.Background
    Corner(main, 10); Stroke(main, Themes.Cyan.Accent)

    local top = Instance.new("TextLabel", main)
    top.Size = UDim2.new(1, 0, 0, 40)
    top.Text = "  " .. title
    top.TextColor3 = Themes.Cyan.Accent
    top.Font = Enum.Font.GothamBold
    top.TextXAlignment = Enum.TextXAlignment.Left
    top.BackgroundTransparency = 1
    
    local content = Instance.new("ScrollingFrame", main)
    content.Size = UDim2.new(1, -20, 1, -60)
    content.Position = UDim2.new(0, 10, 0, 50)
    content.BackgroundTransparency = 1
    content.ScrollBarThickness = 0
    Instance.new("UIListLayout", content).Padding = UDim.new(0, 10)

    local obj = {}
    function obj:MakeSection(t)
        local l = Instance.new("TextLabel", content); l.Size = UDim2.new(1, 0, 0, 20); l.Text = "  "..t:upper(); l.TextColor3 = Themes.Cyan.SubText; l.Font = Enum.Font.GothamBold; l.TextSize = 11; l.TextXAlignment = Enum.TextXAlignment.Left; l.BackgroundTransparency = 1
    end
    function obj:MakeButton(n, cb)
        local b = Instance.new("TextButton", content); b.Size = UDim2.new(1, 0, 0, 40); b.BackgroundColor3 = Themes.Cyan.Element; b.Text = n; b.TextColor3 = Themes.Cyan.Accent; b.Font = Enum.Font.GothamBold; Corner(b, 8); Stroke(b, Themes.Cyan.Accent)
        b.MouseButton1Click:Connect(cb)
    end
    return obj
end

-- EXECUTANDO O MENU
local Win = Library:MakeWindow("MATRIX HUB | REDZ STYLE")
Win:MakeSection("Auxiliar Nível")

Win:MakeButton("Auto Farme", function()
    -- Este botão carrega o SEILA7 que contém o código original
    loadstring(game:HttpGet("https://raw.githubusercontent.com/iagostariasousodeia-del/Seila7/refs/heads/main/README.md"))()
end)

print("MENU MATRIX CARREGADO!")
