--[[
    Mitra Menu V2.1 - RESUMIDO COM WHITELIST POR NICK
    Sistema Ultra Otimizado - Aimbot + ESP + Exploits
]]

-- SISTEMA DE WHITELIST POR NICKNAME
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

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
        if nick:lower() == username:lower() then
            return true
        end
    end
    return false
end

-- Se não estiver na whitelist, mostrar aviso e parar
if not isWhitelisted(LocalPlayer.Name) then
    local sg = Instance.new("ScreenGui")
    sg.Name = "WhitelistWarning"
    sg.Parent = game.CoreGui
    sg.ResetOnSpawn = false
    
    local f = Instance.new("Frame", sg)
    f.Size = UDim2.new(0, 450, 0, 180)
    f.Position = UDim2.new(0.5, -225, 0, 50)
    f.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    f.BorderSizePixel = 0
    
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 10)
    
    -- Título
    local title = Instance.new("TextLabel", f)
    title.Size = UDim2.new(1, 0, 0, 40)
    title.BackgroundTransparency = 1
    title.Text = "🚫 ACESSO NEGADO"
    title.TextColor3 = Color3.fromRGB(255, 80, 80)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 18
    title.TextXAlignment = Enum.TextXAlignment.Center
    
    -- Mensagem principal
    local msg = Instance.new("TextLabel", f)
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
    
    -- Info do usuário
    local userInfo = Instance.new("TextLabel", f)
    userInfo.Size = UDim2.new(1, -20, 0, 25)
    userInfo.Position = UDim2.new(0, 10, 0, 110)
    userInfo.BackgroundTransparency = 1
    userInfo.Text = "Usuário atual: " .. LocalPlayer.Name
    userInfo.TextColor3 = Color3.fromRGB(200, 200, 200)
    userInfo.Font = Enum.Font.Gotham
    userInfo.TextSize = 12
    userInfo.TextXAlignment = Enum.TextXAlignment.Center
    
    -- Discord
    local discord = Instance.new("TextLabel", f)
    discord.Size = UDim2.new(1, 0, 0, 25)
    discord.Position = UDim2.new(0, 0, 0, 135)
    discord.BackgroundTransparency = 1
    discord.Text = "Discord: discord.gg/mitramenu"
    discord.TextColor3 = Color3.fromRGB(100, 150, 255)
    discord.Font = Enum.Font.GothamBold
    discord.TextSize = 12
    discord.TextXAlignment = Enum.TextXAlignment.Center
    
    -- Botão fechar
    local close = Instance.new("TextButton", f)
    close.Size = UDim2.new(0, 80, 0, 25)
    close.Position = UDim2.new(1, -90, 0, 10)
    close.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    close.Text = "X"
    close.TextColor3 = Color3.new(1, 1, 1)
    close.Font = Enum.Font.GothamBold
    close.TextSize = 14
    close.BorderSizePixel = 0
    
    Instance.new("UICorner", close).CornerRadius = UDim.new(0, 5)
    
    close.MouseButton1Click:Connect(function()
        sg:Destroy()
    end)
    
    -- Auto-close após 10 segundos
    game:GetService("Debris"):AddItem(sg, 10)
    
    return -- Para o script aqui se não estiver na whitelist
end

-- SCRIPT PRINCIPAL CONTINUA (usuário autorizado)
local RS = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()

-- DrRay UI Library
local DrRayLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/AZYsGithub/DrRay-UI-Library/main/DrRay.lua"))()

-- Configurações
local S = {
    sa=false, tc=false, wc=false, kc=false, fe=false, fv=120,
    e=false, sh=false, ss=false, eh=false, am=false, smoothness=0.01,
    fly=false, flySpeed=50, speed=false, walkSpeed=50
}

-- Variáveis globais
local espList = {}
local fovCircle = nil
local currentTarget = nil
local flyBodyVelocity = nil
local originalWalkSpeed = 16

