--[[ 
    KILL ALL V13 - NO TOOL / REMOTE BYPASS
    Bypass: VB Anti-Cheat Token & Cosmo Admin Check
]]

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local localPlayer = Players.LocalPlayer

-- 1. IMUNIDADE LOCAL (Engana o Punish do Cosmo)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = localPlayer

-- 2. FUNÇÃO PARA PEGAR O TOKEN (A chave que o VB pede)
local function getSecretToken()
    for _, v in pairs(ReplicatedStorage:GetChildren()) do
        if v:IsA("RemoteFunction") and v:GetAttribute("oyvey") then
            return v:InvokeServer("THUG") -- A falha "THUG" que nos dá a senha
        end
    end
    return nil
end

-- 3. FUNÇÃO DE ATAQUE REMOTO (Sem precisar de ferramenta)
local function killEveryone()
    local token = getSecretToken()
    if not token then return end -- Se não pegar o token, o script para pra não te dar ban

    -- Localiza o Remote de Dano real (não a isca do ACS)
    local events = ReplicatedStorage:FindFirstChild("Events")
    local damageRemote = events and events:FindFirstChild("Damage")

    if damageRemote then
        for _, target in pairs(Players:GetPlayers()) do
            if target ~= localPlayer and target.Character and target.Character:FindFirstChild("Humanoid") then
                -- Dispara o dano diretamente no servidor
                -- O formato do disparo depende do script de dano do mapa (baseado no PDB enviado)
                damageRemote:FireServer(target.Character.Humanoid, 100, token) 
            end
        end
    end
end

-- 4. INTERFACE ULTRA DISCRETA (Evita scanners de UI do cosmo2.txt)
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "System_" .. math.random(1000, 9999)

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 35, 0, 35)
btn.Position = UDim2.new(0, 5, 0.5, 0)
btn.Text = "X" -- Botão pequeno no canto
btn.BackgroundTransparency = 0.8
btn.TextColor3 = Color3.fromRGB(255, 255, 255)

btn.MouseButton1Click:Connect(function()
    killEveryone()
    btn.Text = "OK"
    task.wait(1)
    btn.Text = "X"
end)

-- Tecla K de atalho
game:GetService("UserInputService").InputBegan:Connect(function(input, chat)
    if not chat and input.KeyCode == Enum.KeyCode.K then
        killEveryone()
    end
end)
