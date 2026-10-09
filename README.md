-- Services
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")

-- Destruir GUI anterior se existir
if CoreGui:FindFirstChild("GameBoyXPinturaGui") then
    CoreGui.GameBoyXPinturaGui:Destroy()
end

-- ScreenGui Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GameBoyXPinturaGui"
ScreenGui.ResetOnSpawn = false

if syn and syn.protect_gui then
    syn.protect_gui(ScreenGui)
    ScreenGui.Parent = CoreGui
elseif gethui then
    ScreenGui.Parent = gethui()
else
    ScreenGui.Parent = CoreGui
end

----------------------------------------------------
-- CANVAS OVERLAY DE PINTURA NA TELA DO JOGO
----------------------------------------------------
local ScreenCanvas = Instance.new("Frame")
ScreenCanvas.Name = "ScreenCanvas"
ScreenCanvas.Size = UDim2.new(1, 0, 1, 0)
ScreenCanvas.BackgroundTransparency = 1
ScreenCanvas.ClipsDescendants = true
ScreenCanvas.ZIndex = 5
ScreenCanvas.Parent = ScreenGui

----------------------------------------------------
-- BOTÃO FLUTUANTE (BOLINHA MOVÍVEL COM BORDA VERDE)
----------------------------------------------------
local ToggleButton = Instance.new("TextButton")
local UICornerBtn = Instance.new("UICorner")
local UIStrokeBtn = Instance.new("UIStroke")

ToggleButton.Name = "ToggleButton"
ToggleButton.Size = UDim2.new(0, 65, 0, 65)
ToggleButton.Position = UDim2.new(0.05, 0, 0.4, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
ToggleButton.Active = true
ToggleButton.Draggable = true
ToggleButton.Text = "Pintura"
ToggleButton.TextColor3 = Color3.fromRGB(85, 255, 0)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 15
ToggleButton.Parent = ScreenGui

UICornerBtn.CornerRadius = UDim.new(1, 0)
UICornerBtn.Parent = ToggleButton

-- Borda Verde Bonita e Forte na Bolinha
UIStrokeBtn.Color = Color3.fromRGB(85, 255, 0)
UIStrokeBtn.Thickness = 3
UIStrokeBtn.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
UIStrokeBtn.Parent = ToggleButton

----------------------------------------------------
-- PAINEL PRINCIPAL (GAMEBOYX PINTURA)
----------------------------------------------------
local MainFrame = Instance.new("Frame")
local UICornerMain = Instance.new("UICorner")
local UIStrokeMain = Instance.new("UIStroke")
local Title = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")

MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 360, 0, 260)
MainFrame.Position = UDim2.new(0.5, -180, 0.5, -130)
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
MainFrame.Visible = true
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

UICornerMain.CornerRadius = UDim.new(0, 16)
UICornerMain.Parent = MainFrame

UIStrokeMain.Color = Color3.fromRGB(85, 255, 0)
UIStrokeMain.Thickness = 2
UIStrokeMain.Parent = MainFrame

Title.Name = "Title"
Title.Size = UDim2.new(1, -50, 0, 45)
Title.Position = UDim2.new(0, 20, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "GameBoy<font color=\"rgb(85,255,0)\">X Pintura</font>"
Title.RichText = true
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 22
Title.Font = Enum.Font.SourceSansBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = MainFrame

CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 10)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 85, 85)
CloseButton.TextSize = 20
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.Parent = MainFrame

-- 1º Botão: Pintar em Aba
local OpenPaintBtn = Instance.new("TextButton")
OpenPaintBtn.Name = "OpenPaintBtn"
OpenPaintBtn.Size = UDim2.new(1, -30, 0, 45)
OpenPaintBtn.Position = UDim2.new(0, 15, 0, 60)
OpenPaintBtn.BackgroundColor3 = Color3.fromRGB(85, 255, 0)
OpenPaintBtn.Text = "🎨 Pintar na Aba"
OpenPaintBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
OpenPaintBtn.Font = Enum.Font.SourceSansBold
OpenPaintBtn.TextSize = 16
OpenPaintBtn.Parent = MainFrame
Instance.new("UICorner", OpenPaintBtn).CornerRadius = UDim.new(0, 8)

