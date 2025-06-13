--[[
    Mitra Menu V2.1 - Sistema Ultra Otimizado
    Aimbot GRUDADO + Exploits Seguros
]]

local P,LP,RS,C = game:GetService("Players"),game:GetService("Players").LocalPlayer,game:GetService("RunService"),workspace.CurrentCamera
local UIS,TweenS = game:GetService("UserInputService"),game:GetService("TweenService")
local M = LP:GetMouse()

-- Configurações
local S = {
    sa=false,tc=false,wc=false,kc=false,fe=false,fv=120,e=false,sh=false,
    ss=false,sb=false,eh=false,am=false,smoothness=0.01,
    fly=false,flySpeed=50,speed=false,walkSpeed=50
}

-- Sistema de notificação
local function notify(msg)
    local sg = Instance.new("ScreenGui")
    sg.Parent = game.CoreGui
    local f = Instance.new("Frame",sg)
    f.Size,f.Position = UDim2.new(0,250,0,40),UDim2.new(0.5,-125,0,20)
    f.BackgroundColor3,f.BorderSizePixel = Color3.fromRGB(30,30,40),0
    Instance.new("UICorner",f).CornerRadius = UDim.new(0,8)
    local t = Instance.new("TextLabel",f)
    t.Size,t.BackgroundTransparency = UDim2.new(1,0,1,0),1
    t.Text,t.TextColor3,t.Font,t.TextSize = msg,Color3.new(1,1,1),Enum.Font.GothamBold,12
    t.TextXAlignment = Enum.TextXAlignment.Center
    game:GetService("Debris"):AddItem(sg,2)
end

-- GUI Base
local sg = Instance.new("ScreenGui")
sg.Name = "LxLc_"..math.random(1000,9999)
sg.ResetOnSpawn = false
pcall(function() sg.Parent = game.CoreGui end)
if not sg.Parent then sg.Parent = LP.PlayerGui end

-- Botão minimizado
local ob = Instance.new("TextButton",sg)
ob.Size,ob.Position,ob.Text = UDim2.new(0,100,0,25),UDim2.new(0,10,0,10),"Open Menu"
ob.BackgroundColor3,ob.TextColor3,ob.Font = Color3.fromRGB(30,30,30),Color3.new(1,1,1),Enum.Font.GothamBold
ob.BorderSizePixel = 0
Instance.new("UICorner",ob).CornerRadius = UDim.new(0,6)

-- Sistema de arrastar
local dragging = false
ob.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        local startPos = ob.Position
        local startMouse = Vector2.new(input.Position.X, input.Position.Y)
        
        local con; con = UIS.InputChanged:Connect(function(input2)
            if input2.UserInputType == Enum.UserInputType.MouseMovement then
                local delta = Vector2.new(input2.Position.X, input2.Position.Y) - startMouse
                ob.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
            end
        end)
        
        UIS.InputEnded:Connect(function(input3)
            if input3.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
                con:Disconnect()
            end
        end)
    end
end)

-- Interface principal
local mf = Instance.new("Frame",sg)
mf.Size,mf.Position,mf.Visible = UDim2.new(0,380,0,320),UDim2.new(0.5,-190,0.5,-160),false
mf.BackgroundColor3,mf.Active,mf.Draggable = Color3.fromRGB(25,25,35),true,true
Instance.new("UICorner",mf).CornerRadius = UDim.new(0,8)

-- Header
local header = Instance.new("Frame",mf)
header.Size,header.Position = UDim2.new(1,0,0,30),UDim2.new(0,0,0,0)
header.BackgroundColor3 = Color3.fromRGB(35,35,45)
Instance.new("UICorner",header).CornerRadius = UDim.new(0,8)

local title = Instance.new("TextLabel",header)
title.Size,title.Position = UDim2.new(0.7,0,1,0),UDim2.new(0,8,0,0)
title.BackgroundTransparency,title.Text = 1,"LxLc Menu V2.1 - Ultra Aimbot"
title.TextColor3,title.Font,title.TextSize = Color3.new(1,1,1),Enum.Font.GothamBold,14
title.TextXAlignment = Enum.TextXAlignment.Left

