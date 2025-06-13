--[[
    Mitra Menu - Sistema de Cheat Otimizado
    Melhor Menu da atualidade! J & M
]]

local P,LP,RS,C = game:GetService("Players"),game:GetService("Players").LocalPlayer,game:GetService("RunService"),workspace.CurrentCamera
local M = LP:GetMouse()
local UIS = game:GetService("UserInputService")

-- Verificação de segurança
if not LP or not LP.Character then
    warn("Player ou Character não encontrado!")
    return
end

-- Configurações otimizadas
local S = {
    sa=false,   -- Silent Aim
    tc=false,   -- Team Check
    wc=false,   -- Wall Check
    kc=false,   -- Kill Check
    fe=false,   -- FOV Circle
    fv=100,     -- FOV Value
    e=false,    -- ESP Names
    sh=false,   -- Show Health
    ss=false,   -- Show Distance
    sb=false,   -- Show Box
    eh=false,   -- Expand Hitbox
    am=false,   -- Auto Aim
    smoothness=0.25  -- Suavidade da mira (0.1 = muito suave, 1.0 = instantâneo)
}

-- Variáveis do sistema de aimbot
local aimbot = {
    enabled = false,
    target = nil,
    lastTarget = nil,
    smoothing = true,
    maxDistance = 500,
    updateRate = 1/60, -- 60 FPS
    lockOn = false -- Variável para manter o lock no alvo
}

-- Sistema de notificação
local function notify(msg)
    pcall(function()
        local sg = Instance.new("ScreenGui")
        sg.Name = "Notification"
        sg.Parent = game.CoreGui
        
        local f = Instance.new("Frame",sg)
        f.Size,f.Position = UDim2.new(0,300,0,50),UDim2.new(0.5,-150,0,20)
        f.BackgroundColor3,f.BorderSizePixel = Color3.fromRGB(40,40,50),0
        
        local c = Instance.new("UICorner",f)
        c.CornerRadius = UDim.new(0,8)
        
        local t = Instance.new("TextLabel",f)
        t.Size,t.BackgroundTransparency = UDim2.new(1,0,1,0),1
        t.Text,t.TextColor3,t.Font,t.TextSize = msg,Color3.new(1,1,1),Enum.Font.GothamBold,14
        t.TextXAlignment = Enum.TextXAlignment.Center
        
        game:GetService("Debris"):AddItem(sg,3)
    end)
end

-- GUI Base com proteção
local sg = Instance.new("ScreenGui")
sg.Name = "LxLc_"..math.random(1000,9999)
sg.ResetOnSpawn = false

-- Proteção contra detecção
pcall(function()
    sg.Parent = game.CoreGui
end)

if not sg.Parent then
    sg.Parent = LP.PlayerGui
    notify("Usando PlayerGui (menos seguro)")
end

-- Botão minimizado (draggable)
local ob = Instance.new("TextButton",sg)
ob.Size,ob.Position,ob.Text = UDim2.new(0,120,0,30),UDim2.new(0,10,0,10),"Open Menu"
ob.BackgroundColor3,ob.TextColor3,ob.Font = Color3.fromRGB(30,30,30),Color3.new(1,1,1),Enum.Font.GothamBold
ob.BorderSizePixel = 0
local obc = Instance.new("UICorner",ob)
obc.CornerRadius = UDim.new(0,8)

-- Sistema de arrastar otimizado
local dragging = false
local dragStart = nil
local startPos = nil

ob.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = Vector2.new(input.Position.X, input.Position.Y)
        startPos = ob.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local currentPos = Vector2.new(input.Position.X, input.Position.Y)
        local delta = currentPos - dragStart
        ob.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- Interface principal
local mf = Instance.new("Frame",sg)
mf.Size,mf.Position,mf.Visible = UDim2.new(0,400,0,350),UDim2.new(0.5,-200,0.5,-175),false
mf.BackgroundColor3,mf.Active,mf.Draggable = Color3.fromRGB(30,30,40),true,true
mf.AnchorPoint = Vector2.new(0.5,0.5)
local mc = Instance.new("UICorner",mf)
mc.CornerRadius = UDim.new(0,10)

