--[[ 
    GHOST KILL AURA (AUTO-TP + INVISIBILIDADE)
    Teleporta, mata e pula para o próximo alvo.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")
local runService = game:GetService("RunService")

if coreGui:FindFirstChild("GhostAura_GUI") then coreGui:FindFirstChild("GhostAura_GUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GhostAura_GUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local ghostActive = false
local range = 100 -- Raio de busca para o próximo alvo

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 160, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.65, 0)
btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
btn.Text = "Ghost Aura: OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 14
btn.Parent = screenGui
btn.Draggable = true

-- Função para ficar "Invisível" (Manda seu corpo para baixo do mapa pro servidor)
local function setGhostMode(state)
    local char = player.Character
    if char and char:FindFirstChild("LowerTorso") then
        if state then
            -- Esconde o personagem (Localmente e tenta bugar a visão dos outros)
            for _, v in pairs(char:GetChildren()) do
                if v:IsA("BasePart") or v:IsA("Accessory") then
                    v.Transparency = 0.5 -- Para você ainda se ver um pouco
                end
            end
        else
            for _, v in pairs(char:GetChildren()) do
                if v:IsA("BasePart") or v:IsA("Accessory") then
                    v.Transparency = 0
                end
            end
        end
    end
end

-- LOOP DE CAÇA
task.spawn(function()
    while true do
        task.wait(0.1)
        if ghostActive then
            pcall(function()
                local char = player.Character
                local tool = char and char:FindFirstChildOfClass("Tool")
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                
                if tool and hrp then
                    -- Busca o alvo mais próximo
                    local closest = nil
                    local shortestDist = range
                    
                    for _, v in pairs(game.Players:GetPlayers()) do
                        if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
                            local targetHRP = v.Character:FindFirstChild("HumanoidRootPart")
                            if targetHRP then
                                local dist = (hrp.Position - targetHRP.Position).Magnitude
                                if dist < shortestDist then
                                    closest = v.Character
                                    shortestDist = dist
                                end
                            end
                        end
                    end
                    
                    -- Se achou alguém, teleporta e mata
                    if closest then
                        local targetHRP = closest.HumanoidRootPart
                        -- Fica um pouco atrás e acima do alvo para não bugar
                        hrp.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 2)
                        
                        -- Ataca
                        tool:Activate()
                        firetouchinterest(targetHRP, tool.Handle, 0)
                        firetouchinterest(targetHRP, tool.Handle, 1)
                    end
                end
            end)
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    ghostActive = not ghostActive
    setGhostMode(ghostActive)
    btn.Text = ghostActive and "GHOST MODE: ATIVO 👻" or "Ghost Aura: OFF"
    btn.BackgroundColor3 = ghostActive and Color3.fromRGB(80, 0, 150) or Color3.fromRGB(0, 0, 0)
end)
