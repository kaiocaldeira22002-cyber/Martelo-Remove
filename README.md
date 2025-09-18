-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Função para criar o menu
local function createMenu()
    local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

    -- Remove GUI antiga se existir
    if PlayerGui:FindFirstChild("FerramentasMenu") then
        PlayerGui.FerramentasMenu:Destroy()
    end

    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "FerramentasMenu"
    ScreenGui.Parent = PlayerGui

    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 120, 0, 120)
    MainFrame.Position = UDim2.new(0.5, -60, 0.5, -60)
    MainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    MainFrame.BorderColor3 = Color3.fromRGB(0, 0, 0)
    MainFrame.BorderSizePixel = 6
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Parent = ScreenGui

    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Size = UDim2.new(0, 120, 0, 20)
    TitleLabel.Position = UDim2.new(0, 0, 0, 5)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = "Ferramentas"
    TitleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
    TitleLabel.Font = Enum.Font.SourceSansBold
    TitleLabel.TextScaled = true
    TitleLabel.Parent = MainFrame

    local function createButton(text, yPos)
        local button = Instance.new("TextButton")
        button.Size = UDim2.new(0, 100, 0, 40)
        button.Position = UDim2.new(0, 10, 0, yPos)
        button.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        button.BorderColor3 = Color3.fromRGB(0, 0, 0)
        button.BorderSizePixel = 1
        button.TextColor3 = Color3.fromRGB(0, 0, 0)
        button.Text = text
        button.Font = Enum.Font.SourceSans
        button.TextScaled = true
        button.Parent = MainFrame
        return button
    end

    -- Martelo
    local hasHammer = false
    local MarteloButton = createButton("Martelo", 30)
    MarteloButton.MouseButton1Click:Connect(function()
        if not hasHammer then
            for _, t in pairs(LocalPlayer.Backpack:GetChildren()) do
                if t.Name == "Martelo" then t:Destroy() end
            end

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
            MarteloButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        else
            for _, t in pairs(LocalPlayer.Backpack:GetChildren()) do
                if t.Name == "Martelo" then t:Destroy() end
            end
            hasHammer = false
            MarteloButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        end
    end)

    -- TpTool
    local TpToolActive = false
    local TpToolButton = createButton("TpTool", 70)

    TpToolButton.MouseButton1Click:Connect(function()
        TpToolActive = not TpToolActive
        TpToolButton.BackgroundColor3 = TpToolActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 255, 255)

        if TpToolActive then
            for _, t in pairs(LocalPlayer.Backpack:GetChildren()) do
                if t.Name == "TpTool" then t:Destroy() end
            end

            local Tool = Instance.new("Tool")
            Tool.RequiresHandle = false
            Tool.Name = "TpTool"
            Tool.Parent = LocalPlayer.Backpack

            Tool.Activated:Connect(function()
                local mouse = LocalPlayer:GetMouse()
                if mouse.Target and mouse.Hit.Position.Y < 500 then
                    local character = LocalPlayer.Character
                    if character and character:FindFirstChild("HumanoidRootPart") then
                        character.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.Position + Vector3.new(0,3,0))
                    end
                end
            end)
        else
            for _, t in pairs(LocalPlayer.Backpack:GetChildren()) do
                if t.Name == "TpTool" then t:Destroy() end
            end
        end
    end)

    -- Toggle K
    local menuVisible = true
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.K then
            menuVisible = not menuVisible
            MainFrame.Visible = menuVisible
        end
    end)
end

-- Criar menu inicial
createMenu()

-- Reaplicar menu sempre que respawnar
LocalPlayer.CharacterAdded:Connect(function()
    wait(0.1)
    createMenu()
end)