-- Header
local header = Instance.new("Frame",mf)
header.Size,header.Position = UDim2.new(1,0,0,35),UDim2.new(0,0,0,0)
header.BackgroundColor3 = Color3.fromRGB(40,40,50)
local hc = Instance.new("UICorner",header)
hc.CornerRadius = UDim.new(0,10)

local title = Instance.new("TextLabel",header)
title.Size,title.Position = UDim2.new(0.7,0,1,0),UDim2.new(0,10,0,0)
title.BackgroundTransparency,title.Text = 1,"LxLc Menu V2.0 - Fixed Aimbot"
title.TextColor3,title.Font,title.TextSize = Color3.new(1,1,1),Enum.Font.GothamBold,16
title.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle
local mb = Instance.new("TextButton",header)
mb.Size,mb.Position,mb.Text = UDim2.new(0,25,0,25),UDim2.new(1,-60,0,5),"-"
mb.BackgroundColor3,mb.TextColor3,mb.Font,mb.TextSize = Color3.fromRGB(100,100,100),Color3.new(1,1,1),Enum.Font.GothamBold,20
local mbc = Instance.new("UICorner",mb)
mbc.CornerRadius = UDim.new(1,0)

local cb = Instance.new("TextButton",header)
cb.Size,cb.Position,cb.Text = UDim2.new(0,25,0,25),UDim2.new(1,-30,0,5),"X"
cb.BackgroundColor3,cb.TextColor3,cb.Font,cb.TextSize = Color3.fromRGB(180,60,60),Color3.new(1,1,1),Enum.Font.GothamBold,16
local cbc = Instance.new("UICorner",cb)
cbc.CornerRadius = UDim.new(1,0)

-- Sistema de abas
local tb = Instance.new("Frame",mf)
tb.Size,tb.Position,tb.BackgroundTransparency = UDim2.new(1,0,0,35),UDim2.new(0,0,0,40),1

local function createTab(name,x)
    local b = Instance.new("TextButton",tb)
    b.Size,b.Position,b.Text = UDim2.new(0,95,1,0),UDim2.new(0,x,0,0),name
    b.BackgroundColor3,b.TextColor3,b.Font = Color3.fromRGB(50,50,60),Color3.new(1,1,1),Enum.Font.Gotham
    local bc = Instance.new("UICorner",b)
    bc.CornerRadius = UDim.new(0,5)
    return b
end

local tabs,tabFrames = {"Aimbot","ESP","Misc","Info"},{}
for i,name in ipairs(tabs) do
    local button = createTab(name,(i-1)*100)
    local frame = Instance.new("Frame",mf)
    frame.Size,frame.Position,frame.Visible = UDim2.new(1,-20,1,-85),UDim2.new(0,10,0,80),false
    frame.BackgroundTransparency = 1
    tabFrames[name] = frame
    
    button.MouseButton1Click:Connect(function()
        for _,v in pairs(tabFrames) do v.Visible = false end
        for _,v in pairs(tb:GetChildren()) do 
            if v:IsA("TextButton") then 
                v.BackgroundColor3 = Color3.fromRGB(50,50,60) 
            end 
        end
        frame.Visible = true
        button.BackgroundColor3 = Color3.fromRGB(120,50,180)
    end)
end

