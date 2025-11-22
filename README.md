-- Animal Simulator GUI - Painel Compacto
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- Criar a GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "LR TA 2🚩"
ScreenGui.Parent = player:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Frame principal (MENOR)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 300, 0, 350) -- Tamanho reduzido
MainFrame.Position = UDim2.new(0.1, 0, 0.1, 0) -- Posição no canto
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

-- Arredondar cantos
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = MainFrame

-- URSO DE FUNDO
local BearBackground = Instance.new("ImageLabel")
BearBackground.Name = "BearBackground"
BearBackground.Size = UDim2.new(1, 0, 1, 0)
BearBackground.Position = UDim2.new(0, 0, 0, 0)
BearBackground.BackgroundTransparency = 1
BearBackground.Image = "rbxassetid://10153218425" -- ID de um urso popular no Roblox
BearBackground.ImageTransparency = 0.8 -- Muito transparente para não atrapalhar
BearBackground.ScaleType = Enum.ScaleType.Fit
BearBackground.TileSize = UDim2.new(0, 150, 0, 150)
BearBackground.Parent = MainFrame

-- Header (para arrastar - MAIOR para facilitar)
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 30)
Header.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Header.BorderSizePixel = 0
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 8)
HeaderCorner.Parent = Header

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "LR TA 2🚩"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 12 -- Texto menor
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

-- Botão minimizar
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
MinimizeButton.Position = UDim2.new(1, -55, 0.5, -12)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
MinimizeButton.BorderSizePixel = 0
MinimizeButton.Text = "_"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 14
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Parent = Header

local MinimizeCorner = Instance.new("UICorner")
MinimizeCorner.CornerRadius = UDim.new(0, 4)
MinimizeCorner.Parent = MinimizeButton

-- Botão fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -25, 0.5, -12)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseButton.BorderSizePixel = 0
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 12
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 4)
CloseCorner.Parent = CloseButton

-- Abas
local TabsContainer = Instance.new("Frame")
TabsContainer.Name = "TabsContainer"
TabsContainer.Size = UDim2.new(1, 0, 0, 30) -- Menor
TabsContainer.Position = UDim2.new(0, 0, 0, 30)
TabsContainer.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TabsContainer.BorderSizePixel = 0
TabsContainer.Parent = MainFrame

-- Conteúdo das abas
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -10, 1, -70) -- Ajustado para tamanho menor
ContentFrame.Position = UDim2.new(0, 5, 0, 65)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

-- Variáveis para controle de estado
local isMinimized = false
local originalSize = UDim2.new(0, 300, 0, 350)
local minimizedSize = UDim2.new(0, 300, 0, 30)
local originalPosition = MainFrame.Position

-- Criar sistema de abas
local tabs = {}
local currentTab = nil

local function createTab(name)
    local tabButton = Instance.new("TextButton")
    tabButton.Name = name .. "Tab"
    tabButton.Size = UDim2.new(0.33, -4, 1, -6)
    tabButton.Position = UDim2.new((#tabs * 0.33), (#tabs * 2), 0, 3)
    tabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    tabButton.BorderSizePixel = 0
    tabButton.Text = name
    tabButton.TextColor3 = Color3.fromRGB(200, 200, 200)
    tabButton.TextSize = 10 -- Texto menor
    tabButton.Font = Enum.Font.Gotham
    tabButton.Parent = TabsContainer
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 4)
    tabCorner.Parent = tabButton
    
    local tabContent = Instance.new("ScrollingFrame")
    tabContent.Name = name .. "Content"
    tabContent.Size = UDim2.new(1, 0, 1, 0)
    tabContent.Position = UDim2.new(0, 0, 0, 0)
    tabContent.BackgroundTransparency = 1
    tabContent.BorderSizePixel = 0
    tabContent.ScrollBarThickness = 3
    tabContent.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
    tabContent.Visible = false
    tabContent.Parent = ContentFrame
    
    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Padding = UDim.new(0, 5) -- Menor espaçamento
    UIListLayout.Parent = tabContent
    
    local tabData = {
        button = tabButton,
        content = tabContent,
        name = name
    }
    
    table.insert(tabs, tabData)
    
    tabButton.MouseButton1Click:Connect(function()
        if currentTab then
            currentTab.button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            currentTab.content.Visible = false
        end
        
        tabButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        tabContent.Visible = true
        currentTab = tabData
    end)
    
    if #tabs == 1 then
        tabButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        tabContent.Visible = true
        currentTab = tabData
    end
    
    return tabContent
end

-- Função para criar botões (MENORES)
local function createButton(parent, name, callback)
    local button = Instance.new("TextButton")
    button.Name = name .. "Button"
    button.Size = UDim2.new(1, 0, 0, 30) -- Botões menores
    button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    button.BorderSizePixel = 0
    button.Text = name
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 12 -- Texto menor
    button.Font = Enum.Font.Gotham
    button.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = button
    
    -- Efeito hover
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(55, 55, 55)}):Play()
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
    end)
    
    button.MouseButton1Click:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play()
        wait(0.1)
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
        
        callback()
    end)
    
    return button
