--[[ 
    ULTIMATE SPIN FLING (REPOSITORIO SFLING)
    Como usar: Ative e encoste nas pessoas.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

if coreGui:FindFirstChild("FlingGUI") then coreGui:FindFirstChild("FlingGUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FlingGUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local flingActive = false

-- Interface
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 150, 0, 40)
frame.Position = UDim2.new(1, -170, 1, -110)
frame.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
frame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "Fling (Spin): OFF"
btn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Parent = frame

-- LOGICA DO SPIN (Gira o RootPart para criar força centrífuga)
task.spawn(function()
    while true do
        task.wait()
        if flingActive then
            local char = player.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if hrp then
                -- Cria uma velocidade angular absurda
                local velocity = hrp:FindFirstChild("FlingVelocity") or Instance.new("AngularVelocity")
                velocity.Name = "FlingVelocity"
                velocity.Parent = hrp
                velocity.MaxTorque = 999999999
                velocity.AngularVelocity = Vector3.new(0, 999999999, 0) -- Gira no eixo Y
                velocity.Attachment0 = hrp:FindFirstChild("RootAttachment") or Instance.new("Attachment", hrp)
                
                -- Remove a colisão interna para não te travar
                for _, part in pairs(char:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        else
            -- Remove o giro ao desligar
            local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if hrp and hrp:FindFirstChild("FlingVelocity") then
                hrp.FlingVelocity:Destroy()
            end
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    flingActive = not flingActive
    btn.Text = flingActive and "Fling: GIRANDO 🌀" or "Fling (Spin): OFF"
    btn.BackgroundColor3 = flingActive and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(100, 0, 0)
end)
