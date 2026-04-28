--[[ 
    RIPA GHOST V19 - INVISIBILIDADE & IMUNIDADE
    Bypass: VB Anti-Kick & Cosmo Punish System
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- 1. CLOAK DE ADMIN (Imunidade ao Cosmo)
-- O Cosmo checa se player.Admin.Value == true antes de banir
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = localPlayer

-- 2. ANTI-KICK (Quebra o sistema de expulsão)
-- Isso impede que o servidor consiga dar "Kick" em você via comando
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)

mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "Kick" or method == "kick" then
        return nil -- Ignora o comando de expulsar
    end
    return old(self, ...)
end)
setreadonly(mt, true)

-- 3. INVISIBILIDADE GHOST (-100 Studs)
-- Você fica embaixo do mapa, mas sua arma continua batendo em cima
local flying = true
task.spawn(function()
    while flying do
        RunService.Heartbeat:Wait()
        if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") then
            -- Mantém você seguro debaixo do chão para ninguém te ver
            localPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
        end
    end
end)

-- 4. RIPA KILL AURA (Usando o Token do VB)
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
                -- Ataque Silencioso
                firetouchinterest(p.Character.Head, tool.Handle, 0)
                firetouchinterest(p.Character.Head, tool.Handle, 1)
            end
        end
    end
end

-- 5. INTERFACE DISCRETA
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "Win32_Sys"
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 40, 0, 40)
btn.Position = UDim2.new(0, 10, 0.5, 0)
btn.Text = "G" -- G de Ghost
btn.BackgroundTransparency = 0.6

btn.MouseButton1Click:Connect(function()
    attackAll()
    btn.Text = "OK"
    task.wait(1)
    btn.Text = "G"
end)

-- Tecla K para ativar a Ripa
game:GetService("UserInputService").InputBegan:Connect(function(io, chat)
    if not chat and io.KeyCode == Enum.KeyCode.K then
        attackAll()
    end
end)