end

-- Função para minimizar/restaurar
local function toggleMinimize()
    if isMinimized then
        -- Restaurar
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {
            Size = originalSize
        }):Play()
        
        -- Mostrar conteúdo
        TabsContainer.Visible = true
        ContentFrame.Visible = true
        
        MinimizeButton.Text = "_"
        MinimizeButton.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
    else
        -- Minimizar
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {
            Size = minimizedSize
        }):Play()
        
        -- Esconder conteúdo
        TabsContainer.Visible = false
        ContentFrame.Visible = false
        
        MinimizeButton.Text = "□"
        MinimizeButton.BackgroundColor3 = Color3.fromRGB(150, 150, 50)
    end
    
    isMinimized = not isMinimized
end

-- SISTEMA DE ARRASTAR CORRIGIDO
local dragging = false
local dragStart
local startPos

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
        
        local connection
        connection = input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
                connection:Disconnect()
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            startPos.X.Scale, 
            startPos.X.Offset + delta.X,
            startPos.Y.Scale, 
            startPos.Y.Offset + delta.Y
        )
    end
end)

-- Criar as abas
local animalTab = createTab("Animal")
local helperTab = createTab("Helper") 
local playerTab = createTab("Players")

-- Conteúdo da aba Animal Simulator
createButton(animalTab, "Farm Moedas", function()
    coin = true
    while true do
        wait(0.0)
        if coin == true then
            game:GetService("ReplicatedStorage").Events.CoinEvent:FireServer()
        end
    end
end)

createButton(animalTab, "Aumentar Hitbox", function()
    local hit = workspace.dom_gi.combatHandler.HitBox
    hit.Value = Vector3.new(300, 300, 300)  
    hit.Transparency = 0.5  
    hit.CanCollide = false
    hit.CanTouch = true
end)

createButton(animalTab, "Farm NPCs", function()
    while true do
        wait(0.1)
        local npcNames = {"BOSSBEAR", "Griffin"}
        
        for _, name in ipairs(npcNames) do
            local npc = workspace.NPC:FindFirstChild(name)
            if npc and npc:FindFirstChild("Humanoid") then
                game:GetService("ReplicatedStorage").jdskhfsIIIllliiIIIdchgdIiIIIlIlIli:FireServer(npc.Humanoid)
            end
        end
    end
end)

-- Conteúdo da aba Script Helper
createButton(helperTab, "Hitbox Expander", function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Universal-Hitbox-Expander-16568"))()
end)

createButton(helperTab, "Nameless Admin", function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Nameless-Admin-23304"))()
end)

createButton(helperTab, "Dex Explorer", function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Dex-Explorer-for-Mobile-32019"))()
end)

-- Conteúdo da aba Players
createButton(playerTab, "Rodiar Player", function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fe-Ilussion-67260"))()
end)

createButton(playerTab, "Invisível", function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Invisible-script-v2-64306"))()
end)
createButton(playerTab, "Eps Health", function()
   local View = workspace.dom_gi.Humanoid
while true do
wait(0)
 View.HealthDisplayDistance = 500
end
end)
createButton(playerTab, "fps +90", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Luizhck/Fps-booster/refs/heads/main/README.md"))()
end)
-- Função para fechar a GUI
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Função para minimizar
MinimizeButton.MouseButton1Click:Connect(function()
    toggleMinimize()
end)

-- Efeitos nos botões do header
MinimizeButton.MouseEnter:Connect(function()
    TweenService:Create(MinimizeButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 170, 70)}):Play()
end)

MinimizeButton.MouseLeave:Connect(function()
    if isMinimized then
        TweenService:Create(MinimizeButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(150, 150, 50)}):Play()
    else
        TweenService:Create(MinimizeButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(50, 150, 50)}):Play()
    end
end)

CloseButton.MouseEnter:Connect(function()
    TweenService:Create(CloseButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(220, 70, 70)}):Play()
end)

CloseButton.MouseLeave:Connect(function()
    TweenService:Create(CloseButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(200, 50, 50)}):Play()
end)

-- Atalho de teclado para minimizar (F1)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed then
        if input.KeyCode == Enum.KeyCode.F1 then
            toggleMinimize()
        elseif input.KeyCode == Enum.KeyCode.F2 then
            ScreenGui:Destroy()
        end
    end
end)

-- Ajustar o layout das abas
for _, tab in pairs(tabs) do
    tab.content.CanvasSize = UDim2.new(0, 0, 0, (#tab.content:GetChildren() - 1) * 35)
end

-- Notificação simples
wait(1)
print("lr GUI Carregada!")
print("F1: Minimizar | F2: Fechar")
