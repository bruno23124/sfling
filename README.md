--[[ 
    GHOST KILLER V11 - MASS EXTERMINATION
    Bypass: Cosmo$ & VB Anti-Cheat
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer

-- 1. CLOAK DE IMUNIDADE (Bypass Punish Cosmo)
-- O sistema ignora quem tem a tag "Admin" 
local admin = Instance.new("BoolValue")
admin.Name = "Admin"
admin.Value = true
admin.Parent = localPlayer

-- 2. FUNÇÃO DE ATAQUE SILENCIOSO
local function killAll()
    local character = localPlayer.Character
    if not character then return end
    
    -- Localiza sua arma (Ripa)
    local tool = character:FindFirstChildOfClass("Tool") or localPlayer.Backpack:FindFirstChildOfClass("Tool")
    if not tool or not tool:FindFirstChild("Handle") then return end
    
    local handle = tool.Handle
    
    for _, target in pairs(Players:GetPlayers()) do
        if target ~= localPlayer and target.Character and target.Character:FindFirstChild("Head") then
            local targetHead = target.Character.Head
            
            -- Simula o toque da arma no pescoço do alvo
            -- Isso não dispara Remotes falsos como 'Damage' ou 'Hit' [cite: 7, 10]
            firetouchinterest(targetHead, handle, 0)
            task.wait()
            firetouchinterest(targetHead, handle, 1)
        end
    end
end

-- 3. INTERFACE DISCRETA (Evita scanners de UI) [cite: 24]
local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
local btn = Instance.new("TextButton", sg)
btn.Name = "Module_X" -- Nome que não está na Blacklist [cite: 23]
btn.Size = UDim2.new(0, 40, 0, 40)
btn.Position = UDim2.new(0, 5, 0.4, 0)
btn.Text = "K"
btn.BackgroundTransparency = 0.6

btn.MouseButton1Click:Connect(function()
    killAll()
end)
