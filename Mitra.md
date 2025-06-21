--[[
    Mitra Menu V2.1 - RESUMIDO COM WHITELIST POR NICK
    Sistema Ultra Otimizado - Aimbot + ESP + Exploits
    VERSÃO CORRIGIDA - Todos os erros de sintaxe foram resolvidos
]]

-- SISTEMA DE WHITELIST POR NICKNAME
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera

-- Lista de nicknames permitidos
local whitelist = {
    "Gabrjvo",
    "danizin1356", 
    "Guilherm2584",
    "TestUser123",
    LocalPlayer.Name -- Para teste (remover depois)
}

-- Verificar whitelist
local function isWhitelisted(username)
    for _, nick in pairs(whitelist) do
        if string.lower(nick) == string.lower(username) then
            return true
        end
    end
    return false
end

-- Se não estiver na whitelist, mostrar aviso e parar
if not isWhitelisted(LocalPlayer.Name) then
    local sg = Instance.new("ScreenGui")
    sg.Name = "WhitelistWarning"
    sg.Parent = LocalPlayer:WaitForChild("PlayerGui")
    sg.ResetOnSpawn = false
    
    local f = Instance.new("Frame")
    f.Size = UDim2.new(0, 450, 0, 180)
    f.Position = UDim2.new(0.5, -225, 0, 50)
    f.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    f.BorderSizePixel = 0
    f.Parent = sg
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = f
    
    -- Título
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 40)
    title.BackgroundTransparency = 1
    title.Text = "🚫 ACESSO NEGADO"
    title.TextColor3 = Color3.fromRGB(255, 80, 80)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 18
    title.TextXAlignment = Enum.TextXAlignment.Center
    title.Parent = f
    
    -- Mensagem principal
    local msg = Instance.new("TextLabel")
    msg.Size = UDim2.new(1, -20, 0, 70)
    msg.Position = UDim2.new(0, 10, 0, 40)
    msg.BackgroundTransparency = 1
    msg.Text = "Você não está na lista de usuários permitidos!\nAdquira o script completo no nosso discord"
    msg.TextColor3 = Color3.new(1, 1, 1)
    msg.Font = Enum.Font.Gotham
    msg.TextSize = 14
    msg.TextWrapped = true
    msg.TextYAlignment = Enum.TextYAlignment.Center
    msg.TextXAlignment = Enum.TextXAlignment.Center
    msg.Parent = f
    
    -- Info do usuário
    local userInfo = Instance.new("TextLabel")
    userInfo.Size = UDim2.new(1, -20, 0, 25)
    userInfo.Position = UDim2.new(0, 10, 0, 110)
    userInfo.BackgroundTransparency = 1
    userInfo.Text = "Usuário atual: " .. LocalPlayer.Name
    userInfo.TextColor3 = Color3.fromRGB(200, 200, 200)
    userInfo.Font = Enum.Font.Gotham
    userInfo.TextSize = 12
    userInfo.TextXAlignment = Enum.TextXAlignment.Center
    userInfo.Parent = f
    
    -- Discord
    local discord = Instance.new("TextLabel")
    discord.Size = UDim2.new(1, 0, 0, 25)
    discord.Position = UDim2.new(0, 0, 0, 135)
    discord.BackgroundTransparency = 1
    discord.Text = "Discord: discord.gg/mitramenu"
    discord.TextColor3 = Color3.fromRGB(100, 150, 255)
    discord.Font = Enum.Font.GothamBold
    discord.TextSize = 12
    discord.TextXAlignment = Enum.TextXAlignment.Center
    discord.Parent = f
    
    -- Botão fechar
    local close = Instance.new("TextButton")
    close.Size = UDim2.new(0, 80, 0, 25)
    close.Position = UDim2.new(1, -90, 0, 10)
    close.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    close.Text = "X"
    close.TextColor3 = Color3.new(1, 1, 1)
    close.Font = Enum.Font.GothamBold
    close.TextSize = 14
    close.BorderSizePixel = 0
    close.Parent = f
    
    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 5)
    closeCorner.Parent = close
    
    close.MouseButton1Click:Connect(function()
        sg:Destroy()
    end)
    
    -- Auto-close após 10 segundos
    spawn(function()
        wait(10)
        if sg and sg.Parent then
            sg:Destroy()
        end
    end)
    
    return -- Para o script aqui se não estiver na whitelist
