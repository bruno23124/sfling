--[[ 
    GROUND TORNADO V25 - PDB EDITION
    Instruções: Apenas encoste nos players andando.
]]

local player = game.Players.LocalPlayer
local char = player.Character
local root = char:WaitForChild("HumanoidRootPart")
local runService = game:GetService("RunService")

-- 1. VELOCIDADE DE CORRIDA (Sem voar, apenas correr rápido)
char.Humanoid.WalkSpeed = 60 -- Velocidade alta, mas que não kicka fácil

-- 2. O FLING TERRESTRE
local flingActive = true
runService.Heartbeat:Connect(function()
    if flingActive and root then
        -- Cria a força rotacional invisível
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false -- Você atravessa as coisas
                -- Mas o RootPart vai ter a força de impacto
                if part.Name == "HumanoidRootPart" then
                    part.Velocity = Vector3.new(0, 5000, 0) -- Força pra CIMA
                    part.AngularVelocity = Vector3.new(0, 9999, 0) -- Giro louco
                end
            end
        end
    end
end)

print("✅ TORNADO ATIVO! Corra na direção dos players na quadra.")
