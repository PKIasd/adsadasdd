--[[
  Adiciona ao seu GojoHub uma opção "Ativar Teleporte".
  - Enquanto ativada, ao clicar com o mouse no chão (parte), você será teleportado para o local do clique.
  - Para desativar, clique novamente no botão.
  - Seguro: só teleporta se apontar para uma BasePart no Workspace.
--]]

-- Adicione ao final dos botões (ajuste a posição se necessário):
local botaoTeleport = Instance.new("TextButton")
botaoTeleport.Text = "Ativar Teleporte"
botaoTeleport.Size = UDim2.new(1, -20, 0, 40)
botaoTeleport.Position = UDim2.new(0, 10, 0, 460)
botaoTeleport.BackgroundColor3 = Color3.fromRGB(120, 255, 180)
botaoTeleport.TextColor3 = Color3.new(0,0.4,0)
botaoTeleport.Font = Enum.Font.SourceSansBold
botaoTeleport.TextSize = 18
botaoTeleport.Parent = frame

-- Controle do Teleporte
local teleportAtivo = false
local mouseConn = nil

local function setTeleport(enabled)
    teleportAtivo = enabled
    if teleportAtivo then
        botaoTeleport.Text = "Desativar Teleporte"
        -- Conectar mouse click
        local mouse = player:GetMouse()
        mouseConn = mouse.Button1Down:Connect(function()
            if not teleportAtivo then return end
            local char = player.Character
            if not char or not char:FindFirstChild("HumanoidRootPart") then return end
            local target = mouse.Target
            if target and target:IsA("BasePart") then
                -- Teleporta o HumanoidRootPart para o ponto clicado
                char.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.p + Vector3.new(0, 3, 0)) -- +3 para não cair no chão
            end
        end)
    else
        botaoTeleport.Text = "Ativar Teleporte"
        if mouseConn then
            mouseConn:Disconnect()
            mouseConn = nil
        end
    end
end

botaoTeleport.MouseButton1Click:Connect(function()
    setTeleport(not teleportAtivo)
end)

-- Segurança extra: desativa teleporte ao morrer
player.CharacterAdded:Connect(function()
    setTeleport(false)
end)

-- (restante do seu script permanece igual)
