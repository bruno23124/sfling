--[[ 
    PHYSICAL FLING V24 - SERVER-SIDE FORCE
    Instruções: Chegue perto do player e "atropele" ele.
]]

local RunService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local character = player.Character
local root = character:WaitForChild("HumanoidRootPart")

-- 1. CRIA O MÍSSIL DE FÍSICA
local power = 999999 -- Força do arremesso
local flingPart = Instance.new("Part")
flingPart.Name = "BypassPart"
flingPart.Parent = character
flingPart.Size = Vector3.new(5, 5, 5) -- Tamanho do "hitbox"
flingPart.Transparency = 0.5 -- 0.5 para você ver onde está batendo, depois mude para 1
flingPart.CanCollide = true -- PRECISA de colisão para empurrar os outros
flingPart.Color = Color3.fromRGB(255, 0, 0)

-- 2. TIRA SUA COLISÃO (Ghost Mode) mas mantém a do Míssil
for _, v in pairs(character:GetDescendants()) do
    if v:IsA("BasePart") and v ~= flingPart then
        v.CanCollide = false
    end
end

-- 3. LOOP DE GIRO E POSIÇÃO
RunService.Stepped:Connect(function()
    if character and root then
        -- O míssil fica exatamente na sua frente
        flingPart.CFrame = root.CFrame * CFrame.new(0, 0, -2) 
        
        -- Velocidade Angular (O segredo do Fling)
        flingPart.AngularVelocity = Vector3.new(0, power, 0)
        flingPart.Velocity = Vector3.new(0, power, 0) -- Empurra para CIMA
    end
end)

print("✅ Míssil Ativo! Encoste nos players para mandá-los pro céu.")