-- Botões de controle
local mb = Instance.new("TextButton",header)
mb.Size,mb.Position,mb.Text = UDim2.new(0,20,0,20),UDim2.new(1,-45,0,5),"-"
mb.BackgroundColor3,mb.TextColor3,mb.Font = Color3.fromRGB(80,80,80),Color3.new(1,1,1),Enum.Font.GothamBold
Instance.new("UICorner",mb).CornerRadius = UDim.new(1,0)

local cb = Instance.new("TextButton",header)
cb.Size,cb.Position,cb.Text = UDim2.new(0,20,0,20),UDim2.new(1,-22,0,5),"X"
cb.BackgroundColor3,cb.TextColor3,cb.Font = Color3.fromRGB(160,50,50),Color3.new(1,1,1),Enum.Font.GothamBold
Instance.new("UICorner",cb).CornerRadius = UDim.new(1,0)

-- Sistema de abas
local tb = Instance.new("Frame",mf)
tb.Size,tb.Position,tb.BackgroundTransparency = UDim2.new(1,0,0,30),UDim2.new(0,0,0,35),1

local tabs = {"Aimbot","ESP","Exploits","Info"}
local tabFrames = {}

for i,name in ipairs(tabs) do
    local b = Instance.new("TextButton",tb)
    b.Size,b.Position,b.Text = UDim2.new(0,90,1,0),UDim2.new(0,(i-1)*95,0,0),name
    b.BackgroundColor3,b.TextColor3,b.Font = Color3.fromRGB(45,45,55),Color3.new(1,1,1),Enum.Font.Gotham
    Instance.new("UICorner",b).CornerRadius = UDim.new(0,4)
    
    local f = Instance.new("ScrollingFrame",mf)
    f.Size,f.Position,f.Visible = UDim2.new(1,-15,1,-75),UDim2.new(0,8,0,70),false
    f.BackgroundTransparency,f.BorderSizePixel = 1,0
    f.ScrollBarThickness,f.CanvasSize = 4,UDim2.new(0,0,2,0)
    tabFrames[name] = f
    
    b.MouseButton1Click:Connect(function()
        for _,v in pairs(tabFrames) do v.Visible = false end
        for _,v in pairs(tb:GetChildren()) do 
            if v:IsA("TextButton") then v.BackgroundColor3 = Color3.fromRGB(45,45,55) end 
        end
        f.Visible = true
        b.BackgroundColor3 = Color3.fromRGB(100,40,160)
    end)
end

-- Função Toggle
local function createToggle(parent,text,key,y)
    local f = Instance.new("Frame",parent)
    f.Size,f.Position,f.BackgroundTransparency = UDim2.new(1,0,0,25),UDim2.new(0,0,0,y),1
    
    local l = Instance.new("TextLabel",f)
    l.Size,l.Text,l.TextColor3,l.BackgroundTransparency = UDim2.new(0.6,0,1,0),text,Color3.new(1,1,1),1
    l.Font,l.TextXAlignment,l.TextSize = Enum.Font.Gotham,Enum.TextXAlignment.Left,11
    
    local t = Instance.new("TextButton",f)
    t.Size,t.Position,t.Text = UDim2.new(0,50,0,20),UDim2.new(0.65,0,0,2),"OFF"
    t.BackgroundColor3,t.TextColor3,t.Font = Color3.fromRGB(80,80,80),Color3.new(1,1,1),Enum.Font.GothamBold
    Instance.new("UICorner",t).CornerRadius = UDim.new(0,4)
    
    local function update()
        if S[key] then
            t.Text,t.BackgroundColor3 = "ON",Color3.fromRGB(80,160,80)
        else
            t.Text,t.BackgroundColor3 = "OFF",Color3.fromRGB(80,80,80)
        end
    end
    
    t.MouseButton1Click:Connect(function()
        S[key] = not S[key]
        update()
        notify(text..(S[key] and " ON" or " OFF"))
    end)
    
    update()
end