-- Função Toggle otimizada
local function createToggle(parent,text,key,yPos)
    local frame = Instance.new("Frame",parent)
    frame.Size,frame.Position,frame.BackgroundTransparency = UDim2.new(1,0,0,30),UDim2.new(0,0,0,yPos),1
    
    local label = Instance.new("TextLabel",frame)
    label.Size,label.Text,label.TextColor3,label.BackgroundTransparency = UDim2.new(0.65,0,1,0),text,Color3.new(1,1,1),1
    label.Font,label.TextXAlignment,label.TextSize = Enum.Font.Gotham,Enum.TextXAlignment.Left,12
    
    local toggle = Instance.new("TextButton",frame)
    toggle.Size,toggle.Position,toggle.Text = UDim2.new(0,60,0,25),UDim2.new(0.7,0,0,2),"OFF"
    toggle.BackgroundColor3,toggle.TextColor3,toggle.Font,toggle.TextSize = Color3.fromRGB(100,100,100),Color3.new(1,1,1),Enum.Font.GothamBold,12
    local tc = Instance.new("UICorner",toggle)
    tc.CornerRadius = UDim.new(0,5)
    
    local function updateToggle()
        if S[key] then
            toggle.Text,toggle.BackgroundColor3 = "ON",Color3.fromRGB(100,180,100)
        else
            toggle.Text,toggle.BackgroundColor3 = "OFF",Color3.fromRGB(100,100,100)
        end
    end
    
    toggle.MouseButton1Click:Connect(function()
        S[key] = not S[key]
        updateToggle()
        
        -- Atualizar sistema de aimbot
        if key == "am" then
            aimbot.enabled = S[key]
            if not S[key] then
                aimbot.target = nil
                aimbot.lastTarget = nil
                aimbot.lockOn = false
            end
        end
        
        notify(text..(S[key] and " ativado!" or " desativado!"))
    end)
    
    updateToggle()
    return frame
end

-- Crear slider para FOV e Smoothness
local function createSlider(parent, text, key, minVal, maxVal, yPos)
    local frame = Instance.new("Frame", parent)
    frame.Size, frame.Position, frame.BackgroundTransparency = UDim2.new(1,0,0,40), UDim2.new(0,0,0,yPos), 1
    
    local label = Instance.new("TextLabel", frame)
    label.Size, label.Text, label.TextColor3, label.BackgroundTransparency = UDim2.new(1,0,0,15), text..": "..S[key], Color3.new(1,1,1), 1
    label.Font, label.TextXAlignment, label.TextSize = Enum.Font.Gotham, Enum.TextXAlignment.Left, 12
    
    local sliderFrame = Instance.new("Frame", frame)
    sliderFrame.Size, sliderFrame.Position = UDim2.new(0.8,0,0,15), UDim2.new(0,0,0,20)
    sliderFrame.BackgroundColor3, sliderFrame.BorderSizePixel = Color3.fromRGB(60,60,70), 0
    local sliderCorner = Instance.new("UICorner", sliderFrame)
    sliderCorner.CornerRadius = UDim.new(0,8)
    
    local sliderButton = Instance.new("TextButton", sliderFrame)
    sliderButton.Size, sliderButton.Text = UDim2.new(0,20,1,0), ""
    sliderButton.BackgroundColor3, sliderButton.BorderSizePixel = Color3.fromRGB(120,50,180), 0
    local buttonCorner = Instance.new("UICorner", sliderButton)
    buttonCorner.CornerRadius = UDim.new(1,0)
    
    local draggingSlider = false
    
    local function updateSlider()
        local percentage = (S[key] - minVal) / (maxVal - minVal)
        sliderButton.Position = UDim2.new(percentage * (1 - sliderButton.Size.X.Scale), 0, 0, 0)
        if key == "smoothness" then
            label.Text = text..": "..string.format("%.2f", S[key])
        else
            label.Text = text..": "..S[key]
        end
    end
    
    sliderButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            draggingSlider = true
        end
    end)
    
    UIS.InputChanged:Connect(function(input)
        if draggingSlider and input.UserInputType == Enum.UserInputType.MouseMovement then
            local mouseX = M.X
            local frameX = sliderFrame.AbsolutePosition.X
            local frameWidth = sliderFrame.AbsoluteSize.X
            local buttonWidth = sliderButton.AbsoluteSize.X
            
            local relativeX = math.clamp((mouseX - frameX) / (frameWidth - buttonWidth), 0, 1)
            if key == "smoothness" then
                S[key] = minVal + (maxVal - minVal) * relativeX
            else
                S[key] = math.floor(minVal + (maxVal - minVal) * relativeX)
            end
            updateSlider()
        end
    end)
    
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            draggingSlider = false
        end
    end)
    
    updateSlider()
    return frame
