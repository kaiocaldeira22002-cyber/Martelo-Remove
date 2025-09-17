-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Criando GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MarteloMenu"
ScreenGui.Parent = PlayerGui

-- Mini menu quadrado
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 120, 0, 100) -- quadrado
MainFrame.Position = UDim2.new(0.5, -60, 0.5, -50)
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255) -- fundo branco
MainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0) -- borda preta
MainFrame.BorderSizePixel = 6
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true

-- UICorner removido para manter o quadrado
-- se quiser cantos levemente arredondados, pode deixar um valor pequeno

-- Título pequeno no topo
local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(0, 120, 0, 20)
TitleLabel.Position = UDim2.new(0, 0, 0, 5)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "Blessed"
TitleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
TitleLabel.Font = Enum.Font.SourceSansBold
TitleLabel.TextScaled = true
TitleLabel.Parent = MainFrame

-- Botão Martelo
local MarteloButton = Instance.new("TextButton")
MarteloButton.Size = UDim2.new(0, 100, 0, 40)
MarteloButton.Position = UDim2.new(0, 10, 0, 40)
MarteloButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
MarteloButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
MarteloButton.BorderSizePixel = 4
MarteloButton.TextColor3 = Color3.fromRGB(0, 0, 0)
MarteloButton.Text = "Martelo"
MarteloButton.Font = Enum.Font.SourceSans
MarteloButton.TextScaled = true
MarteloButton.Parent = MainFrame

-- Função para remover item
local function RemoveItemByName(itemName)
    for _, t in pairs(LocalPlayer.Backpack:GetChildren()) do
        if t.Name == itemName then t:Destroy() end
    end
end

-- Toggle do martelo
local hasHammer = false
MarteloButton.MouseButton1Click:Connect(function()
    if not hasHammer then
        RemoveItemByName("Martelo")
        local Tool = Instance.new("Tool")
        Tool.RequiresHandle = false
        Tool.Name = "Martelo"
        Tool.Parent = LocalPlayer.Backpack

        Tool.Activated:Connect(function()
            local mouse = LocalPlayer:GetMouse()
            if mouse.Target then
                mouse.Target:Destroy()
            end
        end)

        hasHammer = true
        MarteloButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- verde
    else
        RemoveItemByName("Martelo")
        hasHammer = false
        MarteloButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255) -- branco
    end
end)

-- Toggle menu com K
local menuVisible = true
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.K then
        menuVisible = not menuVisible
        MainFrame.Visible = menuVisible
    end
end)