-- 2º Botão: Pintar na Tela do Jogo
local OpenScreenPaintBtn = Instance.new("TextButton")
OpenScreenPaintBtn.Name = "OpenScreenPaintBtn"
OpenScreenPaintBtn.Size = UDim2.new(1, -30, 0, 45)
OpenScreenPaintBtn.Position = UDim2.new(0, 15, 0, 115)
OpenScreenPaintBtn.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
OpenScreenPaintBtn.Text = "🖌️ Pintar na Tela do Jogo"
OpenScreenPaintBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
OpenScreenPaintBtn.Font = Enum.Font.SourceSansBold
OpenScreenPaintBtn.TextSize = 16
OpenScreenPaintBtn.Parent = MainFrame
Instance.new("UICorner", OpenScreenPaintBtn).CornerRadius = UDim.new(0, 8)

----------------------------------------------------
-- INTERFACE MOVÍVEL DE CORES PARA PINTAR NA TELA
----------------------------------------------------
local ScreenPaletteFrame = Instance.new("Frame")
ScreenPaletteFrame.Name = "ScreenPaletteFrame"
ScreenPaletteFrame.Size = UDim2.new(0, 340, 0, 120)
ScreenPaletteFrame.Position = UDim2.new(0.5, -170, 0.05, 0)
ScreenPaletteFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ScreenPaletteFrame.Visible = false
ScreenPaletteFrame.Active = true
ScreenPaletteFrame.Draggable = true
ScreenPaletteFrame.ZIndex = 20
ScreenPaletteFrame.Parent = ScreenGui

Instance.new("UICorner", ScreenPaletteFrame).CornerRadius = UDim.new(0, 12)
local PaletteStroke = Instance.new("UIStroke")
PaletteStroke.Color = Color3.fromRGB(85, 255, 0)
PaletteStroke.Thickness = 2
PaletteStroke.Parent = ScreenPaletteFrame

local PaletteTitle = Instance.new("TextLabel")
PaletteTitle.Size = UDim2.new(1, -80, 0, 30)
PaletteTitle.Position = UDim2.new(0, 10, 0, 0)
PaletteTitle.BackgroundTransparency = 1
PaletteTitle.Text = "Pintando na Tela"
PaletteTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
PaletteTitle.Font = Enum.Font.SourceSansBold
PaletteTitle.TextSize = 14
PaletteTitle.TextXAlignment = Enum.TextXAlignment.Left
PaletteTitle.ZIndex = 21
PaletteTitle.Parent = ScreenPaletteFrame

local ClearScreenBtn = Instance.new("TextButton")
ClearScreenBtn.Size = UDim2.new(0, 30, 0, 30)
ClearScreenBtn.Position = UDim2.new(1, -70, 0, 2)
ClearScreenBtn.BackgroundTransparency = 1
ClearScreenBtn.Text = "🗑️"
ClearScreenBtn.TextSize = 16
ClearScreenBtn.ZIndex = 21
ClearScreenBtn.Parent = ScreenPaletteFrame

local CloseScreenPaintBtn = Instance.new("TextButton")
CloseScreenPaintBtn.Size = UDim2.new(0, 30, 0, 30)
CloseScreenPaintBtn.Position = UDim2.new(1, -35, 0, 2)
CloseScreenPaintBtn.BackgroundTransparency = 1
CloseScreenPaintBtn.Text = "X"
CloseScreenPaintBtn.TextColor3 = Color3.fromRGB(255, 85, 85)
CloseScreenPaintBtn.Font = Enum.Font.SourceSansBold
CloseScreenPaintBtn.TextSize = 18
CloseScreenPaintBtn.ZIndex = 21
CloseScreenPaintBtn.Parent = ScreenPaletteFrame