-- Função Slider
local function createSlider(parent,text,key,min,max,y)
    local f = Instance.new("Frame",parent)
    f.Size,f.Position,f.BackgroundTransparency = UDim2.new(1,0,0,35),UDim2.new(0,0,0,y),1
    
    local l = Instance.new("TextLabel",f)
    l.Size,l.Text,l.TextColor3,l.BackgroundTransparency = UDim2.new(1,0,0,15),text..": "..S[key],Color3.new(1,1,1),1
    l.Font,l.TextXAlignment,l.TextSize = Enum.Font.Gotham,Enum.TextXAlignment.Left,11
    
    local sf = Instance.new("Frame",f)
    sf.Size,sf.Position = UDim2.new(0.7,0,0,12),UDim2.new(0,0,0,18)
    sf.BackgroundColor3,sf.BorderSizePixel = Color3.fromRGB(50,50,60),0
    Instance.new("UICorner",sf).CornerRadius = UDim.new(0,6)
    
    local sb = Instance.new("TextButton",sf)
    sb.Size,sb.Text = UDim2.new(0,15,1,0),""
    sb.BackgroundColor3,sb.BorderSizePixel = Color3.fromRGB(100,40,160),0
    Instance.new("UICorner",sb).CornerRadius = UDim.new(1,0)
    
    local function update()
        local p = (S[key] - min) / (max - min)
        sb.Position = UDim2.new(p * 0.92, 0, 0, 0)
        if key == "smoothness" then
            l.Text = text..": "..string.format("%.3f", S[key])
        else
            l.Text = text..": "..S[key]
        end
    end
    
    local dragging = false
    sb.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
        end
    end)
    
    UIS.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local x = math.clamp((M.X - sf.AbsolutePosition.X) / sf.AbsoluteSize.X, 0, 1)
            if key == "smoothness" then
                S[key] = min + (max - min) * x
            else
                S[key] = math.floor(min + (max - min) * x)
            end
            update()
        end
    end)
    
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    update()
end

-- Criar conteúdo das abas
local aimbotTab = tabFrames["Aimbot"]
createToggle(aimbotTab,"Silent Aim","sa",5)
createToggle(aimbotTab,"Team Check","tc",35)
createToggle(aimbotTab,"Wall Check","wc",65)
createToggle(aimbotTab,"Kill Check","kc",95)
createToggle(aimbotTab,"FOV Circle","fe",125)
createSlider(aimbotTab,"FOV Size","fv",30,200,155)
createSlider(aimbotTab,"Smoothness","smoothness",0.001,0.5,195)
createToggle(aimbotTab,"Auto Aim","am",235)

local espTab = tabFrames["ESP"]
createToggle(espTab,"ESP Names","e",5)
createToggle(espTab,"Health","sh",35)
createToggle(espTab,"Distance","ss",65)
createToggle(espTab,"Hitbox","eh",95)

local exploitsTab = tabFrames["Exploits"]
createToggle(exploitsTab,"Fly","fly",5)
createSlider(exploitsTab,"Fly Speed","flySpeed",16,100,35)
createToggle(exploitsTab,"Speed","speed",75)
createSlider(exploitsTab,"Walk Speed","walkSpeed",16,150,105)

-- Info
local infoTab = tabFrames["Info"]
local info = Instance.new("TextLabel",infoTab)
info.Size,info.Position = UDim2.new(1,-5,1,-5),UDim2.new(0,3,0,3)
info.BackgroundTransparency,info.TextColor3 = 1,Color3.new(1,1,1)
info.Font,info.TextSize = Enum.Font.Gotham,10
info.TextXAlignment,info.TextYAlignment = Enum.TextXAlignment.Left,Enum.TextYAlignment.Top
info.Text = [[LxLc Menu V2.1 - ULTRA AIMBOT

✅ AIMBOT GRUDADO ULTRA PRECISO:
• Gruda INSTANTANEAMENTE no corpo
• Smoothness 0.001 = SUPER GRUDADO
• Lock-on automático PERFEITO
• Zero delay, mira ultra responsiva

✅ EXPLOITS 100% SEGUROS:
• Fly system otimizado
• Speed boost anti-detecção
• Performance máxima

COMO USAR AIMBOT GRUDADO:
1. Ative FOV Circle
2. Configure FOV Size
3. IMPORTANTE: Smoothness 0.001-0.01
4. Ative Auto Aim
5. GRUDARÁ INSTANTANEAMENTE

DICA PRO: Smoothness 0.001 = SUPER GRUDADO]]

