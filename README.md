--[[ 
    GHOST KILLER V6 - ANTI-NICK & STEALTH
    Remove o nome da cabeça e esconde o corpo real.
]]

local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")
local runService = game:GetService("RunService")

if coreGui:FindFirstChild("GhostV6") then coreGui:FindFirstChild("GhostV6"):Destroy() end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GhostV6"
screenGui.Parent = coreGui
screenGui.ResetOnSpawn = false 

local active = false
local range = 35 -- Raio de ação (35 é mais seguro para não bugar o teleporte)

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 160, 0, 50)
btn.Position = UDim2.new(0.85, 0, 0.65, 0)
btn.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
btn.Text = "STEALTH AURA: OFF"
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 14
btn.Parent = screenGui
btn.Draggable = true

-- FUNÇÃO PARA ESCONDER O NICK (Local e Servidor-Bypass)
local function hideIdentity(char)
    local hum = char:WaitForChild("Humanoid")
    -- Remove o nome padrão do Roblox
    hum.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
    
    -- Deleta BillboardGuis (Nomes customizados do mapa)
    for _, v in pairs(char:GetDescendants()) do
        if v:IsA("BillboardGui") then
            v:Destroy()
        end
    end
end

-- LOOP DE ATAQUE E POSICIONAMENTO
runService.RenderStepped:Connect(function()
    if active then
        pcall(function()
            local char = player.Character
            local tool = char:FindFirstChildOfClass("Tool")
            local hrp = char:FindFirstChild("HumanoidRootPart")
            
            if hrp and tool then
                -- Busca o alvo
                local target = nil
                local dist = range
                for _, v in pairs(game.Players:GetPlayers()) do
                    if v ~= player and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
                        local tHRP = v.Character:FindFirstChild("HumanoidRootPart")
                        if tHRP then
                            local d = (hrp.Position - tHRP.Position).Magnitude
                            if d < dist then
                                target = v.Character
                                dist = d
                            end
                        end
                    end
                end

                if target then
                    -- TELEPORTE ESTRATÉGICO: 5 studs abaixo do alvo (Invisível e difícil de clicar)
                    hrp.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, -5, 0)
                    
                    -- ATAQUE
                    tool:Activate()
                    firetouchinterest(target.HumanoidRootPart, tool.Handle, 0)
                    firetouchinterest(target.HumanoidRootPart, tool.Handle, 1)
                end
            end
        end)
    end
end)

btn.MouseButton1Click:Connect(function()
    active = not active
    if active then
        if player.Character then hideIdentity(player.Character) end
        btn.Text = "MODO FANTASMA: ATIVO 🕵️"
        btn.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
    else
        btn.Text = "STEALTH AURA: OFF"
        btn.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
        -- Tenta resetar o nome ao desligar (precisa de reset do personagem no jogo)
        if player.Character then 
            player.Character.Humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.Viewer 
        end
    end
end)