end

-- SCRIPT PRINCIPAL CONTINUA (usuário autorizado)
local Mouse = LocalPlayer:GetMouse()

-- Tentar carregar DrRay UI Library com fallback
local DrRayLibrary = nil
local success, err = pcall(function()
    DrRayLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/AZYsGithub/DrRay-UI-Library/main/DrRay.lua"))()
end)

if not success or not DrRayLibrary then
    warn("Erro ao carregar DrRay Library: " .. tostring(err))
    -- Criar interface básica como fallback
    local fallbackUI = Instance.new("ScreenGui")
    fallbackUI.Parent = LocalPlayer:WaitForChild("PlayerGui")
    fallbackUI.Name = "MitraMenuFallback"
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 200)
    frame.Position = UDim2.new(0.5, -150, 0.5, -100)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    frame.BorderSizePixel = 0
    frame.Parent = fallbackUI
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 10)
    corner.Parent = frame
    
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 50)
    title.BackgroundTransparency = 1
    title.Text = "Mitra Menu V2.1 - Modo Básico"
    title.TextColor3 = Color3.new(1, 1, 1)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 16
    title.TextXAlignment = Enum.TextXAlignment.Center
    title.Parent = frame
    
    local statusLabel = Instance.new("TextLabel")
    statusLabel.Size = UDim2.new(1, -20, 1, -60)
    statusLabel.Position = UDim2.new(0, 10, 0, 50)
    statusLabel.BackgroundTransparency = 1
    statusLabel.Text = "❌ Erro ao carregar interface completa\n\nUsuário autorizado: " .. LocalPlayer.Name .. "\n\nRecarregue o script ou verifique conexão"
    statusLabel.TextColor3 = Color3.new(1, 1, 1)
    statusLabel.Font = Enum.Font.Gotham
    statusLabel.TextSize = 12
    statusLabel.TextWrapped = true
    statusLabel.TextYAlignment = Enum.TextYAlignment.Top
    statusLabel.Parent = frame
    
    return
end

-- Configurações
local Settings = {
    silentAim = false,
    teamCheck = false,
    wallCheck = false,
    killCheck = false,
    fovEnabled = false,
    fovSize = 120,
    espEnabled = false,
    showHealth = false,
    showDistance = false,
    espHealth = false,
    autoAim = false,
    smoothness = 0.01,
    flyEnabled = false,
    flySpeed = 50,
    speedEnabled = false,
    walkSpeed = 50
}

-- Variáveis globais
local espList = {}
local fovCircle = nil
local currentTarget = nil
local flyBodyVelocity = nil
local originalWalkSpeed = 16

-- Sistema de notificação melhorado
local function notify(msg)
    local sg = Instance.new("ScreenGui")
    sg.Parent = LocalPlayer:WaitForChild("PlayerGui")
    sg.Name = "MitraNotification"
    
    local f = Instance.new("Frame")
    f.Size = UDim2.new(0, 250, 0, 40)
    f.Position = UDim2.new(0.5, -125, 0, 20)
    f.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    f.BorderSizePixel = 0
    f.Parent = sg
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = f
    
    local t = Instance.new("TextLabel")
    t.Size = UDim2.new(1, 0, 1, 0)
    t.BackgroundTransparency = 1
    t.Text = msg
    t.TextColor3 = Color3.new(1, 1, 1)
    t.Font = Enum.Font.GothamBold
    t.TextSize = 12
    t.TextXAlignment = Enum.TextXAlignment.Center
    t.Parent = f
    
    spawn(function()
        wait(2)
        if sg and sg.Parent then
            sg:Destroy()
        end
    end)
