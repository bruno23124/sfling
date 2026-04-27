local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

-- Remove interface antiga se existir
if coreGui:FindFirstChild("Reach60_GUI") then 
    coreGui:FindFirstChild("Reach60_GUI"):Destroy() 
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Reach60_GUI"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local reachActive = false
local reachSize = Vector3.new(60, 60, 60)

-- Botão de Ativar/Desativar
local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.8, 0) -- Posição bem visível na direita
btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
btn.Text = "REACH 60: OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 18
btn.Parent = screenGui

-- Torna o botão arrastável (caso ele tampe algo)
btn.Active = true
btn.Draggable = true

-- Função principal do Alcance
task.spawn(function()
    while true do
        task.wait(0.2)
        if reachActive then
            pcall(function()
                local tool = player.Character and player.Character:FindFirstChildOfClass("Tool")
                if tool and tool:FindFirstChild("Handle") then
                    tool.Handle.Size = reachSize
                    tool.Handle.Transparency = 0.8 -- Deixa um pouco visível pra você saber que funcionou
                    tool.Handle.Massless = true
                    tool.Handle.CanCollide = false
                end
            end)
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    reachActive = not reachActive
    if reachActive then
        btn.Text = "REACH 60: LIGADO ✅"
        btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    else
        btn.Text = "REACH 60: OFF"
        btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        pcall(function()
            local tool = player.Character:FindFirstChildOfClass("Tool")
            if tool and tool:FindFirstChild("Handle") then
                tool.Handle.Size = Vector3.new(1, 0.8, 4) -- Tamanho normal da Ripa
                tool.Handle.Transparency = 0
            end
        end)
    end
end)
