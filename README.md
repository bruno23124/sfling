--[[ 
    RIPA ULTRA HITBOX - 60 STUDS
    Alcance massivo para dominar o mapa.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")

if coreGui:FindFirstChild("RipaUltra") then coreGui:FindFirstChild("RipaUltra"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "RipaUltra"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local hitboxActive = false
local rangeSize = 60 -- O alcance gigante que você pediu

-- Interface
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 160, 0, 45)
frame.Position = UDim2.new(1, -180, 1, -110)
frame.BackgroundColor3 = Color3.fromRGB(50, 20, 0)
frame.Parent = screenGui

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -10, 1, -10)
btn.Position = UDim2.new(0, 5, 0, 5)
btn.Text = "Hitbox 60: OFF"
btn.BackgroundColor3 = Color3.fromRGB(100, 40, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Parent = frame

-- LOOP DA HITBOX GIGANTE
task.spawn(function()
    while true do
        task.wait(0.5)
        if hitboxActive then
            pcall(function()
                local char = player.Character
                local tool = char:FindFirstChild("Ripa") or player.Backpack:FindFirstChild("Ripa")
                
                if tool and tool:FindFirstChild("Handle") then
                    -- Expande a Hitbox para 60 studs
                    tool.Handle.Size = Vector3.new(rangeSize, rangeSize, rangeSize)
                    tool.Handle.Transparency = 1 -- Mantém invisível para não verem o bloco gigante
                    tool.Handle.CanCollide = false
                    tool.Handle.Massless = true
                end
            end)
        else
            -- Volta ao normal quando desliga
            pcall(function()
                local tool = player.Character:FindFirstChild("Ripa") or player.Backpack:FindFirstChild("Ripa")
                if tool and tool:FindFirstChild("Handle") then
                    tool.Handle.Size = Vector3.new(1, 5, 1)
                    tool.Handle.Transparency = 0
                end
            end)
        end
    end
end)

-- AUTO CLICKER PARA AJUDAR NO ALCANCE
task.spawn(function()
    while true do
        task.wait(0.1)
        if hitboxActive then
            local tool = player.Character and player.Character:FindFirstChild("Ripa")