local PaletteScroll = Instance.new("ScrollingFrame")
PaletteScroll.Size = UDim2.new(1, -20, 0, 75)
PaletteScroll.Position = UDim2.new(0, 10, 0, 35)
PaletteScroll.BackgroundTransparency = 1
PaletteScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
PaletteScroll.ScrollBarThickness = 4
PaletteScroll.ZIndex = 21
PaletteScroll.Parent = ScreenPaletteFrame

local PaletteGrid = Instance.new("UIGridLayout")
PaletteGrid.CellSize = UDim2.new(0, 92, 0, 28)
PaletteGrid.CellPadding = UDim2.new(0, 5, 0, 5)
PaletteGrid.Parent = PaletteScroll

----------------------------------------------------
-- ABA DE DESENHO EM QUADRO BRANCO
----------------------------------------------------
local PaintWindow = Instance.new("Frame")
PaintWindow.Name = "PaintWindow"
PaintWindow.Size = UDim2.new(0, 420, 0, 450)
PaintWindow.Position = UDim2.new(0.5, -210, 0.5, -225)
PaintWindow.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
PaintWindow.Visible = false
PaintWindow.Active = true
PaintWindow.Draggable = true
PaintWindow.Parent = ScreenGui

Instance.new("UICorner", PaintWindow).CornerRadius = UDim.new(0, 14)
local WindowStroke = Instance.new("UIStroke")
WindowStroke.Color = Color3.fromRGB(85, 255, 0)
WindowStroke.Thickness = 2
WindowStroke.Parent = PaintWindow

local PaintTitle = Instance.new("TextLabel")
PaintTitle.Size = UDim2.new(1, -80, 0, 35)
PaintTitle.Position = UDim2.new(0, 15, 0, 0)
PaintTitle.BackgroundTransparency = 1
PaintTitle.Text = "Tela de Desenho"
PaintTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
PaintTitle.Font = Enum.Font.SourceSansBold
PaintTitle.TextSize = 16
PaintTitle.TextXAlignment = Enum.TextXAlignment.Left
PaintTitle.Parent = PaintWindow

local ClearBtn = Instance.new("TextButton")
ClearBtn.Size = UDim2.new(0, 30, 0, 30)
ClearBtn.Position = UDim2.new(1, -70, 0, 3)
ClearBtn.BackgroundTransparency = 1
ClearBtn.Text = "🗑️"
ClearBtn.TextSize = 18
ClearBtn.Parent = PaintWindow

local ClosePaintBtn = Instance.new("TextButton")
ClosePaintBtn.Size = UDim2.new(0, 30, 0, 30)
ClosePaintBtn.Position = UDim2.new(1, -35, 0, 3)
ClosePaintBtn.BackgroundTransparency = 1
ClosePaintBtn.Text = "X"
ClosePaintBtn.TextColor3 = Color3.fromRGB(255, 85, 85)
ClosePaintBtn.Font = Enum.Font.SourceSansBold
ClosePaintBtn.TextSize = 18
ClosePaintBtn.Parent = PaintWindow

local Canvas = Instance.new("Frame")
Canvas.Name = "Canvas"
Canvas.Size = UDim2.new(1, -20, 0, 280)
Canvas.Position = UDim2.new(0, 10, 0, 38)
Canvas.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Canvas.ClipsDescendants = true
Canvas.Active = true
Canvas.Parent = PaintWindow
Instance.new("UICorner", Canvas).CornerRadius = UDim.new(0, 8)

local ColorScroll = Instance.new("ScrollingFrame")
ColorScroll.Size = UDim2.new(1, -20, 0, 110)
ColorScroll.Position = UDim2.new(0, 10, 0, 328)
ColorScroll.BackgroundTransparency = 1
ColorScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
ColorScroll.ScrollBarThickness = 4
ColorScroll.Parent = PaintWindow