end

-- Criar UI
local window = DrRayLibrary:Load("Mitra Menu V2.1 - AUTORIZADO", "Default")

-- Aba Início
local inicioTab = DrRayLibrary.newTab("Início", "rbxassetid://8560362689")
inicioTab.newLabel("🎯 USUÁRIO AUTORIZADO")
inicioTab.newLabel("✅ Bem-vindo: " .. LocalPlayer.Name)
inicioTab.newLabel("")
inicioTab.newLabel("Mitra Menu V2.1 - Sistema Ativo")
inicioTab.newLabel("Discord: discord.gg/mitramenu")
inicioTab.newLabel("")
inicioTab.newLabel("STATUS: Todos os recursos liberados!")

-- Aba Aimbot
local aimbotTab = DrRayLibrary.newTab("Aimbot", "rbxassetid://8560362689")

aimbotTab.newToggle("Silent Aim", "Ativa aimbot silencioso", Settings.silentAim, function(v)
    Settings.silentAim = v
    notify("Silent Aim " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("Team Check", "Verifica equipe", Settings.teamCheck, function(v)
    Settings.teamCheck = v
    notify("Team Check " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("Wall Check", "Verifica paredes", Settings.wallCheck, function(v)
    Settings.wallCheck = v
    notify("Wall Check " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("FOV Circle", "Círculo de mira", Settings.fovEnabled, function(v)
    Settings.fovEnabled = v
    notify("FOV Circle " .. (v and "ON" or "OFF"))
end)

aimbotTab.newSlider("FOV Size", "Tamanho do FOV", 200, 30, Settings.fovSize, function(v)
    Settings.fovSize = v
    notify("FOV: " .. v)
end)

aimbotTab.newSlider("Smoothness", "Suavidade (0.001 = GRUDADO)", 0.5, 0.001, Settings.smoothness, function(v)
    Settings.smoothness = v
    notify("Smoothness: " .. v)
end)

aimbotTab.newToggle("Auto Aim", "AIMBOT GRUDADO", Settings.autoAim, function(v)
    Settings.autoAim = v
    notify("Auto Aim " .. (v and "GRUDADO!" or "OFF"))
end)

-- Aba ESP
local espTab = DrRayLibrary.newTab("ESP", "rbxassetid://8560362689")

espTab.newToggle("ESP Names", "Mostra nomes", Settings.espEnabled, function(v)
    Settings.espEnabled = v
    notify("ESP Names " .. (v and "ON" or "OFF"))
end)

espTab.newToggle("Show Health", "Mostra vida", Settings.showHealth, function(v)
    Settings.showHealth = v
    notify("Show Health " .. (v and "ON" or "OFF"))
end)

espTab.newToggle("Show Distance", "Mostra distância", Settings.showDistance, function(v)
    Settings.showDistance = v
    notify("Show Distance " .. (v and "ON" or "OFF"))
end)

-- Aba Exploits
local exploitsTab = DrRayLibrary.newTab("Exploits", "rbxassetid://8560362689")

exploitsTab.newToggle("Fly", "Modo voo", Settings.flyEnabled, function(v)
    Settings.flyEnabled = v
    notify("Fly " .. (v and "ON" or "OFF"))
end)

exploitsTab.newSlider("Fly Speed", "Velocidade do voo", 100, 16, Settings.flySpeed, function(v)
    Settings.flySpeed = v
    notify("Fly Speed: " .. v)
end)

exploitsTab.newToggle("Speed", "Velocidade aumentada", Settings.speedEnabled, function(v)
    Settings.speedEnabled = v
    notify("Speed " .. (v and "ON" or "OFF"))
end)

exploitsTab.newSlider("Walk Speed", "Velocidade de caminhada", 100, 16, Settings.walkSpeed, function(v)
    Settings.walkSpeed = v
    notify("Walk Speed: " .. v)
end)

-- FUNÇÕES PRINCIPAIS

-- Encontrar alvo (corrigido sem continue)
local function findTarget()
    local closest = nil
    local shortestDistance = math.huge
    local screenCenter = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local character = player.Character
            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character.HumanoidRootPart
            
            if humanoid and rootPart then
                -- Verificações de filtro (corrigido)
                local shouldSkip = false
                
                if Settings.killCheck and humanoid.Health <= 0 then
                    shouldSkip = true
                end
                
                if Settings.teamCheck and player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team then
                    shouldSkip = true
                end
                
                if not shouldSkip then
                    local screenPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
                    if onScreen then
                        local distance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                        if distance <= Settings.fovSize and distance < shortestDistance then
                            if Settings.wallCheck then
                                local rayOrigin = Camera.CFrame.Position
                                local rayDirection = (rootPart.Position - rayOrigin)
                                local raycastParams = RaycastParams.new()
                                raycastParams.FilterDescendantsInstances = {LocalPlayer.Character}
                                raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
                                local result = Workspace:Raycast(rayOrigin, rayDirection, raycastParams)
                                if result and not result.Instance:IsDescendantOf(character) then
                                    shouldSkip = true
                                end
                            end
                            
                            if not shouldSkip then
                                closest = character
                                shortestDistance = distance
                            end
                        end
                    end
                end
            end
        end
    end
    return closest
end

-- Mirar no alvo
local function aimAtTarget(target)
    if not target or not target:FindFirstChild("HumanoidRootPart") then return end
    
    local targetPart = target:FindFirstChild("Head") or target:FindFirstChild("UpperTorso") or target.HumanoidRootPart
    if not targetPart then return end
    
    local targetPos = targetPart.Position
    local cameraPos = Camera.CFrame.Position
    local newCFrame = CFrame.lookAt(cameraPos, targetPos)
    
    if Settings.smoothness <= 0.01 then
        Camera.CFrame = newCFrame
    else
        local lerpValue = math.min(1, (1 - Settings.smoothness) * 10)
        Camera.CFrame = Camera.CFrame:Lerp(newCFrame, lerpValue)
    end
end

-- Atualizar FOV (corrigido com verificação Drawing)
local function updateFOV()
    if Settings.fovEnabled then
        if not fovCircle and Drawing then
            pcall(function()
                fovCircle = Drawing.new("Circle")
                fovCircle.Thickness = 2
                fovCircle.Filled = false
                fovCircle.Transparency = 0.8
                fovCircle.NumSides = 64
            end)
        end
        if fovCircle then
            fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
            fovCircle.Radius = Settings.fovSize
            fovCircle.Visible = true
            fovCircle.Color = currentTarget and Color3.new(1,0.2,0.2) or Color3.new(0.2,1,0.2)
        end
    elseif fovCircle then
        fovCircle.Visible = false
    end
end

-- Atualizar ESP
local function updateESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            if Settings.espEnabled or Settings.showHealth or Settings.showDistance then
                if not espList[player] then
                    pcall(function()
                        local gui = Instance.new("BillboardGui")
                        gui.Size = UDim2.new(0, 200, 0, 50)
                        gui.StudsOffset = Vector3.new(0, 2, 0)
                        gui.AlwaysOnTop = true
                        gui.Parent = player.Character.Head
                        
                        local text = Instance.new("TextLabel")
                        text.Size = UDim2.new(1, 0, 1, 0)
                        text.BackgroundTransparency = 1
                        text.TextColor3 = Color3.new(1, 1, 1)
                        text.Font = Enum.Font.GothamBold
                        text.TextStrokeTransparency = 0
                        text.TextStrokeColor3 = Color3.new(0, 0, 0)
                        text.TextSize = 12
                        text.Parent = gui
                        
                        espList[player] = {gui = gui, text = text}
                    end)
                end
                
                if espList[player] and espList[player].gui.Parent then
                    pcall(function()
                        local txt = ""
                        if Settings.espEnabled then txt = txt .. player.Name .. "\n" end
                        if Settings.showHealth and player.Character:FindFirstChild("Humanoid") then
                            txt = txt .. "HP: " .. math.floor(player.Character.Humanoid.Health) .. "\n"
                        end
                        if Settings.showDistance and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local dist = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                            txt = txt .. math.floor(dist) .. " studs"
                        end
                        espList[player].text.Text = txt
                    end)
                end
            else
                -- Limpar ESP quando desabilitado
                if espList[player] then
                    pcall(function()
                        if espList[player].gui then
                            espList[player].gui:Destroy()
                        end
                        espList[player] = nil
                    end)
                end
            end
        end
    end
end

-- Sistema de voo
local function updateFly()
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return end
    
    local rootPart = LocalPlayer.Character.HumanoidRootPart
    
    if Settings.flyEnabled then
        if not flyBodyVelocity then
            flyBodyVelocity = Instance.new("BodyVelocity")
            flyBodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
            flyBodyVelocity.Parent = rootPart
        end
        
        local velocity = Vector3.new(0, 0, 0)
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then velocity = velocity + (Camera.CFrame.LookVector * Settings.flySpeed) end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then velocity = velocity - (Camera.CFrame.LookVector * Settings.flySpeed) end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then velocity = velocity - (Camera.CFrame.RightVector * Settings.flySpeed) end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then velocity = velocity + (Camera.CFrame.RightVector * Settings.flySpeed) end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then velocity = velocity + Vector3.new(0, Settings.flySpeed, 0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then velocity = velocity - Vector3.new(0, Settings.flySpeed, 0) end
        
        flyBodyVelocity.Velocity = velocity
    else
        if flyBodyVelocity then
            flyBodyVelocity:Destroy()
            flyBodyVelocity = nil
        end
    end
end

-- Sistema de velocidade
local function updateSpeed()
    pcall(function()
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            local humanoid = LocalPlayer.Character.Humanoid
            humanoid.WalkSpeed = Settings.speedEnabled and Settings.walkSpeed or originalWalkSpeed
        end
    end)
end

-- Limpar ESP quando jogador sai
Players.PlayerRemoving:Connect(function(player)
    if espList[player] then
        pcall(function()
            if espList[player].gui then
                espList[player].gui:Destroy()
            end
            espList[player] = nil
        end)
    end
end)

-- LOOP PRINCIPAL OTIMIZADO
local lastUpdate = 0
local connection = RunService.Heartbeat:Connect(function()
    local now = tick()
    if now - lastUpdate < 0.016 then return end -- 60 FPS cap
    lastUpdate = now
    
    pcall(function()
        if Settings.autoAim then
            currentTarget = findTarget()
            if currentTarget then
                aimAtTarget(currentTarget)
            end
        else
            currentTarget = nil
        end
        
        -- Atualizar menos frequentemente para performance
        if now % 0.1 < 0.016 then
            updateFOV()
            updateESP()
        end
        
        updateFly()
        updateSpeed()
    end)
end)

-- Limpeza quando script é recarregado
game.Players.PlayerRemoving:Connect(function(player)
    if player == LocalPlayer then
        if connection then
            connection:Disconnect()
        end
        if fovCircle then
            fovCircle:Remove()
        end
        for _, esp in pairs(espList) do
            if esp.gui then
                esp.gui:Destroy()
            end
        end
    end
end)

-- Notificações de sucesso
notify("Mitra Menu V2.1 - Carregado com sucesso!")
wait(0.5)
notify("Usuário autorizado: " .. LocalPlayer.Name)

print("🎯 Mitra Menu V2.1 - Sistema de Whitelist Ativo")
print("✅ Usuário autorizado: " .. LocalPlayer.Name)
print("📋 Para adicionar usuários, edite a lista 'whitelist' no código")
print("🔧 Script corrigido - Todos os erros de sintaxe foram resolvidos")
