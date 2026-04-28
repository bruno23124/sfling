--[[ 
    GHOST FLING V18 - EXTERMINADOR
    Requer: Ghost Fly V17 Ativado
]]

local runService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local character = player.Character
local root = character:WaitForChild("HumanoidRootPart")

local flingActive = false

-- Interface Simples
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 40, 0, 40)
btn.Position = UDim2.new(0, 50, 0, 85) -- Do lado do botão de Fly
btn.Text = "FLING"
btn.BackgroundTransparency = 0.5

btn.MouseButton1Click:Connect(function()
    flingActive = not flingActive
    btn.BackgroundColor3 = flingActive and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(255, 255, 255)
end)

runService.Heartbeat:Connect(function()
    if flingActive and character and root then
        -- Faz o RootPart girar loucamente apenas para a física, mas você continua vendo normal
        root.AngularVelocity = Vector3.new(0, 99999, 0)
        root.Velocity = Vector3.new(99999, 99999, 99999)
        
        -- Garante que você não morra no processo
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)
