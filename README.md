--[[ 
    GHOST BYPASS V21 - PAINEL DE CONTROLE
    Bypass: Rank 300, Anti-Kick & Auto-Kill
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- 1. IMUNIDADE E RANK 300
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

-- Variáveis de Controle
_G.AutoKill = false

-- 2. FUNÇÃO DE ATAQUE (Ripa)
local function getVBToken()
    for _, v in pairs(ReplicatedStorage:GetChildren()) do
        if v:IsA("RemoteFunction") and v:GetAttribute("oyvey") then
            return v:InvokeServer("THUG")
        end
    end
end

local function doKill()
    local token = getVBToken()
    local tool = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Tool")
    if tool and tool:FindFirstChild("Handle") then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= localPlayer and p.Character and p.Character:FindFirstChild("Head") then
                firetouchinterest(p.Character.Head, tool.Handle, 0)
                firetouchinterest(p.Character.Head, tool.Handle, 1)
            end
        end
    end
end

-- Loop de Ataque Automático
task.spawn(function()
    while true do
        task.wait(0.1)
        if _G.AutoKill then
            pcall(doKill)
        end
    end
end)

-- 3. INTERFACE COM BOTÕES ON/OFF
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "GhostPanel"

local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 150, 0, 100)
frame.Position = UDim2.new(0.05, 0, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 2

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "RIPA GHOST V21"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1

local onBtn = Instance.new("TextButton", frame)
onBtn.Size = UDim2.new(0, 130, 0, 30)
onBtn.Position = UDim2.new(0.5, -65, 0, 35)
onBtn.Text = "LIGAR MATANÇA"
onBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
onBtn.TextColor3 = Color3.fromRGB(255, 255, 255)

local offBtn = Instance.new("TextButton", frame)
offBtn.Size = UDim2.new(0, 130, 0, 30)
offBtn.Position = UDim2.new(0.5, -65, 0, 70)
offBtn.Text = "DESLIGAR"
offBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
offBtn.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Conexões dos Botões
onBtn.MouseButton1Click:Connect(function()
    _G.AutoKill = true
    onBtn.Text = "EXECUTANDO..."
end)

offBtn.MouseButton1Click:Connect(function()
    _G.AutoKill = false
    onBtn.Text = "LIGAR MATANÇA"
end)
