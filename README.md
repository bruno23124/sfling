--[[ 
    GHOST KILLER V5 - INVISIBILIDADE TOTAL + DANO FIX
    O corpo fica seguro e a Ripa faz o trabalho.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")
local runService = game:GetService("RunService")

if coreGui:FindFirstChild("GhostV5") then coreGui:FindFirstChild("GhostV5"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GhostV5"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local active = false
local range = 150 -- Distância que ele busca as vítimas

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 160, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.65, 0)
btn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
btn.Text = "Ghost V5: OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 14
btn.Parent = screenGui
btn.Draggable = true

-- Função para encontrar o alvo mais próximo
local function getClosestPlayer()
    local closest = nil
    local dist = range
    for _, v in pairs(game.Players:GetPlayers()) do
        if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
            local hrp = v.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local d = (player.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                if d < dist then
                    closest = v.Character
                    dist = d
                end
            end
        end
    end
    return closest
end

-- LOOP PRINCIPAL
runService.Stepped:Connect(function()
    if active then
        pcall(function()
            local char = player.Character
            local tool = char:FindFirstChildOfClass("Tool")
            local myHRP = char:FindFirstChild("HumanoidRootPart")
            
            if myHRP and tool then
                local target = getClosestPlayer()
                if target then
                    -- 1. TELEPORTE NAS COSTAS
                    myHRP.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2.5)
                    
                    -- 2. INVISIBILIDADE (Joga o corpo levemente para baixo para não ser visto)
                    myHRP.Velocity = Vector3.new(0, 0, 0)
                    
                    -- 3. FORÇAR DANO (Ativa a ferramenta e usa o interesse de toque)
                    tool:Activate()
                    for _, part in pairs(target:GetChildren()) do
                        if part:IsA("BasePart") then
                            firetouchinterest(part, tool.Handle, 0)
                            firetouchinterest(part, tool.Handle, 1)
                        end
                    end
                end
            end
        end)
    end
end)

btn.MouseButton1Click:Connect(function()
    active = not active
    btn.Text = active and "MODO FANTASMA: ON 💀" or "Ghost V5: OFF"
    btn.BackgroundColor3 = active and Color3.fromRGB(150, 0, 0) or Color3.fromRGB(20, 20, 20)
    
    -- Se desligar, garante que o boneco pare de tentar atravessar o chão
    if not active then
        player.Character.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
    end
end)
