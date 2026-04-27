--[[ 
    ULTRA REACH 60 - POSIÇÃO CORRIGIDA
    O botão agora deve aparecer fixo no canto inferior direito.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

-- Limpa versões antigas para o botão não sumir ou duplicar
if coreGui:FindFirstChild("ReachGUI") then coreGui:FindFirstChild("ReachGUI"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ReachGUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local reachActive = false
local reachSize = Vector3.new(60, 60, 60)

-- Criando o Frame do Botão (Posição ajustada para não sumir)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 160, 0, 45)
mainFrame.Position = UDim2.new(1, -180, 1, -120) -- Subi um pouco a posição (Y)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 2
mainFrame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "REACH 60: OFF"
btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 14
btn.Parent = mainFrame

-- LOOP DO REACH 60
task.spawn(function()
    while true do
        task.wait(0.5)
        if reachActive then
            pcall(function()
                local char = player.Character
                if char then
                    local tool = char:FindFirstChildOfClass("Tool")
                    if tool and tool:FindFirstChild("Handle") then
                        tool.Handle.Size = reachSize
                        tool.Handle.Transparency = 1 
                        tool.Handle.Massless = true
                        tool.Handle.CanCollide = false
                    end
                end
            end)
        end
    end
end)

-- Função do Botão
btn.MouseButton1Click:Connect(function()
    reachActive = not reachActive
    if reachActive then
        btn.Text = "REACH 60: ATIVO ✅"
        btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    else
        btn.Text = "REACH 60: OFF"
        btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        -- Reseta o tamanho ao desligar
        pcall(function()
            local tool = player.Character:FindFirstChildOfClass("Tool")
            if tool and tool:FindFirstChild("Handle") then
                tool.Handle.Size = Vector3.new(1, 1, 1)
                tool.Handle.Transparency = 0
            end
        end)
    end
end)
