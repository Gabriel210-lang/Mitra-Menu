--[[
    Mitra Menu V2.1 - DrRay UI Library
    Sistema Ultra Otimizado - Aimbot GRUDADO + Exploits Seguros
    VERSÃO CORRIGIDA - Sliders funcionando + ESP auto-update + Aimbot Dinâmico
]]

-- Serviços
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Camera = workspace.CurrentCamera
local Mouse = LocalPlayer:GetMouse()
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Tentar carregar DrRay UI Library com tratamento de erro
local DrRayLibrary = nil
local success, result = pcall(function()
    return loadstring(game:HttpGet("https://raw.githubusercontent.com/AZYsGithub/DrRay-UI-Library/main/DrRay.lua"))()
end)
if success then
    DrRayLibrary = result
else
    warn("Falha ao carregar DrRay UI Library: " .. tostring(result))
    return -- Para a execução se a biblioteca não carregar
end

-- Configurações principais
local S = {
    sa = false, -- Silent Aim
    tc = false, -- Team Check
    wc = false, -- Wall Check
    kc = false, -- Kill Check
    fe = false, -- FOV Circle
    fv = 120,   -- FOV Size
    e = false,  -- ESP Names
    sh = false, -- Show Health
    ss = false, -- Show Distance
    sb = false, -- Show Box (não usado atualmente)
    eh = false, -- Hitbox Expand
    am = false, -- Auto Aim
    smoothness = 0.01, -- Suavidade do aimbot
    fly = false, -- Fly
    flySpeed = 50, -- Velocidade do voo
    speed = false, -- Speed
    walkSpeed = 50, -- Velocidade de caminhada
    autoRob = false, -- Auto Roubar Itens
    autoSearch = false -- Auto Revistar
}

-- Variáveis do sistema
local P, LP, RS, C = Players, LocalPlayer, RunService, Camera
local UIS, TweenS = UserInputService, TweenService
local M = Mouse

-- Variáveis globais
local espList = {}
local fovCircle = nil
local currentTarget = nil
local flyBodyVelocity = nil
local originalWalkSpeed = 16
local autoRobLoop = nil
local autoSearchLoop = nil

-- Sistema de notificação
local function notify(msg)
    local sg = Instance.new("ScreenGui")
    sg.Parent = game.CoreGui
    local f = Instance.new("Frame", sg)
    f.Size, f.Position = UDim2.new(0, 250, 0, 40), UDim2.new(0.5, -125, 0, 20)
    f.BackgroundColor3, f.BorderSizePixel = Color3.fromRGB(30, 30, 40), 0
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 8)
    local t = Instance.new("TextLabel", f)
    t.Size, t.BackgroundTransparency = UDim2.new(1, 0, 1, 0), 1
    t.Text, t.TextColor3, t.Font, t.TextSize = msg, Color3.new(1, 1, 1), Enum.Font.GothamBold, 12
    t.TextXAlignment = Enum.TextXAlignment.Center
    game:GetService("Debris"):AddItem(sg, 2)
end

-- Função para limpar ESP
local function cleanupPlayerESP(player)
    if espList[player] then
        pcall(function()
            if espList[player].gui then
                espList[player].gui:Destroy()
            end
        end)
        espList[player] = nil
    end
end

local function cleanupAllESP()
    for player, data in pairs(espList) do
        pcall(function()
            if data.gui then
                data.gui:Destroy()
            end
        end)
    end
    espList = {}
end

-- Sistema de monitoramento de jogadores
local function setupPlayerMonitoring()
    P.PlayerRemoving:Connect(function(player)
        cleanupPlayerESP(player)
    end)
    
    P.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            task.wait(0.5)
            if espList[player] then
                cleanupPlayerESP(player)
            end
        end)
        
        player.CharacterRemoving:Connect(function(character)
            cleanupPlayerESP(player)
        end)
    end)
    
    LP.CharacterAdded:Connect(function(character)
        task.wait(1)
        updateSpeed()
    end)