local UIGrid = Instance.new("UIGridLayout")
UIGrid.CellSize = UDim2.new(0, 92, 0, 30)
UIGrid.CellPadding = UDim2.new(0, 5, 0, 5)
UIGrid.Parent = ColorScroll

----------------------------------------------------
-- LISTA DE CORES E LÓGICA DE PINTURA
----------------------------------------------------
local selectedColor = Color3.fromRGB(0, 0, 0)
local isDrawingTab = false
local isDrawingScreen = false
local isScreenModeActive = false
local lastPointTab = nil
local lastPointScreen = nil

local colorsList = {
    {name = "🤍 Branco", color = Color3.fromRGB(255, 255, 255)},
    {name = "🖤 Preto", color = Color3.fromRGB(0, 0, 0)},
    {name = "💚 Verde-esmeralda", color = Color3.fromRGB(8, 143, 78)},
    {name = "💛 Dourado", color = Color3.fromRGB(212, 175, 55)},
    {name = "🩵 Azul-celeste", color = Color3.fromRGB(135, 206, 235)},
    {name = "🩶 Cinza", color = Color3.fromRGB(128, 128, 128)},
    {name = "💗 Rosa", color = Color3.fromRGB(255, 105, 180)},
    {name = "💙 Azul-marinho", color = Color3.fromRGB(0, 0, 128)},
    {name = "💚 Verde-neon", color = Color3.fromRGB(57, 255, 20)},
    {name = "🧡 Laranja", color = Color3.fromRGB(255, 140, 0)},
    {name = "💜 Violeta", color = Color3.fromRGB(138, 43, 226)},
    {name = "🤎 Marrom", color = Color3.fromRGB(139, 69, 19)},
    {name = "❤️ Bordô", color = Color3.fromRGB(128, 0, 32)},
    {name = "🩵 Ciano", color = Color3.fromRGB(0, 255, 255)},
    {name = "💛 Amarelo", color = Color3.fromRGB(255, 225, 0)},
    {name = "🌸 Rosa-claro", color = Color3.fromRGB(255, 182, 193)},
    {name = "🌊 Turquesa", color = Color3.fromRGB(64, 224, 208)},
    {name = "🔴 Vermelho", color = Color3.fromRGB(255, 0, 0)},
    {name = "🍏 Verde-lima", color = Color3.fromRGB(50, 205, 50)},
    {name = "🫐 Violeta-escuro", color = Color3.fromRGB(75, 0, 130)},
}

for _, item in ipairs(colorsList) do
    local ColorBtn = Instance.new("TextButton")
    ColorBtn.Size = UDim2.new(0, 92, 0, 30)
    ColorBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    ColorBtn.Text = item.name
    ColorBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    ColorBtn.Font = Enum.Font.SourceSansBold
    ColorBtn.TextSize = 11
    ColorBtn.TextTruncate = Enum.TextTruncate.AtEnd
    ColorBtn.Parent = ColorScroll
    Instance.new("UICorner", ColorBtn).CornerRadius = UDim.new(0, 6)
    ColorBtn.MouseButton1Click:Connect(function() selectedColor = item.color end)

    local ScreenColorBtn = Instance.new("TextButton")
    ScreenColorBtn.Size = UDim2.new(0, 92, 0, 28)
    ScreenColorBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    ScreenColorBtn.Text = item.name
    ScreenColorBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    ScreenColorBtn.Font = Enum.Font.SourceSansBold
    ScreenColorBtn.TextSize = 11
    ScreenColorBtn.TextTruncate = Enum.TextTruncate.AtEnd
    ScreenColorBtn.ZIndex = 22
    ScreenColorBtn.Parent = PaletteScroll
    Instance.new("UICorner", ScreenColorBtn).CornerRadius = UDim.new(0, 6)
    ScreenColorBtn.MouseButton1Click:Connect(function() selectedColor = item.color end)
end

