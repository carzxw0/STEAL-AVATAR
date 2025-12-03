-- Serviços do Roblox
local Players = game:GetService("Players")
local InsertService = game:GetService("InsertService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

----------------------------------------------------------
-- SALVAR A APARÊNCIA ORIGINAL
----------------------------------------------------------
local originalAppearance = player.CharacterAppearanceId
local originalBodyColors = character:FindFirstChildOfClass("BodyColors")
local originalHumanoidDescription = Instance.new("HumanoidDescription")
originalHumanoidDescription.Parent = character

pcall(function()
    originalHumanoidDescription = Players:GetHumanoidDescriptionFromUserId(player.UserId)
end)

----------------------------------------------------------
-- GUI PROFISSIONAL
----------------------------------------------------------
local screenGui = Instance.new("ScreenGui", CoreGui)
screenGui.ResetOnSpawn = false

-- Caixa principal
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 300, 0, 180)
mainFrame.Position = UDim2.new(0.35, 0, 0.35, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0

local UICorner = Instance.new("UICorner", mainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

-- Título
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "Copiar Aparência"
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 20

-- Caixa de texto
local nicknameBox = Instance.new("TextBox", mainFrame)
nicknameBox.Size = UDim2.new(0.9, 0, 0, 40)
nicknameBox.Position = UDim2.new(0.05, 0, 0.30, 0)
nicknameBox.PlaceholderText = "Digite o Nick..."
nicknameBox.Text = ""
nicknameBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
nicknameBox.TextColor3 = Color3.new(1,1,1)
nicknameBox.Font = Enum.Font.Gotham
nicknameBox.TextSize = 18
Instance.new("UICorner", nicknameBox).CornerRadius = UDim.new(0, 8)

-- Botão Copiar
local copiarBtn = Instance.new("TextButton", mainFrame)
copiarBtn.Size = UDim2.new(0.9, 0, 0, 40)
copiarBtn.Position = UDim2.new(0.05, 0, 0.55, 0)
copiarBtn.Text = "Copiar"
copiarBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
copiarBtn.TextColor3 = Color3.new(1,1,1)
copiarBtn.Font = Enum.Font.GothamBold
copiarBtn.TextSize = 18
Instance.new("UICorner", copiarBtn).CornerRadius = UDim.new(0, 8)

-- Botão Restaurar
local restoreBtn = Instance.new("TextButton", mainFrame)
restoreBtn.Size = UDim2.new(0.9, 0, 0, 40)
restoreBtn.Position = UDim2.new(0.05, 0, 0.80, 0)
restoreBtn.Text = "Restaurar"
restoreBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
restoreBtn.TextColor3 = Color3.new(1,1,1)
restoreBtn.Font = Enum.Font.GothamBold
restoreBtn.TextSize = 18
Instance.new("UICorner", restoreBtn).CornerRadius = UDim.new(0, 8)

----------------------------------------------------------
-- FUNÇÃO: PEGAR A APARÊNCIA DE OUTRO PLAYER
----------------------------------------------------------
local function copiarAparencia(nick)
    local target = Players:FindFirstChild(nick)
    if not target then
        title.Text = "Jogador não encontrado!"
        title.TextColor3 = Color3.fromRGB(255, 80, 80)
        return
    end

    local humanoid = character:WaitForChild("Humanoid")

    -- Tentamos pegar a descrição do alvo
    local success, desc = pcall(function()
        return Players:GetHumanoidDescriptionFromUserId(target.UserId)
    end)

    if success and desc then
        humanoid:ApplyDescription(desc)
        title.Text = "Aparência copiada!"
        title.TextColor3 = Color3.fromRGB(50, 200, 50)
    else
        title.Text = "Erro ao copiar!"
        title.TextColor3 = Color3.fromRGB(255, 60, 60)
    end
end

----------------------------------------------------------
-- FUNÇÃO: RESTAURAR SUA APARÊNCIA ORIGINAL
----------------------------------------------------------
local function restaurarAparencia()
    local humanoid = character:WaitForChild("Humanoid")

    humanoid:ApplyDescription(originalHumanoidDescription)

    title.Text = "Aparência restaurada!"
    title.TextColor3 = Color3.fromRGB(100, 255, 100)
end

----------------------------------------------------------
-- BOTÕES
----------------------------------------------------------
copiarBtn.MouseButton1Click:Connect(function()
    copiarAparencia(nicknameBox.Text)
end)

restoreBtn.MouseButton1Click:Connect(function()
    restaurarAparencia()
end)
