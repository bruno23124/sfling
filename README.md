--[[ 
    ULTRA REACH SCRIPT - 60 STUDS (Baseado no seu modelo)
    Aumenta a área de acerto da sua Ripa massivamente.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

if coreGui:FindFirstChild("ReachGUI") then coreGui:FindFirstChild("ReachGUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ReachGUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local reachActive = false
local reachSize = Vector3.new(60, 60, 60) -- O alcance de 60 que você pediu

-- Interface Discreta
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 160, 0, 40)
mainFrame.Position = UDim2.new(1, -180, 1, -60)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "Alcance: OFF"
btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Parent = mainFrame

-- LOOP DO REACH
task.spawn(function()
    while true do
        task.wait(0.5)
        if reachActive then
            pcall(function()
                local char = player.Character
                if char then
                    -- Pega a Ripa ou qualquer arma na mão
                    local tool = char:FindFirstChildOfClass("Tool")
                    
                    if tool and tool:FindFirstChild("Handle") then
                        -- Aplica o tamanho de 60 studs
                        tool.Handle.Size = reachSize
                        
                        -- Configurações para ficar invisível e não bugar o personagem
                        tool.Handle.Transparency = 1 
                        tool.Handle.Massless = true
                        tool.Handle.CanCollide = false
                    end
                end
            end)
        else
            -- Reseta o tamanho caso você desligue o botão
            pcall(function()
                local tool = player.Character:FindFirstChildOfClass("Tool")
                if tool and tool:FindFirstChild("Handle") then
                    tool.Handle.Size = Vector3.new(1, 5, 1) -- Tamanho padrão aproximado
                    tool.Handle.Transparency = 0
                end
            end)
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    reachActive = not reachActive
    btn.Text = reachActive and "REACH 60: ATIVO" or "Alcance: OFF"
    btn.BackgroundColor3 = reachActive and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(50, 50, 50)
end)