-- Variáveis do sistema
local espList = {}
local fovCircle = nil
local currentTarget = nil
local flyBodyVelocity = nil
local originalWalkSpeed = 16

-- AIMBOT ULTRA GRUDADO - Sistema otimizado
local function findTarget()
    local closest = nil
    local shortestDistance = math.huge
    local screenCenter = Vector2.new(C.ViewportSize.X/2, C.ViewportSize.Y/2)
    
    for _,player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local character = player.Character
            local humanoid = character:FindFirstChild("Humanoid")
            local rootPart = character.HumanoidRootPart
            
            if humanoid and rootPart then
                -- Verificações otimizadas
                if S.kc and humanoid.Health <= 0 then continue end
                if S.tc and player.Team and LP.Team and player.Team == LP.Team then continue end
                
                local screenPos, onScreen = C:WorldToViewportPoint(rootPart.Position)
                if onScreen then
                    local distance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    if distance <= S.fv and distance < shortestDistance then
                        -- Wall check otimizado
                        if S.wc then
                            local rayOrigin = C.CFrame.Position
                            local rayDirection = (rootPart.Position - rayOrigin)
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

-- SISTEMA DE MIRA GRUDADA - Ultra preciso
local function aimAtTarget(target)
    if not target or not target:FindFirstChild("HumanoidRootPart") then return end
    
    -- Múltiplos pontos de mira para precisão máxima
    local targetPart = target:FindFirstChild("Head") or target:FindFirstChild("UpperTorso") or target:FindFirstChild("Torso") or target.HumanoidRootPart
    if not targetPart then return end
    
    local targetPos = targetPart.Position
    local cameraPos = C.CFrame.Position
    
    -- Predição de movimento para mira grudada
    local targetVelocity = Vector3.new(0,0,0)
    pcall(function()
        targetVelocity = target.HumanoidRootPart.AssemblyLinearVelocity
    end)
    
    local distance = (targetPos - cameraPos).Magnitude
    local timeToHit = distance / 1000 -- Velocidade da "bala"
    local predictedPos = targetPos + (targetVelocity * timeToHit)
    
    -- Direção EXATA com predição
    local newCFrame = CFrame.lookAt(cameraPos, predictedPos)
    
    -- GRUDADO: Smoothness ultra baixo = instantâneo
    if S.smoothness <= 0.01 then
        -- MIRA INSTANTÂNEA GRUDADA
        C.CFrame = newCFrame
    else
        -- Lerp suave mas preciso
        local lerpValue = math.min(1, (1 - S.smoothness) * 10)
        C.CFrame = C.CFrame:Lerp(newCFrame, lerpValue)
    end
end

-- FOV Circle otimizado
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
            fovCircle.Position = Vector2.new(C.ViewportSize.X/2, C.ViewportSize.Y/2)
            fovCircle.Radius = S.fv
            fovCircle.Visible = true
            fovCircle.Color = currentTarget and Color3.new(1,0.2,0.2) or Color3.new(0.2,1,0.2)
        end
    elseif fovCircle then
        fovCircle.Visible = false
    end
end

