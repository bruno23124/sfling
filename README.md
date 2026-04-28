--[[ 
    GHOST KILLER V12 - SILENT MASS KILL
    Bypass: Cosmo$, VB Anti-Cheat & LogService
]]

local player = game.Players.LocalPlayer
local players = game:GetService("Players")
local runService = game:GetService("RunService")
local replicatedStorage = game:GetService("ReplicatedStorage")

-- 1. CLOAK DE IMUNIDADE (Engana o Punish do Cosmo)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = player

-- Variáveis de controle
local killAuraActive = false
local range = 50 -- Distância máxima

-- Criar ScreenGui Silenciosa (Nomes aleatórios para evitar scanners de UI)
local screenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
screenGui.Name = "DataModule_" .. math.random(100, 999)

local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 200, 0, 60)
mainFrame.Position = UDim2.new(1, -220, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0

local uiCorner = Instance.new("UICorner", mainFrame)

local killButton = Instance.new("TextButton", mainFrame)
killButton.Size = UDim2.new(0, 180, 0, 40)
killButton.Position = UDim2.new(0.5, -90, 0.5, -20)
killButton.Text = "MODO SHADOW: OFF"
killButton.TextColor3 = Color3.fromRGB(255, 255, 255)
killButton.Font = Enum.Font.GothamBold
killButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Instance.new("UICorner", killButton)

-- 2. FUNÇÃO PARA PEGAR O TOKEN (Bypass do VB Anti-Cheat)
local function getAuthToken()
    for _, v in pairs(replicatedStorage:GetChildren()) do
        if v:IsA("RemoteFunction") and v:GetAttribute("oyvey") then
            return v:InvokeServer("THUG")
        end
    end
    return nil
end

-- 3. ATAQUE POR SOBREPOSIÇÃO FÍSICA (firetouchinterest)
-- Este método é o único que o Cosmo não consegue bloquear via Remote
local function attack(target)
    local char = player.Character
    local tool = char:FindFirstChildOfClass("Tool")
    
    if tool and tool:FindFirstChild("Handle") and target:FindFirstChild("HumanoidRootPart") then
        -- Simula o toque da Ripa no alvo
        firetouchinterest(target.HumanoidRootPart, tool.Handle, 0)
        firetouchinterest(target.HumanoidRootPart, tool.Handle, 1)
        
        -- Opcional: Ativa a ferramenta se necessário
        tool:Activate()
    end
end

-- 4. LOOP PRINCIPAL (Silent & Fast)
runService.Heartbeat:Connect(function()
    if killAuraActive then
        pcall(function()
            for _, p in pairs(players:GetPlayers()) do
                if p ~= player and p.Character and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
                    local dist = (player.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude
                    if dist <= range then
                        attack(p.Character)
                    end
                end
            end
        end)
    end
end)

-- Alternar Estado
killButton.MouseButton1Click:Connect(function()
    killAuraActive = not killAuraActive
    if killAuraActive then
        killButton.Text = "SHADOW ACTIVE 💀"
        killButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
        -- Tenta pegar o token ao ativar
        getAuthToken() 
    else
        killButton.Text = "MODO SHADOW: OFF"
        killButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    end
end)

-- Tecla de Atalho (K)
game:GetService("UserInputService").InputBegan:Connect(function(input, chat)
    if not chat and input.KeyCode == Enum.KeyCode.K then
        killButton:Click()
    end
end)
