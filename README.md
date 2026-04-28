--[[ 
    GHOST BUMPER V26 - PDB BYPASS
    Como usar: Chegue perto do player e "atravesse" ele.
    O impacto físico vai lançá-lo sem disparar o anti-cheat de CFrame.
]]

local RunService = game:GetService("RunService")
local player = game.Players.LocalPlayer
local char = player.Character
local root = char:WaitForChild("HumanoidRootPart")

-- 1. CONFIGURAÇÃO DA VELOCIDADE (Sem dar Speed Kick)
char.Humanoid.WalkSpeed = 45 -- Valor seguro para o PDB

-- 2. CRIAÇÃO DO OBJETO DE IMPACTO (Invisível)
local bumper = Instance.new("Part")
bumper.Name = "BypassBumper"
bumper.Parent = char
bumper.Size = Vector3.new(4, 4, 4) -- Área de impacto
bumper.Transparency = 0.8 -- Quase invisível
bumper.CanCollide = true
bumper.Color = Color3.fromRGB(255, 255, 0)

-- Gruda o objeto em você para ele te seguir
local weld = Instance.new("Weld", bumper)
weld.Part0 = bumper
weld.Part1 = root
weld.C0 = CFrame.new(0, 0, 1.5) -- Fica um pouco na sua frente

-- 3. BYPASS DE COLISÃO DO SEU CORPO
-- Você fica fantasma, mas o Bumper (objeto) continua sólido
for _, v in pairs(char:GetDescendants()) do
    if v:IsA("BasePart") and v ~= bumper then
        v.CanCollide = false
    end
end

-- 4. O MOTOR DE FLING (O segredo do PDB)
-- Usamos BodyAngularVelocity porque o VB Anti-Cheat foca em vetores de Velocity
local force = Instance.new("BodyAngularVelocity", bumper)
force.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
force.AngularVelocity = Vector3.new(0, 999999, 0) -- Giro massivo

local push = Instance.new("BodyVelocity", bumper)
push.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
push.Velocity = Vector3.new(0, 20, 0) -- Leve força para cima para tirar o cara do chão

-- 5. INTERFACE SIMPLES
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 100, 0, 40)
btn.Position = UDim2.new(0, 10, 0.5, -20)
btn.Text = "BUMPER ON"
btn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)

btn.MouseButton1Click:Connect(function()
    bumper.CanCollide = not bumper.CanCollide
    btn.Text = bumper.CanCollide and "BUMPER ON" or "BUMPER OFF"
    btn.BackgroundColor3 = bumper.CanCollide and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(200, 0, 0)
end)

print("✅ Bypass Bumper V26 Ativo! Aproxime-se dos alvos.")