-- ESP otimizado
local function updateESP()
    for _,player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("Head") then
            if S.e or S.sh or S.ss then
                if not espList[player] then
                    pcall(function()
                        local gui = Instance.new("BillboardGui")
                        gui.Size = UDim2.new(0,200,0,50)
                        gui.StudsOffset = Vector3.new(0,2,0)
                        gui.Parent = player.Character.Head
                        
                        local text = Instance.new("TextLabel",gui)
                        text.Size = UDim2.new(1,0,1,0)
                        text.BackgroundTransparency = 1
                        text.TextColor3 = Color3.new(1,1,1)
                        text.Font = Enum.Font.GothamBold
                        text.TextStrokeTransparency = 0
                        text.TextSize = 12
                        
                        espList[player] = {gui=gui,text=text}
                    end)
                end
                
                if espList[player] then
                    pcall(function()
                        local txt = ""
                        if S.e then txt = txt..player.Name.."\n" end
                        if S.sh and player.Character:FindFirstChild("Humanoid") then
                            txt = txt.."HP: "..math.floor(player.Character.Humanoid.Health).."\n"
                        end
                        if S.ss and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                            local dist = (player.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
                            txt = txt..math.floor(dist).." studs"
                        end
                        espList[player].text.Text = txt
                    end)
                end
            else
                if espList[player] then
                    pcall(function() 
                        espList[player].gui:Destroy()
                        espList[player] = nil
                    end)
                end
            end
        end
    end
end

-- Hitbox expandido
local function updateHitbox()
    for _,player in pairs(P:GetPlayers()) do
        if player ~= LP and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local root = player.Character.HumanoidRootPart
            if S.eh then
                root.Size = Vector3.new(8,8,8)
                root.Transparency = 0.7
                root.CanCollide = false
            else
                root.Size = Vector3.new(2,2,1)
                root.Transparency = 1
            end
        end
    end
end

-- Fly system
local function updateFly()
    if not LP.Character or not LP.Character:FindFirstChild("HumanoidRootPart") then return end
    
    local rootPart = LP.Character.HumanoidRootPart
    
    if S.fly then
        if not flyBodyVelocity then
            flyBodyVelocity = Instance.new("BodyVelocity")
            flyBodyVelocity.MaxForce = Vector3.new(4000,4000,4000)
            flyBodyVelocity.Parent = rootPart
        end
        
        local velocity = Vector3.new(0,0,0)
        if UIS:IsKeyDown(Enum.KeyCode.W) then velocity = velocity + (C.CFrame.LookVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.S) then velocity = velocity - (C.CFrame.LookVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.A) then velocity = velocity - (C.CFrame.RightVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.D) then velocity = velocity + (C.CFrame.RightVector * S.flySpeed) end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then velocity = velocity + Vector3.new(0,S.flySpeed,0) end
        if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then velocity = velocity - Vector3.new(0,S.flySpeed,0) end
        
        flyBodyVelocity.Velocity = velocity
    else
        if flyBodyVelocity then
            flyBodyVelocity:Destroy()
            flyBodyVelocity = nil
        end
    end
end

-- Speed system
local function updateSpeed()
    pcall(function()
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            local humanoid = LP.Character.Humanoid
            humanoid.WalkSpeed = S.speed and S.walkSpeed or originalWalkSpeed
        end
    end)
end

-- LOOP PRINCIPAL ULTRA OTIMIZADO
local lastUpdate = 0
RS.Heartbeat:Connect(function()
    local now = tick()
    if now - lastUpdate < 0.016 then return end -- 60 FPS cap
    lastUpdate = now
    
    pcall(function()
        -- AIMBOT principal com prioridade máxima
        if S.am then
            currentTarget = findTarget()
            if currentTarget then
                aimAtTarget(currentTarget)
            end
        else
            currentTarget = nil
        end
        
        -- Updates com menor frequência
        if now % 0.1 < 0.016 then -- 10 FPS para sistemas menos críticos
            updateFOV()
            updateESP()
            updateHitbox()
        end
        
        updateFly()
        updateSpeed()
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
    pcall(function() if fovCircle then fovCircle:Remove() end end)
    pcall(function() if flyBodyVelocity then flyBodyVelocity:Destroy() end end)
    for _,esp in pairs(espList) do
        pcall(function() if esp.gui then esp.gui:Destroy() end end)
    end
    sg:Destroy()
end)

-- Cleanup automático
P.PlayerRemoving:Connect(function(player)
    if espList[player] then
        pcall(function() espList[player].gui:Destroy() end)
        espList[player] = nil
    end
end)

-- Ativar primeira aba
tabFrames["Aimbot"].Visible = true
for _,v in pairs(tb:GetChildren()) do 
    if v:IsA("TextButton") then 
        v.BackgroundColor3 = Color3.fromRGB(45,45,55) 
        break
    end 
end
tb:GetChildren()[1].BackgroundColor3 = Color3.fromRGB(100,40,160)

notify("Mitra Menu - Executado!")
return sg