end

-- Crear toggles para cada aba
local aimbotTab = tabFrames["Aimbot"]
createToggle(aimbotTab,"Silent Aim","sa",10)
createToggle(aimbotTab,"Team Check","tc",45)
createToggle(aimbotTab,"Wall Check","wc",80)
createToggle(aimbotTab,"Kill Check","kc",115)
createToggle(aimbotTab,"FOV Circle","fe",150)
createSlider(aimbotTab,"FOV Size","fv",50,300,185)
createSlider(aimbotTab,"Smoothness","smoothness",0.05,1.0,230)
createToggle(aimbotTab,"Auto Aim","am",275)

local espTab = tabFrames["ESP"]
createToggle(espTab,"ESP Names","e",10)
createToggle(espTab,"Health","sh",45)
createToggle(espTab,"Distance","ss",80)
createToggle(espTab,"Box","sb",115)

local miscTab = tabFrames["Misc"]
createToggle(miscTab,"Hitbox","eh",10)

-- Info Tab
local infoTab = tabFrames["Info"]
local infoText = Instance.new("TextLabel",infoTab)
infoText.Size,infoText.Position = UDim2.new(1,-10,1,-10),UDim2.new(0,5,0,5)
infoText.BackgroundTransparency,infoText.TextColor3 = 1,Color3.new(1,1,1)
infoText.Font,infoText.TextSize = Enum.Font.Gotham,12
infoText.TextXAlignment,infoText.TextYAlignment = Enum.TextXAlignment.Left,Enum.TextYAlignment.Top
infoText.Text = [[LxLc Menu V2.0 - AIMBOT CORRIGIDO

CORREÇÃO IMPLEMENTADA:
✓ Mira agora GRUDA automaticamente nos jogadores
✓ Sistema de lock-on melhorado
✓ Detecção de alvos mais precisa
✓ Movimento suave e natural
✓ Zero bugs de travamento

COMO USAR:
1. Ative "FOV Circle" para ver a área de mira
2. Ajuste "FOV Size" para o tamanho desejado
3. Configure "Smoothness" (0.05 = muito suave, 1.0 = rápido)
4. Ative "Auto Aim" para mira automática
5. A mira GRUDARÁ em jogadores dentro do FOV

CONFIGURAÇÕES:
• Team Check - Ignora aliados
• Wall Check - Só mira sem obstáculos  
• Kill Check - Ignora jogadores mortos
• Silent Aim - Mira invisível
• Smoothness - Velocidade da mira

MELHORIAS:
✓ Sistema de priorização de alvos
✓ Lock-on automático
✓ Mira mais responsiva
✓ Performance otimizada
✓ Detecção inteligente]]

-- Variáveis globais otimizadas
local espList = {}
local fovCircle = nil
local lastUpdate = 0
local updateInterval = 1/120 -- 120 FPS para suavidade máxima

-- Função para calcular centro da tela
local function getScreenCenter()
    local viewportSize = C.ViewportSize
    return Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
end

-- Sistema de detecção de alvos otimizado
local function isValidTarget(player)
    if not player or not player.Character then return false end
    
    local character = player.Character
    local humanoid = character:FindFirstChild("Humanoid")
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    
    if not humanoid or not rootPart then return false end
    
    -- Kill Check
    if S.kc and humanoid.Health <= 0 then return false end
    
    -- Team Check
    if S.tc and player.Team and LP.Team and player.Team == LP.Team then return false end
    
    return true
end