end

-- AUTO ROUBAR ITENS
local function toggleAutoRob()
    S.autoRob = not S.autoRob
    
    if S.autoRob then
        autoRobLoop = task.spawn(function()
            local PlayerGui = LP:WaitForChild("PlayerGui")
            local itens = {
                "AK47", "Uzi", "PARAFAL", "Glock 17", "Faca", "IA2", "G3", "Dinamite",
                "Hi Power", "Natalina", "HK416", "Lockpick", "Escudo", "Skate", 
                "Saco de lixo", "Peça de Arma", "Tratamento", "AR-15", "PS5", "C4", "USP", "Xbox"
            }
            
            local args = { [1] = "mudaInv", [2] = "2", [4] = "1" }
            
            while S.autoRob do
                for _, gui in ipairs(PlayerGui:GetChildren()) do
                    if gui.Name == "NotifyGui" and gui:IsA("ScreenGui") then
                        gui:Destroy()
                    end
                end
                
                for i, item in ipairs(itens) do
                    if i <= 16 and S.autoRob then
                        args[3] = item
                        args[2] = tostring(i)
                        
                        task.spawn(function()
                            pcall(function()
                                ReplicatedStorage:WaitForChild("Modules")
                                    :WaitForChild("InvRemotes")
                                    :WaitForChild("InvRequest")
                                    :InvokeServer(unpack(args))
                            end)
                        end)
                    end
                end
                task.wait(0.1)
            end
        end)
        notify("Auto Roubar Itens ON")
    else
        S.autoRob = false
        notify("Auto Roubar Itens OFF")
    end
end

-- AUTO REVISTAR
local function toggleAutoSearch()
    S.autoSearch = not S.autoSearch
    
    if S.autoSearch then
        autoSearchLoop = task.spawn(function()
            local RS = ReplicatedStorage:WaitForChild("RemoteNovos")
            local dsada = RS:WaitForChild("b6173c3a-0f6d-4f0f-9f0b-7e6b7e6b7e6b")
            local DETECTION_RADIUS = 10
            local CheckInterval = 0.5
            local detectados = {}
            
            local function isDead(player)
                if player and player.Character then
                    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
                    return humanoid and humanoid.Health <= 0
                end
                return false
            end
            
            local function distanceBetween(p1, p2)
                return (p1 - p2).Magnitude
            end
            
            while S.autoSearch do
                task.wait(CheckInterval)
                
                local character = LP.Character
                if not character or not character:FindFirstChild("HumanoidRootPart") then continue end
                
                local pos = character.HumanoidRootPart.Position
                
                for _, otherPlayer in ipairs(P:GetPlayers()) do
                    if otherPlayer ~= LP and isDead(otherPlayer) and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        local otherPos = otherPlayer.Character.HumanoidRootPart.Position
                        if distanceBetween(pos, otherPos) <= DETECTION_RADIUS then
                            if not detectados[otherPlayer] then
                                detectados[otherPlayer] = true
                                pcall(function()
                                    dsada:FireServer("/revistar morto")
                                end)
                            end
                        else
                            detectados[otherPlayer] = nil
                        end
                    end
                end
            end
        end)
        notify("Auto Revistar ON")
    else
        S.autoSearch = false
        notify("Auto Revistar OFF")
    end
end

