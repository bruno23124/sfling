--[[ 
    KILL AURA DISCRETA - 30 STUDS
    Equilíbrio perfeito entre alcance e segurança.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

if coreGui:FindFirstChild("KillAura_V30") then coreGui:FindFirstChild("KillAura_V30"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "KillAura_V30"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local auraActive = false
local auraRange = 30 -- Ajustado para 30 studs conforme pedido

-- Botão de Interface
local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.7, 0)
btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
btn.Text = "Aura (30): OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 16
btn.Parent = screenGui
btn.Draggable = true

-- LOOP DO KILL AURA
task.spawn(function()
    while true do
        task.wait(0.1) -- Delay leve para não travar seu PC com 10k de ping
        if auraActive then
            pcall(function()
                local char = player.Character
                local tool = char and char:FindFirstChildOfClass("Tool")
                
                if tool and char:FindFirstChild("HumanoidRootPart") then
                    for _, v in pairs(game.Players:GetPlayers()) do
                        if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
                            local targetPart = v.Character:FindFirstChild("HumanoidRootPart")
                            if targetPart then
                                local mag = (char.HumanoidRootPart.Position - targetPart.Position).Magnitude
                                
                                if mag <= auraRange then
                                    -- Ativa a Ripa e força o dano no alvo
                                    tool:Activate()
                                    firetouchinterest(targetPart, tool.Handle, 0)
                                    firetouchinterest(targetPart, tool.Handle, 1)
                                end
                            end
                        end
                    end
                end
            end)
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    auraActive = not auraActive
    if auraActive then
        btn.Text = "AURA 30: ATIVA 🛡️"
        btn.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
    else
        btn.Text = "Aura (30): OFF"
        btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    end
end)
