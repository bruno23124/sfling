--[[ 
    GHOST FLY V17 - ULTRA STEALTH
    Bypass: Cosmo$ Hard-Anchor & VB Gravity Check
]]

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")
local runService = game:GetService("RunService")
local inputService = game:GetService("UserInputService")

-- 1. CLOAK DE IMUNIDADE (Bypass Cosmo)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = player

-- Variáveis
local flying = false
local speed = 1.5 -- Velocidade ajustada para não dar Kick
local cam = workspace.CurrentCamera

-- Interface Invisível para Scanners
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "Win_" .. math.random(111, 999)
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 30, 0, 30)
btn.Position = UDim2.new(0, 5, 0.85, 0)
btn.Text = ""
btn.BackgroundTransparency = 0.9

local function toggleFly()
    flying = not flying
    if flying then
        btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    else
        btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    end
end

-- Loop de Movimentação Fantasma
runService.Stepped:Connect(function()
    if flying and character then
        -- Desativa colisões para não ficar preso no chão
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
        
        -- Movimentação por CFrame (Ignora Gravidade)
        local moveDir = Vector3.new(0, 0.001, 0) -- Oscilação mínima
        
        if inputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + (cam.CFrame.LookVector * speed) end
        if inputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - (cam.CFrame.LookVector * speed) end
        if inputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - (cam.CFrame.RightVector * speed) end
        if inputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + (cam.CFrame.RightVector * speed) end
        if inputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, speed, 0) end
        if inputService:IsKeyDown(Enum.KeyCode.LeftControl) then moveDir = moveDir - Vector3.new(0, speed, 0) end
        
        root.CFrame = root.CFrame + moveDir
        root.Velocity = Vector3.new(0, 0, 0) -- Zera a velocidade física para o VB não ver
    end
end)

btn.MouseButton1Click:Connect(toggleFly)
inputService.InputBegan:Connect(function(io, p)
    if not p and io.KeyCode == Enum.KeyCode.F then toggleFly() end
end)