-- FLY + BYPASS
local function createFlySystem()
    local flyEnabled = false
    local moveVector = Vector3.new()
    
    local function updateMoveVector()
        local cam = workspace.CurrentCamera
        local look = cam.CFrame.LookVector
        local right = cam.CFrame.RightVector
        local up = Vector3.new(0, 1, 0)
        
        local forward = 0
        local rightVal = 0
        local upVal = 0
        
        if UIS:IsKeyDown(Enum.KeyCode.W) then forward += 1 end
        if UIS:IsKeyDown(Enum.KeyCode.S) then forward -= 1 end
        if UIS:IsKeyDown(Enum.KeyCode.A) then rightVal -= 1 end
        if UIS:IsKeyDown(Enum.KeyCode.D) then rightVal += 1 end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then upVal += 1 end
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then upVal -= 1 end
        
        local dir = (look * forward + right * rightVal + up * upVal)
        moveVector = dir.Magnitude > 0 and dir.Unit or Vector3.new()
    end
    
    S.fly = not S.fly
    flyEnabled = S.fly
    
    if flyEnabled then
        pcall(function()
            if LP.Character:FindFirstChild("DanoQueda") then
                LP.Character:FindFirstChild("DanoQueda"):Destroy()
            end
        end)
        
        local connection
        connection = RS.RenderStepped:Connect(function()
            if not flyEnabled or not LP.Character or not LP.Character:FindFirstChild("HumanoidRootPart") then
                if connection then connection:Disconnect() end
                return
            end
            
            local character = LP.Character
            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            
            if flyEnabled and rootPart and humanoid then
                updateMoveVector()
                local cam = workspace.CurrentCamera
                local _, y, _ = cam.CFrame.Rotation:ToEulerAnglesYXZ()
                rootPart.CFrame = CFrame.new(rootPart.Position) * CFrame.Angles(0, y, 0)
                rootPart.AssemblyLinearVelocity = moveVector * S.flySpeed
                if humanoid.AutoRotate then
                    humanoid.AutoRotate = false
                end
            end
        end)
        notify("Fly + Bypass ON - Use WASD + Space/Ctrl")
    else
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.AutoRotate = true
        end
        notify("Fly OFF")
    end
end

-- Criar UI com DrRay
local window = DrRayLibrary:Load("Mitra Menu V2.1", "Default")

-- Aba Início
local inicioTab = DrRayLibrary.newTab("Início", "rbxassetid://8560362689")
inicioTab.newLabel("🎯 MITRA MENU V2.1")
inicioTab.newLabel("")
inicioTab.newLabel("Executando em: " .. game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name)
inicioTab.newLabel("")
inicioTab.newLabel("Discord: https://discord.gg/mitramenu")
inicioTab.newLabel("")
inicioTab.newLabel("━━━━━━━━━━━━━━━━━━━━━━━━")
inicioTab.newLabel("✅ STATUS DO SISTEMA:")
inicioTab.newLabel("• Aimbot GRUDADO: Pronto")
inicioTab.newLabel("• ESP Through Walls: Ativo")
inicioTab.newLabel("• Exploits Seguros: Online")
inicioTab.newLabel("• Auto Systems: Disponível")
inicioTab.newLabel("━━━━━━━━━━━━━━━━━━━━━━━━")

-- Aba Aimbot
local aimbotTab = DrRayLibrary.newTab("Aimbot", "rbxassetid://8560362689")

aimbotTab.newToggle("Silent Aim", "Ativa o aimbot silencioso GRUDADO", S.sa, function(state)
    S.sa = state
    notify("Silent Aim " .. (state and "ON" or "OFF"))
end)

aimbotTab.newToggle("Team Check", "Verifica equipe", S.tc, function(state)
    S.tc = state
    notify("Team Check " .. (state and "ON" or "OFF"))
end)

aimbotTab.newToggle("Wall Check", "Verifica paredes", S.wc, function(state)
    S.wc = state
    notify("Wall Check " .. (state and "ON" or "OFF"))
end)

aimbotTab.newToggle("Kill Check", "Verifica se está vivo", S.kc, function(state)
    S.kc = state
    notify("Kill Check " .. (state and "ON" or "OFF"))
end)

aimbotTab.newToggle("FOV Circle", "Círculo de mira", S.fe, function(state)
    S.fe = state
    notify("FOV Circle " .. (state and "ON" or "OFF"))
end)

aimbotTab.newSlider("FOV Size", "Tamanho do FOV", 200, 30, S.fv, function(value)
    S.fv = value
    notify("FOV Size: " .. tostring(value))
end)

