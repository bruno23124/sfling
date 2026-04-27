--[[ 
    KILL AURA BYPASS - 30 STUDS
    Usa interpolação de posição para forçar o dano real.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")
local runService = game:GetService("RunService")

if coreGui:FindFirstChild("AuraBypass_GUI") then coreGui:FindFirstChild("AuraBypass_GUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AuraBypass_GUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local active = false
local range = 30

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.7, 0)
btn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
btn.Text = "Aura Bypass: OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 14
btn.Parent = screenGui
btn.Draggable = true

-- Função de Dano por Interpolação
runService.RenderStepped:Connect(function()
    if active then
        pcall(function()
            local char = player.Character
            local tool = char and char:FindFirstChildOfClass("Tool")
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            
            if tool and hrp then
                for _, v in pairs(game.Players:GetPlayers()) do
                    if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        local targetHRP = v.Character.HumanoidRootPart
                        local dist = (hrp.Position - targetHRP.Position).Magnitude
                        
                        if dist <= range and v.Character.Humanoid.Health > 0 then
                            -- Força o ataque
                            tool:Activate()
                            
                            -- Teleporta a ferramenta pro alvo e volta (Invisível)
                            local oldCFrame = tool.Handle.CFrame
                            tool.Handle.CFrame = targetHRP.CFrame
                            task.wait() -- Delay mínimo
                            tool.Handle.CFrame = oldCFrame
                        end
                    end
                end
            end
        end)
    end
end)

btn.MouseButton1Click:Connect(function()
    active = not active
    btn.Text = active and "BYPASS: ATIVO ⚔️" or "Aura Bypass: OFF"
    btn.BackgroundColor3 = active and Color3.fromRGB(150, 0, 0) or Color3.fromRGB(20, 20, 20)
end)
