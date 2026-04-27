--[[ 
    SETSTATE FLING V3 (REPOSITORIO SFLING)
    Esse método quebra a proteção do servidor.
]]

local player = game.Players.LocalPlayer
local runService = game:GetService("RunService")
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
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "Fling V3: OFF"
btn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Parent = frame

-- LOGICA DE FLING POR ESTADO DE FISICA
runService.Stepped:Connect(function()
    if flingActive then
        local char = player.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        
        if hrp and hum then
            -- Força o estado de física que ignora o anti-fling do mapa
            hum:ChangeState(Enum.HumanoidStateType.Physics)
            hrp.Velocity = Vector3.new(0, 5000, 0) -- Velocidade invisível pro servidor
            
            -- Remove colisões internas para você não bugar
            for _, v in pairs(char:GetChildren()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end
end)

btn.MouseButton1Click:Connect(function()
    flingActive = not flingActive
    btn.Text = flingActive and "FLING: ATIVO 🔥" or "Fling V3: OFF"
    btn.BackgroundColor3 = flingActive and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(80, 80, 80)
    
    if not flingActive then
        player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
    end
end)
