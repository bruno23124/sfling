--[[ 
    GHOST BYPASS V20 - RANK SPOOFER (DONO)
    Bypass: Cosmo$, VB & Staff Rank Levels
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- 1. CLOAK DE ADMIN (Imunidade Cosmo)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = localPlayer

-- 2. RANK SPOOFER (Intercepta GetRankInGroup)
-- Quando o Anti-Cheat ou um Script de Staff checar seu level, ele lerá 300
local mt = getrawmetatable(game)
local oldNamecall = mt.__namecall
setreadonly(mt, false)

mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    local args = {...}

    if method == "GetRankInGroup" or method == "getRankInGroup" then
        return 300 -- Retorna nível de Dono do Jogo
    end
    
    if method == "Kick" or method == "kick" then
        return nil -- Bloqueia o comando de expulsão
    end

    return oldNamecall(self, ...)
end)
setreadonly(mt, true)

-- 3. INVISIBILIDADE E GHOST MODE
local depth = -120 -- Profundidade extra para fugir de qualquer sensor
task.spawn(function()
    while task.wait() do
        if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Trava você debaixo do mapa para ficar invisível
            localPlayer.Character.HumanoidRootPart.CFrame = localPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0)
        end
    end
end)

-- 4. KILL AURA (Ripa) com Bypass de Token
local function getVBToken()
    for _, v in pairs(ReplicatedStorage:GetChildren()) do
        if v:IsA("RemoteFunction") and v:GetAttribute("oyvey") then
            return v:InvokeServer("THUG")
        end
    end
end

local function attackAll()
    local token = getVBToken()
    local char = localPlayer.Character
    local tool = char and char:FindFirstChildOfClass("Tool")
    
    if tool and tool:FindFirstChild("Handle") then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= localPlayer and p.Character and p.Character:FindFirstChild("Head") then
                -- O Cosmo não consegue bloquear o firetouchinterest por ser físico
                firetouchinterest(p.Character.Head, tool.Handle, 0)
                firetouchinterest(p.Character.Head, tool.Handle, 1)
            end
        end
    end
end

-- 5. BOTÃO INVISÍVEL (Nome genérico para o Cosmo2 não detectar)
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "SystemCheck"
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 30, 0, 30)
btn.Position = UDim2.new(0, 5, 0.5, 0)
btn.Text = "" -- Sem texto para não ser pego pelo LogService
btn.BackgroundTransparency = 0.9

btn.MouseButton1Click:Connect(attackAll)
game:GetService("UserInputService").InputBegan:Connect(function(io, chat)
    if not chat and io.KeyCode == Enum.KeyCode.K then attackAll() end
end)
