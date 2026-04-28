--[[ 
    GHOST FLING V18 - EXTERMINADOR
    Requer: Ghost Fly V17 já ativado no mapa
]]

local runService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local character = player.Character
local root = character:WaitForChild("HumanoidRootPart")

local flingActive = false

-- Interface Pequena (ao lado do botão de Fly)
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "Win32_Fling"
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 35, 0, 35)
btn.Position = UDim2.new(0, 45, 0, 85) -- Posição ajustada
btn.Text = "FL"
btn.BackgroundTransparency = 0.7
btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

-- Função para ligar/desligar o Fling
btn.MouseButton1Click:Connect(function()
    flingActive = not flingActive
    if flingActive then
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        btn.Text = "ON"
    else
        btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        btn.Text = "FL"
    end
end)

runService.Heartbeat:Connect(function()
    if flingActive and character and root then
        -- Cria uma velocidade angular massiva (Giro invisível)
        -- Isso faz qualquer um que encostar em você ser arremessado
        root.AngularVelocity = Vector3.new(0, 99999, 0)
        root.Velocity = Vector3.new(99999, 99999, 99999)
        
        -- Mantém você sem colisão para o anti-cheat não te puxar
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)