-- Sistema de verificação de parede otimizado
local function canSeeTarget(targetPosition)
    if not S.wc then return true end
    
    local success, result = pcall(function()
        local camera = workspace.CurrentCamera
        local rayOrigin = camera.CFrame.Position
        local rayDirection = (targetPosition - rayOrigin).Unit * (targetPosition - rayOrigin).Magnitude
        
        local raycastParams = RaycastParams.new()
        raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
        raycastParams.FilterDescendantsInstances = {LP.Character}
        
        local raycastResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
        return raycastResult == nil
    end)
    
    return success and result
end

-- Sistema de busca de alvo otimizado com priorização
local function findBestTarget()
    local bestTarget = nil
    local shortestDistance = math.huge
    local screenCenter = getScreenCenter()
    local validTargets = {}
    
    -- Primeiro, colete todos os alvos válidos
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP and isValidTarget(player) then
            local character = player.Character
            local rootPart = character.HumanoidRootPart
            local head = character:FindFirstChild("Head")
            
            if rootPart and head then
                -- Verificar se está na tela
                local screenPos, onScreen = C:WorldToViewportPoint(rootPart.Position)
                if onScreen then
                    -- Calcular distância do centro da tela
                    local screenDistance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    
                    -- Verificar se está dentro do FOV
                    if screenDistance <= S.fv then
                        -- Verificar parede se necessário
                        if canSeeTarget(head.Position) then
                            table.insert(validTargets, {
                                character = character,
                                distance = screenDistance,
                                worldDistance = (rootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                            })
                        end
                    end
                end
            end
        end
    end
    
    -- Se há alvo atual e ainda é válido, priorize-o (lock-on)
    if aimbot.target and aimbot.lockOn then
        for _, targetData in pairs(validTargets) do
            if targetData.character == aimbot.target then
                return aimbot.target -- Manter lock no alvo atual
            end
        end
    end
    
    -- Escolher o alvo mais próximo do centro da tela
    for _, targetData in pairs(validTargets) do
        if targetData.distance < shortestDistance then
            bestTarget = targetData.character
            shortestDistance = targetData.distance
        end
    end
    
    return bestTarget
end

-- Sistema de mira suave ultra otimizado - CORRIGIDO
local function smoothAimToTarget(targetPosition)
    if not targetPosition then return end
    
    local camera = workspace.CurrentCamera
    local currentCFrame = camera.CFrame
    
    -- Calcular a direção para o alvo
    local direction = (targetPosition - currentCFrame.Position).Unit
    local targetCFrame = CFrame.lookAt(currentCFrame.Position, targetPosition)
    
    -- Aplicar interpolação suave
    local newCFrame = currentCFrame:Lerp(targetCFrame, S.smoothness)
    
    -- Aplicar a nova rotação da câmera
    camera.CFrame = newCFrame
end

-- Sistema ESP otimizado
local function updateESP()
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("Head") then
            if S.e then
                if not espList[player] then
                    pcall(function()
                        local billboardGui = Instance.new("BillboardGui")
                        billboardGui.Size = UDim2.new(0,200,0,50)
                        billboardGui.StudsOffset = Vector3.new(0,2,0)
                        billboardGui.Parent = player.Character.Head
                        
                        local textLabel = Instance.new("TextLabel",billboardGui)
                        textLabel.Size = UDim2.new(1,0,1,0)
                        textLabel.BackgroundTransparency = 1
                        textLabel.TextColor3 = Color3.new(1,1,1)
                        textLabel.Font = Enum.Font.GothamBold
                        textLabel.TextStrokeTransparency = 0
                        textLabel.TextStrokeColor3 = Color3.new(0,0,0)
                        textLabel.TextSize = 14
                        
                        espList[player] = {gui=billboardGui,text=textLabel}
                    end)
                end
                
                if espList[player] then
                    local displayText = ""
                    if S.e then displayText = displayText..player.Name.."\n" end
                    if S.sh and player.Character:FindFirstChild("Humanoid") then 
                        displayText = displayText.."HP: "..math.floor(player.Character.Humanoid.Health).."\n" 
                    end
                    if S.ss and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                        local distance = (player.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                        displayText = displayText..math.floor(distance).." studs"
                    end
                    espList[player].text.Text = displayText
                end
            else
                if espList[player] then 
                    espList[player].gui:Destroy() 
                    espList[player] = nil 
                end
            end
        end
    end
end

-- FOV Circle otimizado
local function updateFOVCircle()
    if S.fe then
        if not fovCircle then
            pcall(function()
                fovCircle = Drawing.new("Circle")
                fovCircle.Thickness = 2
                fovCircle.Filled = false
                fovCircle.Transparency = 0.8
            end)
        end
        
        if fovCircle then
            local screenCenter = getScreenCenter()
            fovCircle.Position = screenCenter
            fovCircle.Radius = S.fv
            fovCircle.Visible = true
            
            -- Mudar cor baseado no estado
            if aimbot.target and aimbot.lockOn then
                fovCircle.Color = Color3.new(1,0.2,0.2) -- Vermelho quando travado
            else
                fovCircle.Color = Color3.new(1,1,1) -- Branco normal
            end
        end
    elseif fovCircle then
        fovCircle.Visible = false
    end
end

-- Sistema de hitbox otimizado
local function updateHitboxes()
    for _, player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local rootPart = player.Character.HumanoidRootPart
            if S.eh then
                rootPart.Size = Vector3.new(8,8,8)
                rootPart.Transparency = 0.7
            else
                rootPart.Size = Vector3.new(2,2,1)
                rootPart.Transparency = 1
            end
        end
    end
end

-- Loop principal ultra otimizado - SISTEMA CORRIGIDO
local heartbeatConnection
heartbeatConnection = RS.Heartbeat:Connect(function(deltaTime)
    local currentTime = tick()
    
    -- Controle de taxa de atualização
    if currentTime - lastUpdate < updateInterval then return end
    lastUpdate = currentTime
    
    pcall(function()
        -- Sistema de Auto Aim principal - CORRIGIDO
        if S.am and S.fe then
            aimbot.enabled = true
            
            local newTarget = findBestTarget()
            
            if newTarget and newTarget:FindFirstChild("Head") then
                aimbot.target = newTarget
                aimbot.lockOn = true
                
                -- Mirar na cabeça para maior precisão
                local headPosition = newTarget.Head.Position
                smoothAimToTarget(headPosition)
            else
                -- Se não há alvo válido, desligar lock
                if not newTarget then
                    aimbot.target = nil
                    aimbot.lockOn = false
                end
            end
        else
            aimbot.enabled = false
            aimbot.target = nil
            aimbot.lockOn = false
        end
        
        -- Atualizar outros sistemas com menor frequência
        if currentTime % 0.1 < deltaTime then -- 10 FPS para sistemas menos críticos
            updateESP()
            updateHitboxes()
        end
        
        -- FOV Circle sempre atualizado para suavidade
        updateFOVCircle()
    end)
end)

-- Event handlers
ob.MouseButton1Click:Connect(function() 
    if not dragging then
        mf.Visible = true
        ob.Visible = false 
    end
end)

mb.MouseButton1Click:Connect(function() 
    mf.Visible = false
    ob.Visible = true 
end)

cb.MouseButton1Click:Connect(function() 
    -- Cleanup otimizado
    if heartbeatConnection then heartbeatConnection:Disconnect() end
    if fovCircle then pcall(function() fovCircle:Remove() end) end
    for _, esp in pairs(espList) do
        if esp.gui then pcall(function() esp.gui:Destroy() end) end
    end
    pcall(function() sg:Destroy() end)
end)

-- Ativar primeira aba
tabFrames["Aimbot"].Visible = true
tb:GetChildren()[1].BackgroundColor3 = Color3.fromRGB(120,50,180)

-- Notificação final
notify("Mitra Menu - Executado!")

return sg