-- Sistema de notificação
local function notify(msg)
    local sg = Instance.new("ScreenGui")
    sg.Parent = game.CoreGui
    local f = Instance.new("Frame", sg)
    f.Size = UDim2.new(0, 250, 0, 40)
    f.Position = UDim2.new(0.5, -125, 0, 20)
    f.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    f.BorderSizePixel = 0
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 8)
    local t = Instance.new("TextLabel", f)
    t.Size = UDim2.new(1, 0, 1, 0)
    t.BackgroundTransparency = 1
    t.Text = msg
    t.TextColor3 = Color3.new(1, 1, 1)
    t.Font = Enum.Font.GothamBold
    t.TextSize = 12
    t.TextXAlignment = Enum.TextXAlignment.Center
    game:GetService("Debris"):AddItem(sg, 2)
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

aimbotTab.newToggle("Silent Aim", "Ativa aimbot silencioso", S.sa, function(v)
    S.sa = v
    notify("Silent Aim " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("Team Check", "Verifica equipe", S.tc, function(v)
    S.tc = v
    notify("Team Check " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("Wall Check", "Verifica paredes", S.wc, function(v)
    S.wc = v
    notify("Wall Check " .. (v and "ON" or "OFF"))
end)

aimbotTab.newToggle("FOV Circle", "Círculo de mira", S.fe, function(v)
    S.fe = v
    notify("FOV Circle " .. (v and "ON" or "OFF"))
end)

aimbotTab.newSlider("FOV Size", "Tamanho do FOV", 200, 30, S.fv, function(v)
    S.fv = v
    notify("FOV: " .. v)
end)

aimbotTab.newSlider("Smoothness", "Suavidade (0.001 = GRUDADO)", 0.5, 0.001, S.smoothness, function(v)
    S.smoothness = v
    notify("Smoothness: " .. v)
end)

aimbotTab.newToggle("Auto Aim", "AIMBOT GRUDADO", S.am, function(v)
    S.am = v
    notify("Auto Aim " .. (v and "GRUDADO!" or "OFF"))
end)

-- Aba ESP
local espTab = DrRayLibrary.newTab("ESP", "rbxassetid://8560362689")

espTab.newToggle("ESP Names", "Mostra nomes", S.e, function(v)
    S.e = v
    notify("ESP Names " .. (v and "ON" or "OFF"))
end)

espTab.newToggle("Show Health", "Mostra vida", S.sh, function(v)
    S.sh = v
    notify("Show Health " .. (v and "ON" or "OFF"))
end)

espTab.newToggle("Show Distance", "Mostra distância", S.ss, function(v)
    S.ss = v
    notify("Show Distance " .. (v and "ON" or "OFF"))
end)

-- Aba Exploits
local exploitsTab = DrRayLibrary.newTab("Exploits", "rbxassetid://8560362689")

exploitsTab.newToggle("Fly", "Modo voo", S.fly, function(v)
    S.fly = v
    notify("Fly " .. (v and "ON" or "OFF"))
end)

exploitsTab.newSlider("Fly Speed", "Velocidade do voo", 100, 16, S.flySpeed, function(v)
    S.flySpeed = v
    notify("Fly Speed: " .. v)
end)

exploitsTab.newToggle("Speed", "Velocidade aumentada", S.speed, function(v)
    S.speed = v
    notify("Speed " .. (v and "ON" or "OFF"))
end)

exploitsTab.newSlider("Walk Speed", "Velocidade de caminhada", 100, 16, S.walkSpeed, function(v)
    S.walkSpeed = v
    notify("Walk Speed: " .. v)
end)

-- FUNÇÕES PRINCIPAIS

-- Encontrar alvo
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
                if S.kc and humanoid.Health <= 0 then continue end
                if S.tc and player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team then continue end
                
                local screenPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
                if onScreen then
                    local distance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    if distance <= S.fv and distance < shortestDistance then
                        if S.wc then
                            local rayOrigin = Camera.CFrame.Position
                            local rayDirection = (rootPart.Position - rayOrigin)
                            local raycastParams = RaycastParams.new()
                            raycastParams.FilterDescendantsInstances = {LocalPlayer.Character}
                            raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
                            local result = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
                            if result and not result.Instance:IsDescendantOf(character) then
                                continue
                            end
                        end
                        closest = character
                        shortestDistance = distance
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
    
    if S.smoothness <= 0.01 then
        Camera.CFrame = newCFrame
    else
        local lerpValue = math.min(1, (1 - S.smoothness) * 10)
        Camera.CFrame = Camera.CFrame:Lerp(newCFrame, lerpValue)
    end
end

-- Atualizar FOV
local function updateFOV()
    if S.fe then
        if not fovCircle then
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
            fovCircle.Radius = S.fv
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
            if S.e or S.sh or S.ss then
                if not espList[player] then
                    pcall(function()
                        local gui = Instance.new("BillboardGui")
                        gui.Size = UDim2.new(0, 200, 0, 50)
                        gui.StudsOffset = Vector3.new(0, 2, 0)
                        gui.AlwaysOnTop = true
                        gui.Parent = player.Character.Head
                        
                        local text = Instance.new("TextLabel", gui)
                        text.Size = UDim2.new(1, 0, 1, 0)
                        text.BackgroundTransparency = 1
                        text.TextColor3 = Color3.new(1, 1, 1)
                        text.Font = Enum.Font.GothamBold
                        text.TextStrokeTransparency = 0
                        text.TextStrokeColor3 = Color3.new(0, 0, 0)
                        text.TextSize = 12
                        
                        espList[player] = {gui = gui, text = text}
                    end)
                end
                
                if espList[player] and espList[player].gui.Parent then
                    pcall(function()
                        local txt = ""
                        if S.e then txt = txt .. player.Name .. "\n" end
                        if S.sh and player.Character:FindFirstChild("Humanoid") then
                            txt = txt .. "HP: " .. math.floor(player.Character.Humanoid.Health) .. "\n"
                        end
                        if S.ss and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local dist = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                            txt = txt .. math.floor(dist) .. " studs"
                        end
                        espList[player].text.Text = txt
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
    
    if S.fly then
        if not flyBodyVelocity then
            flyBodyVelocity = Instance.new("BodyVelocity")
            flyBodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000)
            flyBodyVelocity.Parent = rootPart
        end
        
        local velocity = Vector3.new(0, 0, 0)
        if UIS:IsKeyDown(Enum.KeyCode.W) then velocity = velocity + (Camera.CFrame.LookVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.S) then velocity = velocity - (Camera.CFrame.LookVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.A) then velocity = velocity - (Camera.CFrame.RightVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.D) then velocity = velocity + (Camera.CFrame.RightVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then velocity = velocity + Vector3.new(0, S.flySpeed, 0) end
        if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then velocity = velocity - Vector3.new(0, S.flySpeed, 0) end
        
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
            humanoid.WalkSpeed = S.speed and S.walkSpeed or originalWalkSpeed
        end
    end)
end

-- LOOP PRINCIPAL
local lastUpdate = 0
RS.Heartbeat:Connect(function()
    local now = tick()
    if now - lastUpdate < 0.016 then return end
    lastUpdate = now
    
    pcall(function()
        if S.am then
            currentTarget = findTarget()
            if currentTarget then
                aimAtTarget(currentTarget)
            end
        else
            currentTarget = nil
        end
        
        if now % 0.1 < 0.016 then
            updateFOV()
            updateESP()
        end
        
        updateFly()
        updateSpeed()
    end)
end)

-- Notificação de sucesso
notify("Mitra Menu V2.1 - Carregado com sucesso!")
notify("Usuário autorizado: " .. LocalPlayer.Name)

print("🎯 Mitra Menu V2.1 - Sistema de Whitelist Ativo")
print("✅ Usuário autorizado: " .. LocalPlayer.Name)
print("📋 Para adicionar usuários, edite a lista 'whitelist' no código")