aimbotTab.newSlider("Smoothness", "Suavidade (0.001 = GRUDADO)", 0.5, 0.001, S.smoothness, function(value)
    S.smoothness = value
    notify("Smoothness: " .. tostring(value))
end)

aimbotTab.newToggle("Auto Aim", "AIMBOT GRUDADO ATIVO", S.am, function(state)
    S.am = state
    notify("Auto Aim " .. (state and "ON - GRUDADO!" or "OFF"))
end)

-- Aba ESP
local espTab = DrRayLibrary.newTab("ESP", "rbxassetid://8560362689")

espTab.newToggle("ESP Names", "Mostra nomes", S.e, function(state)
    S.e = state
    notify("ESP Names " .. (state and "ON" or "OFF"))
    if not state then cleanupAllESP() end
end)

espTab.newToggle("Show Health", "Mostra vida", S.sh, function(state)
    S.sh = state
    notify("Show Health " .. (state and "ON" or "OFF"))
end)

espTab.newToggle("Show Distance", "Mostra distância", S.ss, function(state)
    S.ss = state
    notify("Show Distance " .. (state and "ON" or "OFF"))
end)

espTab.newToggle("Hitbox Expand", "Expande hitbox", S.eh, function(state)
    S.eh = state
    notify("Hitbox " .. (state and "ON" or "OFF"))
end)

-- Aba Exploits
local exploitsTab = DrRayLibrary.newTab("Exploits", "rbxassetid://8560362689")

exploitsTab.newButton("Fly + Bypass", "Voo com bypass de dano", function()
    createFlySystem()
end)

exploitsTab.newSlider("Fly Speed", "Velocidade do voo", 100, 16, S.flySpeed, function(value)
    S.flySpeed = value
    notify("Fly Speed: " .. tostring(value))
end)

exploitsTab.newToggle("Speed", "Velocidade aumentada", S.speed, function(state)
    S.speed = state
    notify("Speed " .. (state and "ON" or "OFF"))
end)

exploitsTab.newSlider("Walk Speed", "Velocidade de caminhada", 150, 16, S.walkSpeed, function(value)
    S.walkSpeed = value
    notify("Walk Speed: " .. tostring(value))
end)

exploitsTab.newButton("Auto Roubar Itens", "Pega itens automaticamente", function()
    toggleAutoRob()
end)

exploitsTab.newButton("Auto Revistar", "Revista mortos automaticamente", function()
    toggleAutoSearch()
end)

-- FUNÇÕES DO AIMBOT (corrigidas)
local function findTarget()
    local closest = nil
    local shortestDistance = math.huge
    local screenCenter = Vector2.new(C.ViewportSize.X / 2, C.ViewportSize.Y / 2)
    
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local character = player.Character
            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character.HumanoidRootPart
            
            if humanoid and rootPart then
                if S.kc and humanoid.Health <= 0 then continue end
                if S.tc and player.Team and LP.Team and player.Team == LP.Team then continue end
                
                local screenPos, onScreen = C:WorldToViewportPoint(rootPart.Position)
                if onScreen then
                    local distance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    if distance <= S.fv and distance < shortestDistance then
                        if S.wc then
                            local rayOrigin = C.CFrame.Position
                            local rayDirection = (rootPart.Position - rayOrigin).Unit * 1000
                            local raycastParams = RaycastParams.new()
                            raycastParams.FilterDescendantsInstances = {LP.Character}
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

