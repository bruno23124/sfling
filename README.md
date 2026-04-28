--[[ 
    STEALTH FLY V15 - ANTI-DETECTION
    Bypass: Cosmo$ (Punish Check) & VB Anti-Cheat (Speed Check)
]]

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")
local mouse = player:GetMouse()
local runService = game:GetService("RunService")

-- 1. CLOAK DE ADMIN (Bypass de Punição do Cosmo)
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = player

-- Variáveis de Voo
local flying = false
local speed = 50 -- Velocidade segura para não dar kick por SpeedHack
local ctrl = {f = 0, b = 0, l = 0, r = 0}
local lastCtrl = {f = 0, b = 0, l = 0, r = 0}

-- 2. INTERFACE SILENCIOSA (Evita LogService)
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "Module_" .. math.random(100, 999)

local btn = Instance.new("TextButton", sg)
btn.Size = UDim2.new(0, 40, 0, 40)
btn.Position = UDim2.new(0, 10, 0.7, 0)
btn.Text = "FLY"
btn.BackgroundTransparency = 0.5
btn.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Função de Voo
local function toggleFly()
    flying = not flying
    if flying then
        local bv = Instance.new("BodyVelocity", root)
        bv.Velocity = Vector3.new(0, 0.1, 0) -- Força mínima para anular a gravidade
        bv.MaxForce = Vector3.new(9e9, 9e9, 9e9)
        bv.Name = "Velocity_X"
        
        local bg = Instance.new("BodyGyro", root)
        bg.P = 9e4
        bg.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
        bg.CFrame = root.CFrame
        bg.Name = "Gyro_X"
        
        task.spawn(function()
            repeat
                runService.RenderStepped:Wait()
                character.Humanoid.PlatformStand = true
                
                if ctrl.l + ctrl.r ~= 0 or ctrl.f + ctrl.b ~= 0 then
                    speed = 50 -- Velocidade estável
                else
                    speed = 0
                end
                
                if (ctrl.l + ctrl.r) ~= 0 or (ctrl.f + ctrl.b) ~= 0 then
                    bv.Velocity = ((workspace.CurrentCamera.CFrame.LookVector * (ctrl.f + ctrl.b)) + ((workspace.CurrentCamera.CFrame * CFrame.new(ctrl.l + ctrl.r, (ctrl.f + ctrl.b) * 0.2, 0).p) - workspace.CurrentCamera.CFrame.p)) * speed
                    lastCtrl = {f = ctrl.f, b = ctrl.b, l = ctrl.l, r = ctrl.r}
                else
                    bv.Velocity = Vector3.new(0, 0.1, 0)
                end
                bg.CFrame = workspace.CurrentCamera.CFrame
            until not flying
            
            -- Limpeza ao desligar
            character.Humanoid.PlatformStand = false
            bv:Destroy()
            bg:Destroy()
        end)
    end
end

-- Controles (Teclado)
mouse.KeyDown:Connect(function(key)
    if key:lower() == "w" then ctrl.f = 1
    elseif key:lower() == "s" then ctrl.b = -1
    elseif key:lower() == "a" then ctrl.l = -1
    elseif key:lower() == "d" then ctrl.r = 1
    elseif key:lower() == "f" then toggleFly() -- Tecla F para Voar
    end
end)

mouse.KeyUp:Connect(function(key)
    if key:lower() == "w" then ctrl.f = 0
    elseif key:lower() == "s" then ctrl.b = 0
    elseif key:lower() == "a" then ctrl.l = 0
    elseif key:lower() == "d" then ctrl.r = 0
    end
end)

btn.MouseButton1Click:Connect(toggleFly)
