--[[ 
    KILL AURA GHOST V27 - SILENT KILLER
    Bypass: Magnitude Check & VB Anti-Cheat
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- 1. CONFIGURAÇÕES
_G.AuraActive = false
local range = 50 -- Distância do ataque (não coloque muito alto para não dar kick)

-- 2. FUNÇÃO DE ATAQUE SILENCIOSO
local function attack()
    local char = localPlayer.Character
    local tool = char and char:FindFirstChildOfClass("Tool")
    
    if tool and tool:FindFirstChild("Handle") then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= localPlayer and p.Character and p.Character:FindFirstChild("Head") then
                local targetPart = p.Character.Head
                local distance = (localPlayer.Character.HumanoidRootPart.Position - targetPart.Position).Magnitude
                
                -- Só ataca se estiver no range para evitar detecção de teleporte de arma
                if distance <= range then
                    -- Simula o toque da arma no alvo
                    firetouchinterest(targetPart, tool.Handle, 0)
                    firetouchinterest(targetPart, tool.Handle, 1)
                end
            end
        end
    end
end

-- 3. LOOP DE EXECUÇÃO
task.spawn(function()
    while true do
        task.wait(0.1) -- Delay para não lagar o seu PC
        if _G.AuraActive then
            pcall(attack)
        end
    end
end)

-- 4. INTERFACE DE CONTROLE (ON/OFF)
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "AuraSystem"

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.05, 0, 0.4, 0)
btn.Text = "KILL AURA: OFF"
btn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 20

btn.MouseButton1Click:Connect(function()
    _G.AuraActive = not _G.AuraActive
    btn.Text = _G.AuraActive and "KILL AURA: ON" or "KILL AURA: OFF"
    btn.BackgroundColor3 = _G.AuraActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(150, 0, 0)
end)

print("✅ Kill Aura Ghost V27 Carregada! Use a Ripa.")
