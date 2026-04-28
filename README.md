--[[ 
    THE JUÍZO FINAL V22 - SKY LAUNCHER
    Bypass: Rank 300 & VB Physics Check
]]

local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
local runService = game:GetService("RunService")

-- 1. IMUNIDADE E AUTORIDADE (Rank 300)
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "GetRankInGroup" then return 300 end
    if method == "Kick" or method == "kick" then return nil end
    return old(self, ...)
end)
setreadonly(mt, true)

-- 2. FUNÇÃO DE LANÇAMENTO (Sky Launch)
local function launchPlayers()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= localPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            task.spawn(function()
                local root = p.Character.HumanoidRootPart
                -- Teleporta o cara para 50 mil studs de altura instantaneamente
                -- Usamos um pequeno loop para garantir que o anti-cheat de queda não o puxe de volta
                for i = 1, 5 do
                    root.CFrame = root.CFrame * CFrame.new(0, 10000, 0)
                    root.Velocity = Vector3.new(0, 5000, 0)
                    task.wait(0.05)
                end
            end)
        end
    end
end

-- 3. INTERFACE DE COMANDO
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "SkySystem"

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 150, 0, 50)
btn.Position = UDim2.new(0.4, 0, 0.1, 0) -- Topo da tela, centralizado
btn.Text = "JUDGMENT"
btn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.BlackOpsOne
btn.TextSize = 20
btn.BorderSizePixel = 3

-- Efeito visual de clique
btn.MouseButton1Click:Connect(function()
    btn.Text = "LANÇANDO..."
    btn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    launchPlayers()
    task.wait(2)
    btn.Text = "JUDGMENT"
    btn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
end)

-- Tecla J para o Juízo Final (Opcional)
game:GetService("UserInputService").InputBegan:Connect(function(io, chat)
    if not chat and io.KeyCode == Enum.KeyCode.J then
        launchPlayers()
    end
end)