ColorScroll.CanvasSize = UDim2.new(0, 0, 0, math.ceil(#colorsList / 4) * 35)
PaletteScroll.CanvasSize = UDim2.new(0, 0, 0, math.ceil(#colorsList / 3) * 33)

local function drawLineCanvas(p1, p2)
    local canvasPos = Canvas.AbsolutePosition
    local canvasSize = Canvas.AbsoluteSize

    local relP1 = Vector2.new(p1.X - canvasPos.X, p1.Y - canvasPos.Y)
    local relP2 = Vector2.new(p2.X - canvasPos.X, p2.Y - canvasPos.Y)

    local distance = (relP2 - relP1).Magnitude
    local angle = math.atan2(relP2.Y - relP1.Y, relP2.X - relP1.X)

    local line = Instance.new("Frame")
    line.Size = UDim2.new(0, distance, 0, 6)
    line.Position = UDim2.new(0, relP1.X, 0, relP1.Y)
    line.Rotation = math.deg(angle)
    line.AnchorPoint = Vector2.new(0, 0.5)
    line.BackgroundColor3 = selectedColor
    line.BorderSizePixel = 0
    line.Parent = Canvas
    Instance.new("UICorner", line).CornerRadius = UDim.new(1, 0)
end

local function drawLineScreen(p1, p2)
    local distance = (p2 - p1).Magnitude
    local angle = math.atan2(p2.Y - p1.Y, p2.X - p1.X)

    local line = Instance.new("Frame")
    line.Size = UDim2.new(0, distance, 0, 6)
    line.Position = UDim2.new(0, p1.X, 0, p1.Y)
    line.Rotation = math.deg(angle)
    line.AnchorPoint = Vector2.new(0, 0.5)
    line.BackgroundColor3 = selectedColor
    line.BorderSizePixel = 0
    line.ZIndex = 6
    line.Parent = ScreenCanvas
    Instance.new("UICorner", line).CornerRadius = UDim.new(1, 0)
end

Canvas.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDrawingTab = true
        lastPointTab = Vector2.new(input.Position.X, input.Position.Y)
    end
end)

Canvas.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDrawingTab = false
        lastPointTab = nil
    end
end)

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if isScreenModeActive and not gameProcessed then
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            isDrawingScreen = true
            lastPointScreen = Vector2.new(input.Position.X, input.Position.Y)
        end
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        isDrawingScreen = false
        lastPointScreen = nil
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if isDrawingTab and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local currentPoint = Vector2.new(input.Position.X, input.Position.Y)
        if lastPointTab then drawLineCanvas(lastPointTab, currentPoint) end
        lastPointTab = currentPoint
    elseif isDrawingScreen and isScreenModeActive and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local currentPoint = Vector2.new(input.Position.X, input.Position.Y)
        if lastPointScreen then drawLineScreen(lastPointScreen, currentPoint) end
        lastPointScreen = currentPoint
    end
end)

ClearBtn.MouseButton1Click:Connect(function()
    for _, child in ipairs(Canvas:GetChildren()) do
        if child:IsA("Frame") then child:Destroy() end
    end
end)

ClearScreenBtn.MouseButton1Click:Connect(function()
    for _, child in ipairs(ScreenCanvas:GetChildren()) do
        if child:IsA("Frame") then child:Destroy() end
    end
end)

OpenPaintBtn.MouseButton1Click:Connect(function()
    PaintWindow.Visible = true
end)

OpenScreenPaintBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    ScreenPaletteFrame.Visible = true
    isScreenModeActive = true
end)

ClosePaintBtn.MouseButton1Click:Connect(function()
    PaintWindow.Visible = false
    isDrawingTab = false
    lastPointTab = nil
end)

CloseScreenPaintBtn.MouseButton1Click:Connect(function()
    ScreenPaletteFrame.Visible = false
    isScreenModeActive = false
    isDrawingScreen = false
    lastPointScreen = nil
    MainFrame.Visible = true
end)

ToggleButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)