local function aimAtTarget(target)
    if not target or not target:FindFirstChild("HumanoidRootPart") then return end
    
    local targetPart = target:FindFirstChild("Head") or target:FindFirstChild("UpperTorso") or target:FindFirstChild("Torso") or target.HumanoidRootPart
    if not targetPart then return end
    
    local targetPos = targetPart.Position
    local cameraPos = C.CFrame.Position
    
    local targetVelocity = Vector3.new(0, 0, 0)
    pcall(function()
        targetVelocity = target.HumanoidRootPart.AssemblyLinearVelocity
    end)
    
    local distance = (targetPos - cameraPos).Magnitude
    local timeToHit = distance / 1000
    local predictedPos = targetPos + (targetVelocity * timeToHit)
    
    local newCFrame = CFrame.new(cameraPos, predictedPos)
    
    if S.smoothness <= 0.001 then
        C.CFrame = newCFrame
    else
        local lerpValue = math.clamp(1 - (S.smoothness * 10), 0.1, 1)
        C.CFrame = C.CFrame:Lerp(newCFrame, lerpValue)
    end
end

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
            fovCircle.Position = Vector2.new(C.ViewportSize.X / 2, C.ViewportSize.Y / 2)
            fovCircle.Radius = S.fv
            fovCircle.Visible = true
            fovCircle.Color = currentTarget and Color3.new(1, 0.2, 0.2) or Color3.new(0.2, 1, 0.2)
        end
    elseif fovCircle then
        fovCircle.Visible = false
    end
end

local function updateESP()
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP then
            if player.Character and player.Character:FindFirstChild("Head") then
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
                            
                            espList[player] = { gui = gui, text = text }
                        end)
                    end
                    
                    if espList[player] and espList[player].gui.Parent then
                        pcall(function()
                            local txt = ""
                            if S.e then txt = txt .. player.Name .. "\n" end
                            if S.sh and player.Character:FindFirstChild("Humanoid") then
                                txt = txt .. "HP: " .. math.floor(player.Character.Humanoid.Health) .. "\n"
                            end
                            if S.ss and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                                local dist = (player.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                                txt = txt .. math.floor(dist) .. " studs"
                            end
                            espList[player].text.Text = txt
                            
                            if S.ss and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                                local dist = (player.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                                if dist < 50 then
                                    espList[player].text.TextColor3 = Color3.new(1, 0.2, 0.2)
                                elseif dist < 100 then
                                    espList[player].text.TextColor3 = Color3.new(1, 1, 0.2)
                                else
                                    espList[player].text.TextColor3 = Color3.new(0.2, 1, 0.2)
                                end
                            else
                                espList[player].text.TextColor3 = Color3.new(1, 1, 1)
                            end
                        end)
                    end
                else
                    cleanupPlayerESP(player)
                end
            else
                cleanupPlayerESP(player)
            end
        end
    end
end

local function updateHitbox()
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local root = player.Character.HumanoidRootPart
            if S.eh then
                root.Size = Vector3.new(8, 8, 8)
                root.Transparency = 0.7
                root.CanCollide = false
            else
                root.Size = Vector3.new(2, 2, 1)
                root.Transparency = 1
            end
        end
    end
end

function updateSpeed()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            local humanoid = LP.Character.Humanoid
            humanoid.WalkSpeed = S.speed and S.walkSpeed or originalWalkSpeed
        end
    end)
end

-- Inicializar sistema
setupPlayerMonitoring()

-- LOOP PRINCIPAL
local lastUpdate = 0
RS.Heartbeat:Connect(function(deltaTime)
    local now = tick()
    if now - lastUpdate < 0.016 then return end
    lastUpdate = now
    
    pcall(function()
        if S.am then
            currentTarget = findTarget()
            if currentTarget then
                aimAtTarget(currentTarget)
            else
                currentTarget = nil
            end
        else
            currentTarget = nil
        end
        
        if now % 0.1 < 0.016 then
            updateFOV()
            updateESP()
            updateHitbox()
        end
        
        updateSpeed()
    end)
end)

notify("Mitra Menu V2.1 - Sistema Completo Carregado!")
print("🎯 Mitra Menu V2.1 - TODAS as funções ativas!")
print("• Aimbot GRUDADO Ultra Preciso")
print("• ESP Through Walls com auto-update")
print("• Fly + Bypass dano de queda")
print("• Auto Roubar Itens")
print("• Auto Revistar mortos")
print("• Sistema anti-lag otimizado")
