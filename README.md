--[[ 
    FLING ON TOUCH (EMPURRÃO AO TOCAR)
    Como usar: Ative e simplesmente ande contra as pessoas.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

if coreGui:FindFirstChild("FlingGUI") then coreGui:FindFirstChild("FlingGUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FlingGUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local flingActive = false
local power = 150 -- Força do arremesso (Aumente se quiser mandar pra lua)

-- Interface Discreta
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 150, 0, 40)
frame.Position = UDim2.new(1, -170, 1, -110) -- Acima do botão da Ripa
frame.BackgroundColor3 = Color3.fromRGB(30, 0, 50)
frame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "Fling: OFF"
btn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Parent = frame

-- FUNÇÃO QUE APLICA O EMPURRÃO
local function applyFling(otherPart)
    if not flingActive then return end
    
    local character = player.Character
    if not character then return end
    
    local targetModel = otherPart.Parent
    local targetHumanoid = targetModel:FindFirstChildOfClass("Humanoid")
    
    -- Verifica se é um jogador e não você mesmo
    if targetHumanoid and targetModel ~= character then
        local hrp = targetModel:FindFirstChild("HumanoidRootPart")
        if hrp then
            -- Aplica uma força de velocidade instantânea
            local pushDir = (hrp.Position - character.HumanoidRootPart.Position).Unit
            hrp.Velocity = (pushDir + Vector3.new(0, 0.5, 0)) * power
        end
    end
end

-- Conecta o toque em todas as partes do seu corpo
player.CharacterAdded:Connect(function(char)
    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") then
            part.Touched:Connect(applyFling)
        end
    end
end)

-- Conexão inicial
if player.Character then
    for _, part in pairs(player.Character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Touched:Connect(applyFling)
        end
    end
end

btn.MouseButton1Click:Connect(function()
    flingActive = not flingActive
    btn.Text = flingActive and "Fling: ATIVO 🚀" or "Fling: OFF"
    btn.BackgroundColor3 = flingActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(60, 0, 100)
end)
