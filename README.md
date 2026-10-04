-- ==================== SISTEMA DE KEY (PRIMEIRO) ====================
local KEY_CORRETA = "157"

local p = game.Players.LocalPlayer
local PlayerGui = p:WaitForChild("PlayerGui")

local keyGui = Instance.new("ScreenGui", PlayerGui)
keyGui.Name = "LorenzoKeySystem"
keyGui.ResetOnSpawn = false

local keyFrame = Instance.new("Frame", keyGui)
keyFrame.Size = UDim2.new(0, 300, 0, 180)
keyFrame.Position = UDim2.new(0.5, -150, 0.5, -90)
keyFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
keyFrame.BorderSizePixel = 0
keyFrame.Active = true
keyFrame.Draggable = true

local cornerKey = Instance.new("UICorner", keyFrame)
cornerKey.CornerRadius = UDim.new(0, 10)

local strokeKey = Instance.new("UIStroke", keyFrame)
strokeKey.Color = Color3.fromRGB(255, 0, 0)
strokeKey.Thickness = 2

local titleKey = Instance.new("TextLabel", keyFrame)
titleKey.Size = UDim2.new(1, 0, 0, 35)
titleKey.BackgroundTransparency = 1
titleKey.Text = "SISTEMA DE KEY"
titleKey.TextColor3 = Color3.fromRGB(255, 0, 0)
titleKey.Font = Enum.Font.Code
titleKey.TextSize = 18

local textBoxKey = Instance.new("TextBox", keyFrame)
textBoxKey.Size = UDim2.new(0.8, 0, 0, 35)
textBoxKey.Position = UDim2.new(0.1, 0, 0.3, 0)
textBoxKey.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
textBoxKey.TextColor3 = Color3.fromRGB(255, 255, 255)
textBoxKey.PlaceholderText = "Digite a Key aqui..."
textBoxKey.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
textBoxKey.Font = Enum.Font.Code
textBoxKey.TextSize = 14
textBoxKey.Text = ""

local cornerBox = Instance.new("UICorner", textBoxKey)
cornerBox.CornerRadius = UDim.new(0, 6)

local btnEntrar = Instance.new("TextButton", keyFrame)
btnEntrar.Size = UDim2.new(0.8, 0, 0, 35)
btnEntrar.Position = UDim2.new(0.1, 0, 0.6, 10)
btnEntrar.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
btnEntrar.TextColor3 = Color3.fromRGB(255, 255, 255)
btnEntrar.Text = "ENTRAR"
btnEntrar.Font = Enum.Font.Code
btnEntrar.TextSize = 16

local cornerBtn = Instance.new("UICorner", btnEntrar)
cornerBtn.CornerRadius = UDim.new(0, 6)

local keyAprovada = false

btnEntrar.MouseButton1Click:Connect(function()
    if textBoxKey.Text == KEY_CORRETA then
        keyAprovada = true
        keyGui:Destroy()
    else
        textBoxKey.Text = ""
        textBoxKey.PlaceholderText = "Key Incorreta!"
        textBoxKey.PlaceholderColor3 = Color3.fromRGB(255, 50, 50)
    end
end)

repeat task.wait() until keyAprovada

-- ==================== TELA PRETA DE INTRO ====================
local introGui = Instance.new("ScreenGui", PlayerGui)
introGui.DisplayOrder = 9999
introGui.ResetOnSpawn = false

local fundoPreto = Instance.new("Frame", introGui)
fundoPreto.Size = UDim2.new(1, 0, 1, 0)
fundoPreto.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
fundoPreto.BorderSizePixel = 0

local textoIntro = Instance.new("TextLabel", fundoPreto)
textoIntro.Size = UDim2.new(1, 0, 1, 0)
textoIntro.BackgroundTransparency = 1
textoIntro.Text = "Lorenzo Hub"
textoIntro.TextColor3 = Color3.fromRGB(255, 0, 0)
textoIntro.TextSize = 80
textoIntro.Font = Enum.Font.Code

task.wait(2.5)

for i = 0, 1, 0.05 do
    fundoPreto.BackgroundTransparency = i
    textoIntro.TextTransparency = i
    task.wait(0.04)
end
introGui:Destroy()

-- ==================== VARIÁVEIS PRINCIPAIS ====================
local Pessoas = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera
local Mouse = p:GetMouse()

if p.PlayerGui:FindFirstChild("xX_XxHub") then
    p.PlayerGui.xX_XxHub:Destroy()
end

local gui = Instance.new("ScreenGui")
gui.Name = "xX_XxHub"
gui.ResetOnSpawn = false
gui.Parent = p:WaitForChild("PlayerGui")

local function addCorner(parent, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius or 8)
    corner.Parent = parent
    return corner
end

-- ==================== VARIÁVEIS DE ESTADO ====================
local ws = 16
local jp = 50
local hitboxSize = 2 

local infJumpAtivo = false
local noclipAtivo = false
local flyAtivo = false
local touchFlingAtivo = false
local espAtivo = false
local espLineAtivo = false
local travarServeAtivo = false
local transparenciaAtiva = false
local invisivelAtivo = false
local rgbBonecoAtivo = false
local painelRgbAtivo = true 
local corPainelAtual = Color3.fromRGB(255, 0, 0)
local espectandoAtivo = false
local aimbotAtivo = false
local aimbotParteAlvo = "Head"
local walkFlingAtivado = false
local conexaoWalkFling = nil

local teclaAtalho = nil
local aguardandoTecla = false
local temaAtual = "padrao"
local tituloForaAtivo = true

-- MM2 ESP & RECURSOS
local mm2AssassinoAtivo = false
local mm2XerifeAtivo = false
local mm2InocenteAtivo = false
local autoTpArmaAtivo = false
local aimbotApenasAssassinoAtivo = false
local mm2Boxes = {}
local mm2Tags = {}
local mm2TracerLines = {}

local originalTransparencias = {}
local tamanhosOriginaisParts = {}
local transparenciaPersonagemOriginal = {}
local espBoxes = {}
local espTags = {}
local espTracerLines = {}
local tamanhosOriginaisHrps = {}
local c00lkiddAtivo = false
local jogadorSelecionado = nil
local alvoUltSelecionado = nil
local jaAutoPegouArma = false

-- Função para localizar a Arma do Xerife no chão
local function encontrarArmaNoChao()
    for _, obj in pairs(Workspace:GetChildren()) do
        if obj.Name == "GunDrop" or obj.Name == "SheriffGun" then
            return obj
        end
    end
    for _, obj in pairs(Workspace:GetDescendants()) do
        if (obj:IsA("Tool") or obj:IsA("Model")) and (obj.Name == "GunDrop" or obj.Name == "Gun" or obj.Name == "SheriffGun") then
            if not obj:IsDescendantOf(Pessoas) then
                return obj
            end
        end
    end
    return nil
end

-- FOV AIMBOT
local fovCircle = Drawing.new("Circle")
fovCircle.Visible = false
fovCircle.Transparency = 0.7
fovCircle.Thickness = 2
fovCircle.Color = Color3.fromRGB(255, 0, 0)
fovCircle.Filled = false
fovCircle.Radius = 100
fovCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

-- ==================== JANELA PRINCIPAL ====================
local f = Instance.new("Frame", gui)
f.Size = UDim2.new(0, 340, 0, 420)
f.Position = UDim2.new(0.5, -170, 0.5, -210)
f.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
f.BorderSizePixel = 0
f.Active = true
f.Draggable = true 
f.Visible = true
addCorner(f, 12)

local strokeMain = Instance.new("UIStroke", f)
strokeMain.Thickness = 2
strokeMain.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
strokeMain.Color = Color3.fromRGB(255, 0, 0)

local titulo = Instance.new("TextLabel", f)
titulo.Size = UDim2.new(1, -40, 0, 30)
titulo.Position = UDim2.new(0, 10, 0, 5)
titulo.Text = "Lorenzo Hub"
titulo.Font = Enum.Font.Code
titulo.TextSize = 16
titulo.TextColor3 = Color3.fromRGB(255, 0, 0)
titulo.TextXAlignment = Enum.TextXAlignment.Left
titulo.BackgroundTransparency = 1
titulo.ZIndex = 2

local fechar = Instance.new("TextButton", f)
fechar.Size = UDim2.new(0, 24, 0, 24)
fechar.Position = UDim2.new(1, -30, 0, 5)
fechar.Text = "✕"
fechar.Font = Enum.Font.GothamBold
fechar.TextSize = 13
fechar.TextColor3 = Color3.fromRGB(255, 0, 0)
fechar.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
fechar.BorderSizePixel = 0
addCorner(fechar, 6)
fechar.ZIndex = 2

-- ==================== ÁREA DE CONTEÚDO ====================
local function criarAreaConteudo()
    local sc = Instance.new("ScrollingFrame", f)
    sc.Size = UDim2.new(1, -115, 1, -45)
    sc.Position = UDim2.new(0, 10, 0, 38)
    sc.BackgroundTransparency = 1
    sc.ScrollBarThickness = 4
    sc.Visible = false
    sc.ZIndex = 2
    return sc
end

local abaJogador = criarAreaConteudo()
abaJogador.CanvasSize = UDim2.new(0, 0, 0, 720)
abaJogador.Visible = true

local abaLista = criarAreaConteudo()
abaLista.CanvasSize = UDim2.new(0, 0, 0, 300)

local abaScripts = criarAreaConteudo()
abaScripts.CanvasSize = UDim2.new(0, 0, 0, 400)

local abaMm2 = criarAreaConteudo()
abaMm2.CanvasSize = UDim2.new(0, 0, 0, 420)

local abaConfig = criarAreaConteudo()
abaConfig.CanvasSize = UDim2.new(0, 0, 0, 560)

-- ==================== PAINEL DAS ABAS ====================
local sidebar = Instance.new("Frame", f)
sidebar.Size = UDim2.new(0, 85, 1, -45)
sidebar.Position = UDim2.new(1, -95, 0, 38)
sidebar.BackgroundTransparency = 1
sidebar.ZIndex = 2

local function criarAbaBtn(txt, yPos)
    local b = Instance.new("TextButton", sidebar)
    b.Size = UDim2.new(1, 0, 0, 26)
    b.Position = UDim2.new(0, 0, 0, yPos)
    b.Text = txt
    b.Font = Enum.Font.Code
    b.TextSize = 9
    b.TextColor3 = Color3.fromRGB(200, 150, 150)
    b.BackgroundTransparency = 0.5
    b.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    b.BorderSizePixel = 0
    b.ZIndex = 2
    addCorner(b, 6)
    return b
end

local abaJogadorBtn = criarAbaBtn("Jogador", 0)
abaJogadorBtn.BackgroundTransparency = 0.2
abaJogadorBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
abaJogadorBtn.TextColor3 = Color3.fromRGB(255, 255, 255)

local abaListaBtn = criarAbaBtn("Pessoas", 30)
local abaScriptsBtn = criarAbaBtn("Scripts", 60)
local abaMm2Btn = criarAbaBtn("Murder Mystery", 90)
local abaConfigBtn = criarAbaBtn("Configurar", 120)

local function mudarAba(ativa)
    abaJogador.Visible = (ativa == 1)
    abaLista.Visible = (ativa == 2)
    abaScripts.Visible = (ativa == 3)
    abaMm2.Visible = (ativa == 4)
    abaConfig.Visible = (ativa == 5)

    abaJogadorBtn.BackgroundTransparency = (ativa == 1) and 0.2 or 0.5
    abaJogadorBtn.BackgroundColor3 = (ativa == 1) and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    abaJogadorBtn.TextColor3 = (ativa == 1) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 150, 150)

    abaListaBtn.BackgroundTransparency = (ativa == 2) and 0.2 or 0.5
    abaListaBtn.BackgroundColor3 = (ativa == 2) and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    abaListaBtn.TextColor3 = (ativa == 2) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 150, 150)

    abaScriptsBtn.BackgroundTransparency = (ativa == 3) and 0.2 or 0.5
    abaScriptsBtn.BackgroundColor3 = (ativa == 3) and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    abaScriptsBtn.TextColor3 = (ativa == 3) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 150, 150)

    abaMm2Btn.BackgroundTransparency = (ativa == 4) and 0.2 or 0.5
    abaMm2Btn.BackgroundColor3 = (ativa == 4) and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    abaMm2Btn.TextColor3 = (ativa == 4) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 150, 150)

    abaConfigBtn.BackgroundTransparency = (ativa == 5) and 0.2 or 0.5
    abaConfigBtn.BackgroundColor3 = (ativa == 5) and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    abaConfigBtn.TextColor3 = (ativa == 5) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(200, 150, 150)
end

abaJogadorBtn.MouseButton1Click:Connect(function() mudarAba(1) end)
abaListaBtn.MouseButton1Click:Connect(function() mudarAba(2) end)
abaScriptsBtn.MouseButton1Click:Connect(function() mudarAba(3) end)
abaMm2Btn.MouseButton1Click:Connect(function() mudarAba(4) end)
abaConfigBtn.MouseButton1Click:Connect(function() mudarAba(5) end)

local todosBotoes = {}

local function criarBtn(parent, txt, pos, size)
    local b = Instance.new("TextButton", parent)
    b.Size = size or UDim2.new(1, 0, 0, 28)
    b.Position = pos
    b.Text = txt
    b.Font = Enum.Font.Code
    b.TextSize = 11
    b.TextColor3 = Color3.fromRGB(255, 200, 200)
    b.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    b.BorderSizePixel = 0
    b.ZIndex = 2
    addCorner(b, 6)
    table.insert(todosBotoes, b)
    return b
end

-- ==================== TÍTULO LORENZO FORA DO PAINEL ====================
local titleButton = Instance.new("TextButton", gui)
titleButton.Name = "TitleLorenzo"
titleButton.Position = UDim2.new(0.5, 0, 0, 2) 
titleButton.AnchorPoint = Vector2.new(0.5, 0)
titleButton.Size = UDim2.new(0, 200, 0, 35)
titleButton.BackgroundTransparency = 1 
titleButton.Font = Enum.Font.GothamBold
titleButton.Text = "Lorenzo"
titleButton.TextColor3 = Color3.fromRGB(255, 255, 255) 
titleButton.TextSize = 20
titleButton.TextStrokeTransparency = 0.5 
titleButton.ZIndex = 10
titleButton.Visible = true

titleButton.MouseButton1Click:Connect(function()
    f.Visible = not f.Visible
end)

-- ==================== APLICAÇÃO DE TEMAS ====================
local function aplicarTema(nomeTema)
    temaAtual = nomeTema
    painelRgbAtivo = false
    
    if nomeTema == "hacker" then
        corPainelAtual = Color3.fromRGB(0, 255, 0)
        f.BackgroundColor3 = Color3.fromRGB(5, 15, 5)
        strokeMain.Color = Color3.fromRGB(0, 255, 0)
        titulo.TextColor3 = Color3.fromRGB(0, 255, 0)
        titulo.Font = Enum.Font.Code
        for _, btn in pairs(todosBotoes) do
            btn.BackgroundColor3 = Color3.fromRGB(0, 25, 0)
            btn.TextColor3 = Color3.fromRGB(0, 255, 0)
            btn.Font = Enum.Font.Code
        end
    elseif nomeTema == "minecraft" then
        corPainelAtual = Color3.fromRGB(60, 39, 24)
        f.BackgroundColor3 = Color3.fromRGB(116, 82, 58)
        strokeMain.Color = Color3.fromRGB(60, 39, 24)
        titulo.TextColor3 = Color3.fromRGB(255, 255, 255)
        titulo.Font = Enum.Font.GothamBold
        for _, btn in pairs(todosBotoes) do
            btn.BackgroundColor3 = Color3.fromRGB(139, 139, 139)
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamBold
        end
    elseif nomeTema == "tubers93" then
        corPainelAtual = Color3.fromRGB(255, 0, 51)
        f.BackgroundColor3 = Color3.fromRGB(10, 0, 0)
        strokeMain.Color = Color3.fromRGB(255, 0, 51)
        titulo.TextColor3 = Color3.fromRGB(255, 0, 51)
        titulo.Font = Enum.Font.SourceSansBold
        for _, btn in pairs(todosBotoes) do
            btn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
            btn.TextColor3 = Color3.fromRGB(255, 0, 51)
            btn.Font = Enum.Font.SourceSansBold
        end
    elseif nomeTema == "padrao" then
        painelRgbAtivo = true
        f.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        titulo.Font = Enum.Font.Code
        for _, btn in pairs(todosBotoes) do
            btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
            btn.TextColor3 = Color3.fromRGB(255, 200, 200)
            btn.Font = Enum.Font.Code
        end
    end
end

-- ==================== FUNÇÃO FLING ====================
local function FlingNoAlvo(alvo)
    local meuChar = p.Character
    local alvoChar = alvo.Character
    
    if meuChar and alvoChar and meuChar:FindFirstChild("HumanoidRootPart") and alvoChar:FindFirstChild("HumanoidRootPart") then
        local hrp = meuChar.HumanoidRootPart
        local hrpAlvo = alvoChar.HumanoidRootPart
        local humanoid = meuChar:FindFirstChildOfClass("Humanoid")
        local posicaoOriginal = hrp.CFrame
        
        for _, v in pairs(meuChar:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
        if humanoid then humanoid.PlatformStand = true end
        
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = hrp
        
        local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
        bodyAngularVelocity.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        bodyAngularVelocity.AngularVelocity = Vector3.new(0, 50000, 0)
        bodyAngularVelocity.Parent = hrp
        
        local tempoInicio = tick()
        while tick() - tempoInicio < 0.6 do
            if hrpAlvo and hrpAlvo.Parent and hrp and hrp.Parent then
                hrp.CFrame = hrpAlvo.CFrame
                bodyVelocity.Velocity = Vector3.new(math.random(-300, 300), 4000, math.random(-300, 300))
            end
            task.wait()
        end
        
        bodyAngularVelocity:Destroy()
        bodyVelocity:Destroy()
        if humanoid then humanoid.PlatformStand = false end
        
        for _, v in pairs(meuChar:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = true end
        end
        
        if hrp and hrp.Parent then
            hrp.CFrame = posicaoOriginal
            hrp.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
            hrp.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
        end
    end
end

-- ==================== ABA PESSOAS & MINI PAINEL ====================
local containerLista = Instance.new("ScrollingFrame", abaLista)
containerLista.Size = UDim2.new(1, 0, 1, 0)
containerLista.BackgroundTransparency = 1
containerLista.ScrollBarThickness = 4
containerLista.CanvasSize = UDim2.new(0, 0, 0, 0)
containerLista.ZIndex = 2

local miniPainel = Instance.new("Frame", gui)
miniPainel.Size = UDim2.new(0, 190, 0, 125)
miniPainel.Position = UDim2.new(0.5, -95, 0.5, -62)
miniPainel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
miniPainel.BorderSizePixel = 0
miniPainel.Visible = false
miniPainel.Active = true
miniPainel.Draggable = true
miniPainel.ZIndex = 15
addCorner(miniPainel, 8)

local miniStroke = Instance.new("UIStroke", miniPainel)
miniStroke.Color = Color3.fromRGB(255, 0, 0)
miniStroke.Thickness = 2

local miniLabel = Instance.new("TextLabel", miniPainel)
miniLabel.Size = UDim2.new(1, 0, 0, 22)
miniLabel.Position = UDim2.new(0, 0, 0, 4)
miniLabel.Text = "Alvo: Nenhum"
miniLabel.Font = Enum.Font.Code
miniLabel.TextSize = 11
miniLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
miniLabel.BackgroundTransparency = 1
miniLabel.ZIndex = 16

local tpMiniBtn = Instance.new("TextButton", miniPainel)
tpMiniBtn.Size = UDim2.new(0.28, 0, 0, 28)
tpMiniBtn.Position = UDim2.new(0.03, 0, 0, 28)
tpMiniBtn.Text = "TP"
tpMiniBtn.Font = Enum.Font.Code
tpMiniBtn.TextSize = 11
tpMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tpMiniBtn.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
tpMiniBtn.BorderSizePixel = 0
tpMiniBtn.ZIndex = 16
addCorner(tpMiniBtn, 5)

local viewMiniBtn = Instance.new("TextButton", miniPainel)
viewMiniBtn.Size = UDim2.new(0.31, 0, 0, 28)
viewMiniBtn.Position = UDim2.new(0.34, 0, 0, 28)
viewMiniBtn.Text = "View: OFF"
viewMiniBtn.Font = Enum.Font.Code
viewMiniBtn.TextSize = 10
viewMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
viewMiniBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
viewMiniBtn.BorderSizePixel = 0
viewMiniBtn.ZIndex = 16
addCorner(viewMiniBtn, 5)

local flingMiniBtn = Instance.new("TextButton", miniPainel)
flingMiniBtn.Size = UDim2.new(0.29, 0, 0, 28)
flingMiniBtn.Position = UDim2.new(0.68, 0, 0, 28)
flingMiniBtn.Text = "Fling"
flingMiniBtn.Font = Enum.Font.Code
flingMiniBtn.TextSize = 11
flingMiniBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
flingMiniBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
flingMiniBtn.BorderSizePixel = 0
flingMiniBtn.ZIndex = 16
addCorner(flingMiniBtn, 5)

local fecharMini = Instance.new("TextButton", miniPainel)
fecharMini.Size = UDim2.new(0.94, 0, 0, 24)
fecharMini.Position = UDim2.new(0.03, 0, 0, 68)
fecharMini.Text = "Fechar"
fecharMini.Font = Enum.Font.Code
fecharMini.TextSize = 10
fecharMini.TextColor3 = Color3.fromRGB(200, 150, 150)
fecharMini.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
fecharMini.BorderSizePixel = 0
fecharMini.ZIndex = 16
addCorner(fecharMini, 5)

fecharMini.MouseButton1Click:Connect(function()
    miniPainel.Visible = false
    espectandoAtivo = false
    viewMiniBtn.Text = "View: OFF"
    viewMiniBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Camera.CameraType = Enum.CameraType.Custom
    if p.Character and p.Character:FindFirstChildOfClass("Humanoid") then
        Camera.CameraSubject = p.Character:FindFirstChildOfClass("Humanoid")
    end
    jogadorSelecionado = nil
end)

tpMiniBtn.MouseButton1Click:Connect(function()
    if jogadorSelecionado and jogadorSelecionado.Character and jogadorSelecionado.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = p.Character and p.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.CFrame = jogadorSelecionado.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
        end
    end
end)

flingMiniBtn.MouseButton1Click:Connect(function()
    if jogadorSelecionado then FlingNoAlvo(jogadorSelecionado) end
end)

viewMiniBtn.MouseButton1Click:Connect(function()
    espectandoAtivo = not espectandoAtivo
    viewMiniBtn.Text = espectandoAtivo and "View: ON" or "View: OFF"
    viewMiniBtn.BackgroundColor3 = espectandoAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    
    if espectandoAtivo and jogadorSelecionado and jogadorSelecionado.Character then
        local humanoid = jogadorSelecionado.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            Camera.CameraType = Enum.CameraType.Custom
            Camera.CameraSubject = humanoid
        end
    else
        Camera.CameraType = Enum.CameraType.Custom
        if p.Character and p.Character:FindFirstChildOfClass("Humanoid") then
            Camera.CameraSubject = p.Character:FindFirstChildOfClass("Humanoid")
        end
    end
end)

local function atualizarListaPessoas()
    for _, v in pairs(containerLista:GetChildren()) do
        if v:IsA("TextButton") then v:Destroy() end
    end
    
    local y = 0
    for _, plr in pairs(Pessoas:GetPlayers()) do
        if plr ~= p then
            local btn = Instance.new("TextButton", containerLista)
            btn.Size = UDim2.new(1, 0, 0, 28)
            btn.Position = UDim2.new(0, 0, 0, y)
            btn.Text = plr.Name
            btn.Font = Enum.Font.Code
            btn.TextSize = 11
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
            btn.BorderSizePixel = 0
            btn.ZIndex = 2
            addCorner(btn, 6)
            
            btn.MouseButton1Click:Connect(function()
                jogadorSelecionado = plr
                miniLabel.Text = "Alvo: " .. plr.Name
                miniPainel.Visible = true
            end)
            y = y + 32
        end
    end
    containerLista.CanvasSize = UDim2.new(0, 0, 0, y)
end

Pessoas.PlayerAdded:Connect(atualizarListaPessoas)
Pessoas.PlayerRemoving:Connect(atualizarListaPessoas)
task.spawn(atualizarListaPessoas)

-- ==================== ABA MURDER MYSTERY (MM2) ====================
local mm2AssassinoBtn = criarBtn(abaMm2, "Ver Assassinos: OFF", UDim2.new(0, 0, 0, 0))
local mm2XerifeBtn = criarBtn(abaMm2, "Ver Xerifes/Heróis: OFF", UDim2.new(0, 0, 0, 35))
local mm2InocenteBtn = criarBtn(abaMm2, "Ver Inocentes: OFF", UDim2.new(0, 0, 0, 70))
local autoTpArmaBtn = criarBtn(abaMm2, "TP até a Arma: OFF", UDim2.new(0, 0, 0, 105))
local killAllFacaBtn = criarBtn(abaMm2, "Puxar Hitbox p/ Mim (Murder Kill All)", UDim2.new(0, 0, 0, 140))
local aimbotAssassinoBtn = criarBtn(abaMm2, "Aimbot Apenas no Assassino: OFF", UDim2.new(0, 0, 0, 175))

killAllFacaBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)

mm2AssassinoBtn.MouseButton1Click:Connect(function()
    mm2AssassinoAtivo = not mm2AssassinoAtivo
    mm2AssassinoBtn.Text = mm2AssassinoAtivo and "Ver Assassinos: ON" or "Ver Assassinos: OFF"
    mm2AssassinoBtn.BackgroundColor3 = mm2AssassinoAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
end)

mm2XerifeBtn.MouseButton1Click:Connect(function()
    mm2XerifeAtivo = not mm2XerifeAtivo
    mm2XerifeBtn.Text = mm2XerifeAtivo and "Ver Xerifes/Heróis: ON" or "Ver Xerifes/Heróis: OFF"
    mm2XerifeBtn.BackgroundColor3 = mm2XerifeAtivo and Color3.fromRGB(0, 100, 180) or Color3.fromRGB(0, 0, 0)
end)

mm2InocenteBtn.MouseButton1Click:Connect(function()
    mm2InocenteAtivo = not mm2InocenteAtivo
    mm2InocenteBtn.Text = mm2InocenteAtivo and "Ver Inocentes: ON" or "Ver Inocentes: OFF"
    mm2InocenteBtn.BackgroundColor3 = mm2InocenteAtivo and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(0, 0, 0)
end)

autoTpArmaBtn.MouseButton1Click:Connect(function()
    autoTpArmaAtivo = not autoTpArmaAtivo
    autoTpArmaBtn.Text = autoTpArmaAtivo and "TP até a Arma: ON" or "TP até a Arma: OFF"
    autoTpArmaBtn.BackgroundColor3 = autoTpArmaAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    jaAutoPegouArma = false
end)

aimbotAssassinoBtn.MouseButton1Click:Connect(function()
    aimbotApenasAssassinoAtivo = not aimbotApenasAssassinoAtivo
    aimbotAssassinoBtn.Text = aimbotApenasAssassinoAtivo and "Aimbot Apenas no Assassino: ON" or "Aimbot Apenas no Assassino: OFF"
    aimbotAssassinoBtn.BackgroundColor3 = aimbotApenasAssassinoAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    fovCircle.Visible = aimbotApenasAssassinoAtivo or aimbotAtivo
end)

killAllFacaBtn.MouseButton1Click:Connect(function()
    pcall(function()
        local meuChar = p.Character
        if not meuChar or not meuChar:FindFirstChild("HumanoidRootPart") then return end
        local hrp = meuChar.HumanoidRootPart
        
        local tool = meuChar:FindFirstChildOfClass("Tool")
        if not tool then
            local backpack = p:FindFirstChildOfClass("Backpack")
            if backpack then
                for _, item in pairs(backpack:GetChildren()) do
                    if item:IsA("Tool") and (item.Name:lower():find("knife") or item.Name:lower():find("faca")) then
                        tool = item
                        tool.Parent = meuChar
                        break
                    end
                end
            end
        end
        
        for _, plr in pairs(Pessoas:GetPlayers()) do
            if plr ~= p and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local alvHrp = plr.Character.HumanoidRootPart
                alvHrp.CFrame = hrp.CFrame * CFrame.new(0, 0, -2)
                alvHrp.CanCollide = false
            end
        end
        
        task.wait(0.1)
        if tool and tool.Parent == meuChar then tool:Activate() end
    end)
end)

local function detectarPapelMM2(plr)
    local char = plr.Character
    local backpack = plr:FindFirstChildOfClass("Backpack")
    local temFaca, temArma = false, false
    
    local function verificarObjeto(obj)
        if obj:IsA("Tool") then
            local nome = obj.Name:lower()
            if nome:find("knife") or nome:find("faca") then temFaca = true
            elseif nome:find("gun") or nome:find("revolver") or nome:find("arma") then temArma = true end
        end
    end
    
    if char then for _, v in pairs(char:GetChildren()) do verificarObjeto(v) end end
    if backpack then for _, v in pairs(backpack:GetChildren()) do verificarObjeto(v) end end
    
    if temFaca then return "Murderer"
    elseif temArma then return "Sheriff" end
    return "Innocent"
end

-- ==================== AUTOMATIZAÇÃO TP ARMA E CHAMS MM2 ====================
RunService.RenderStepped:Connect(function()
    -- TELEPORT AUTOMÁTICO SE A ARMA CAIR NO CHÃO
    if autoTpArmaAtivo then
        local arma = encontrarArmaNoChao()
        if arma and not jaAutoPegouArma then
            local meuChar = p.Character
            local hrp = meuChar and meuChar:FindFirstChild("HumanoidRootPart")
            if hrp then
                jaAutoPegouArma = true
                local cfOriginal = hrp.CFrame
                local cfArma = arma:IsA("BasePart") and arma.CFrame or arma:GetPivot()
                
                hrp.CFrame = cfArma + Vector3.new(0, 2, 0)
                task.wait(0.3)
                hrp.CFrame = cfOriginal
            end
        elseif not arma then
            jaAutoPegouArma = false
        end
    end

    -- CHAMS COLORIDO CORPO INTEIRO MM2
    for _, plr in pairs(Pessoas:GetPlayers()) do
        if plr ~= p and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local papel = detectarPapelMM2(plr)
            local deveMostrar = false
            local corPapel = Color3.fromRGB(255, 255, 255)
            local textoPapel = ""
            
            if papel == "Murderer" and mm2AssassinoAtivo then
                deveMostrar = true
                corPapel = Color3.fromRGB(255, 0, 0)
                textoPapel = "[ASSASSINO] " .. plr.Name
            elseif papel == "Sheriff" and mm2XerifeAtivo then
                deveMostrar = true
                corPapel = Color3.fromRGB(0, 120, 255)
                textoPapel = "[XERIFE/HERÓI] " .. plr.Name
            elseif papel == "Innocent" and mm2InocenteAtivo then
                deveMostrar = true
                corPapel = Color3.fromRGB(0, 255, 0)
                textoPapel = "[INOCENTE] " .. plr.Name
            end
            
            if deveMostrar then
                local box = mm2Boxes[plr]
                if not box or box.Parent ~= plr.Character then
                    if box then box:Destroy() end
                    box = Instance.new("Highlight")
                    box.Adornee = plr.Character
                    box.FillTransparency = 0.4 -- CORPO TOTALMENTE COLORIDO
                    box.OutlineTransparency = 0
                    box.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    box.Parent = plr.Character
                    mm2Boxes[plr] = box
                end
                box.FillColor = corPapel
                box.OutlineColor = Color3.fromRGB(255, 255, 255)
                
                local tag = mm2Tags[plr]
                local head = plr.Character:FindFirstChild("Head")
                if head and (not tag or tag.Parent ~= head) then
                    if tag then tag:Destroy() end
                    tag = Instance.new("BillboardGui")
                    tag.Size = UDim2.new(0, 160, 0, 40)
                    tag.StudsOffset = Vector3.new(0, 2.5, 0)
                    tag.AlwaysOnTop = true
                    tag.Adornee = head
                    
                    local txt = Instance.new("TextLabel", tag)
                    txt.Size = UDim2.new(1, 0, 1, 0)
                    txt.BackgroundTransparency = 1
                    txt.TextColor3 = Color3.fromRGB(255, 255, 255)
                    txt.TextStrokeTransparency = 0
                    txt.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                    txt.Font = Enum.Font.Code
                    txt.TextSize = 13
                    txt.Name = "LabelPapel"
                    
                    tag.Parent = head
                    mm2Tags[plr] = tag
                end
                
                if tag and tag:FindFirstChild("LabelPapel") then
                    tag.LabelPapel.Text = textoPapel
                    tag.LabelPapel.TextColor3 = corPapel
                end
            else
                if mm2Boxes[plr] then mm2Boxes[plr]:Destroy() mm2Boxes[plr] = nil end
                if mm2Tags[plr] then mm2Tags[plr]:Destroy() mm2Tags[plr] = nil end
            end
        else
            if mm2Boxes[plr] then mm2Boxes[plr]:Destroy() mm2Boxes[plr] = nil end
            if mm2Tags[plr] then mm2Tags[plr]:Destroy() mm2Tags[plr] = nil end
        end
    end
end)

-- ==================== ABA CONFIGURAR ====================
local lblKeybind = Instance.new("TextLabel", abaConfig)
lblKeybind.Size = UDim2.new(1, 0, 0, 20)
lblKeybind.Position = UDim2.new(0, 0, 0, 0)
lblKeybind.Text = "Atalho Teclado: NENHUM"
lblKeybind.Font = Enum.Font.Code
lblKeybind.TextSize = 12
lblKeybind.TextColor3 = Color3.fromRGB(255, 255, 0)
lblKeybind.BackgroundTransparency = 1
lblKeybind.ZIndex = 2

local btnDefinirKeybind = criarBtn(abaConfig, "Clique para definir Tecla", UDim2.new(0, 0, 0, 22))

btnDefinirKeybind.MouseButton1Click:Connect(function()
    aguardandoTecla = true
    btnDefinirKeybind.Text = "Pressione uma tecla..."
    btnDefinirKeybind.BackgroundColor3 = Color3.fromRGB(180, 180, 0)
end)

UIS.InputBegan:Connect(function(input, gameProcessed)
    if aguardandoTecla and input.UserInputType == Enum.UserInputType.Keyboard then
        teclaAtalho = input.KeyCode
        lblKeybind.Text = "Atalho Teclado: " .. tostring(teclaAtalho.Name)
        btnDefinirKeybind.Text = "Alterar Tecla"
        btnDefinirKeybind.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        aguardandoTecla = false
        return
    end

    if not gameProcessed and teclaAtalho and input.KeyCode == teclaAtalho then
        f.Visible = not f.Visible
    end
end)

local toggleTituloForaBtn = criarBtn(abaConfig, "Nome Fora do Painel: ON", UDim2.new(0, 0, 0, 57))
toggleTituloForaBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)

toggleTituloForaBtn.MouseButton1Click:Connect(function()
    tituloForaAtivo = not tituloForaAtivo
    titleButton.Visible = tituloForaAtivo
    toggleTituloForaBtn.Text = tituloForaAtivo and "Nome Fora do Painel: ON" or "Nome Fora do Painel: OFF"
    toggleTituloForaBtn.BackgroundColor3 = tituloForaAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
end)

local lblTemas = Instance.new("TextLabel", abaConfig)
lblTemas.Size = UDim2.new(1, 0, 0, 20)
lblTemas.Position = UDim2.new(0, 0, 0, 95)
lblTemas.Text = "--- ESTILOS DE TEMA ---"
lblTemas.Font = Enum.Font.Code
lblTemas.TextSize = 12
lblTemas.TextColor3 = Color3.fromRGB(255, 255, 255)
lblTemas.BackgroundTransparency = 1
lblTemas.ZIndex = 2

local btnTemaHacker = criarBtn(abaConfig, "💻 Tema Hacker", UDim2.new(0, 0, 0, 120))
local btnTemaMinecraft = criarBtn(abaConfig, "⛏️ Tema Minecraft", UDim2.new(0, 0, 0, 155))
local btnTemaTubers = criarBtn(abaConfig, "👺 Tema Tubers93", UDim2.new(0, 0, 0, 190))
local btnTemaPadrao = criarBtn(abaConfig, "🔄 Tema Padrão (RGB)", UDim2.new(0, 0, 0, 225))

btnTemaHacker.MouseButton1Click:Connect(function() aplicarTema("hacker") end)
btnTemaMinecraft.MouseButton1Click:Connect(function() aplicarTema("minecraft") end)
btnTemaTubers.MouseButton1Click:Connect(function() aplicarTema("tubers93") end)
btnTemaPadrao.MouseButton1Click:Connect(function() aplicarTema("padrao") end)

local corPainelBtn = criarBtn(abaConfig, "Cor do Painel: RGB 🌈", UDim2.new(0, 0, 0, 265))

local subMenuCores = Instance.new("Frame", abaConfig)
subMenuCores.Size = UDim2.new(1, 0, 0, 190)
subMenuCores.Position = UDim2.new(0, 0, 0, 297)
subMenuCores.BackgroundTransparency = 1
subMenuCores.Visible = false
subMenuCores.ZIndex = 2

local function criarSubCorBtn(txt, yPos, corDestino, isRgb)
    local b = Instance.new("TextButton", subMenuCores)
    b.Size = UDim2.new(1, 0, 0, 28)
    b.Position = UDim2.new(0, 0, 0, yPos)
    b.Text = txt
    b.Font = Enum.Font.Code
    b.TextSize = 11
    b.TextColor3 = Color3.fromRGB(255, 255, 255)
    b.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    b.BorderSizePixel = 0
    b.ZIndex = 2
    addCorner(b, 6)
    table.insert(todosBotoes, b)
    
    b.MouseButton1Click:Connect(function()
        if isRgb then
            painelRgbAtivo = true
            corPainelBtn.Text = "Cor do Painel: RGB 🌈"
        else
            painelRgbAtivo = false
            corPainelAtual = corDestino
            corPainelBtn.Text = "Cor do Painel: " .. txt
        end
        subMenuCores.Visible = false
    end)
    return b
end

criarSubCorBtn("🔴 Vermelho", 0, Color3.fromRGB(255, 0, 0), false)
criarSubCorBtn("🟠 Laranja", 32, Color3.fromRGB(255, 128, 0), false)
criarSubCorBtn("🟡 Amarelo", 64, Color3.fromRGB(255, 255, 0), false)
criarSubCorBtn("🟢 Verde", 96, Color3.fromRGB(0, 255, 0), false)
criarSubCorBtn("🔵 Azul", 128, Color3.fromRGB(0, 128, 255), false)
criarSubCorBtn("🌈 RGB", 160, nil, true)

corPainelBtn.MouseButton1Click:Connect(function()
    subMenuCores.Visible = not subMenuCores.Visible
end)

-- ==================== ABA JOGADOR ====================
local speedLabel = criarBtn(abaJogador, "Speed: 16", UDim2.new(0, 0, 0, 0))
local speedMenos = criarBtn(abaJogador, "-", UDim2.new(0, 0, 0, 32), UDim2.new(0, 105, 0, 28))
local speedMais = criarBtn(abaJogador, "+", UDim2.new(0, 112, 0, 32), UDim2.new(0, 105, 0, 28))

local jumpLabel = criarBtn(abaJogador, "Jump: 50", UDim2.new(0, 0, 0, 65))
local jumpMenos = criarBtn(abaJogador, "-", UDim2.new(0, 0, 0, 97), UDim2.new(0, 105, 0, 28))
local jumpMais = criarBtn(abaJogador, "+", UDim2.new(0, 112, 0, 97), UDim2.new(0, 105, 0, 28))

local hitboxLabel = criarBtn(abaJogador, "Hitbox Size: 2", UDim2.new(0, 0, 0, 130))
local hitboxMenos = criarBtn(abaJogador, "-", UDim2.new(0, 0, 0, 162), UDim2.new(0, 105, 0, 28))
local hitboxMais = criarBtn(abaJogador, "+", UDim2.new(0, 112, 0, 162), UDim2.new(0, 105, 0, 28))

local noclipBtn = criarBtn(abaJogador, "Noclip: OFF", UDim2.new(0, 0, 0, 195))
local flyBtn = criarBtn(abaJogador, "Fly: OFF", UDim2.new(0, 0, 0, 227))
local invisivelBtn = criarBtn(abaJogador, "Invisível: OFF", UDim2.new(0, 0, 0, 259))
local espBtn = criarBtn(abaJogador, "ESP Box + @: OFF", UDim2.new(0, 0, 0, 291))
local espLineBtn = criarBtn(abaJogador, "ESP Line Players: OFF", UDim2.new(0, 0, 0, 323)) -- NOVO BOTÃO PEDIDO
local aimbotBtn = criarBtn(abaJogador, "Aimbot: OFF", UDim2.new(0, 0, 0, 355))
local aimbotParteBtn = criarBtn(abaJogador, "Alvo Aimbot: Cabeça", UDim2.new(0, 0, 0, 387))

local ultBlueLockBtn = criarBtn(abaJogador, "Ult Blue Lock (Jamal Dance)", UDim2.new(0, 0, 0, 422))
ultBlueLockBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)

local ultLabelLista = Instance.new("TextLabel", abaJogador)
ultLabelLista.Size = UDim2.new(1, 0, 0, 20)
ultLabelLista.Position = UDim2.new(0, 0, 0, 455)
ultLabelLista.Text = "Selecione o Alvo da Ult abaixo:"
ultLabelLista.Font = Enum.Font.Code
ultLabelLista.TextSize = 11
ultLabelLista.TextColor3 = Color3.fromRGB(255, 255, 0)
ultLabelLista.BackgroundTransparency = 1
ultLabelLista.ZIndex = 2

local ultContainerLista = Instance.new("ScrollingFrame", abaJogador)
ultContainerLista.Size = UDim2.new(1, 0, 0, 180)
ultContainerLista.Position = UDim2.new(0, 0, 0, 478)
ultContainerLista.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
ultContainerLista.BorderSizePixel = 1
ultContainerLista.BorderColor3 = Color3.fromRGB(100, 0, 0)
ultContainerLista.ScrollBarThickness = 4
ultContainerLista.CanvasSize = UDim2.new(0, 0, 0, 0)
ultContainerLista.ZIndex = 2
addCorner(ultContainerLista, 6)

local function atualizarListaUlt()
    for _, v in pairs(ultContainerLista:GetChildren()) do
        if v:IsA("TextButton") then v:Destroy() end
    end
    
    local y = 0
    for _, plr in pairs(Pessoas:GetPlayers()) do
        if plr ~= p then
            local btn = Instance.new("TextButton", ultContainerLista)
            btn.Size = UDim2.new(1, 0, 0, 26)
            btn.Position = UDim2.new(0, 0, 0, y)
            btn.Text = plr.Name .. (alvoUltSelecionado == plr and " [SELECIONADO]" or "")
            btn.Font = Enum.Font.Code
            btn.TextSize = 11
            btn.TextColor3 = alvoUltSelecionado == plr and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 255, 255)
            btn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
            btn.BorderSizePixel = 0
            btn.ZIndex = 2
            addCorner(btn, 4)
            
            btn.MouseButton1Click:Connect(function()
                alvoUltSelecionado = plr
                atualizarListaUlt()
            end)
            y = y + 30
        end
    end
    ultContainerLista.CanvasSize = UDim2.new(0, 0, 0, y)
end

Pessoas.PlayerAdded:Connect(atualizarListaUlt)
Pessoas.PlayerRemoving:Connect(atualizarListaUlt)
task.spawn(atualizarListaUlt)

ultBlueLockBtn.MouseButton1Click:Connect(function()
    if not alvoUltSelecionado or not alvoUltSelecionado.Character then return end
    
    local meuChar = p.Character
    local alvoChar = alvoUltSelecionado.Character
    if not meuChar or not meuChar:FindFirstChild("HumanoidRootPart") or not alvoChar:FindFirstChild("HumanoidRootPart") then return end
    
    local hrpMeu = meuChar.HumanoidRootPart
    local hrpAlvo = alvoChar.HumanoidRootPart
    local cam = Workspace.CurrentCamera
    
    local posicaoOriginal = hrpMeu.CFrame
    hrpMeu.CFrame = hrpAlvo.CFrame * CFrame.new(0, 0, 3)
    
    local animTrack = nil
    pcall(function()
        local humanoid = meuChar:FindFirstChildOfClass("Humanoid")
        if humanoid then
            local animator = humanoid:FindFirstChildOfClass("Animator") or Instance.new("Animator", humanoid)
            local anim = Instance.new("Animation")
            anim.AnimationId = "rbxassetid://507771019"
            animTrack = animator:LoadAnimation(anim)
            animTrack:Play()
        end
    end)
    
    local tempoInicial = tick()
    local conexaoUltCam
    conexaoUltCam = RunService.RenderStepped:Connect(function()
        if tick() - tempoInicial < 3.5 then
            cam.CameraType = Enum.CameraType.Scriptable
            cam.CFrame = CFrame.new(hrpMeu.Position + Vector3.new(0, 3, -6), hrpMeu.Position)
        else
            cam.CameraType = Enum.CameraType.Custom
            cam.CameraSubject = meuChar:FindFirstChildOfClass("Humanoid")
            if conexaoUltCam then conexaoUltCam:Disconnect() end
        end
    end)
    
    task.wait(2.0)
    FlingNoAlvo(alvoUltSelecionado)
    
    if hrpMeu and hrpMeu.Parent then
        hrpMeu.CFrame = posicaoOriginal
        hrpMeu.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
        hrpMeu.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
    end
    
    if animTrack then pcall(function() animTrack:Stop() end) end
end)

-- ==================== ABA SCRIPTS ====================
local infit = criarBtn(abaScripts, "Infit Jump: OFF", UDim2.new(0, 0, 0, 0))
local touchFlingBtn = criarBtn(abaScripts, "Touch Fling: OFF", UDim2.new(0, 0, 0, 32))
local rgbBonecoBtn = criarBtn(abaScripts, "RGB Boneco (RBLGBT): OFF", UDim2.new(0, 0, 0, 64))

local dropkickBtn = criarBtn(abaScripts, "DropKick: Executar", UDim2.new(0, 0, 0, 96))
dropkickBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
dropkickBtn.TextColor3 = Color3.fromRGB(255, 200, 200)

local travarServeBtn = criarBtn(abaScripts, "Cuidado vc vai ser lagado☢️: OFF", UDim2.new(0, 0, 0, 128))
local transMapBtn = criarBtn(abaScripts, "Transparência: OFF", UDim2.new(0, 0, 0, 160))
local clickTpBtn = criarBtn(abaScripts, "Click TP (Tool)", UDim2.new(0, 0, 0, 192))
local seletorAlvoBtn = criarBtn(abaScripts, "Seletor (Tool)", UDim2.new(0, 0, 0, 224))

local walkFlingBtn = criarBtn(abaScripts, "WalkFling: OFF", UDim2.new(0, 0, 0, 256))

local c00lkiddThemeBtn = criarBtn(abaScripts, "c00lkidd Fake: OFF", UDim2.new(0, 0, 0, 288))
c00lkiddThemeBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)

local allItensBtn = criarBtn(abaScripts, "All Itens", UDim2.new(0, 0, 0, 320))
allItensBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
allItensBtn.TextColor3 = Color3.fromRGB(255, 255, 255)

local excluir = criarBtn(abaScripts, "Excluir Painel", UDim2.new(0, 0, 0, 352))
excluir.BackgroundColor3 = Color3.fromRGB(120, 20, 20)

task.spawn(function()
    while true do
        if painelRgbAtivo and temaAtual == "padrao" then
            local hue = tick() % 5 / 5
            local corRgb = Color3.fromHSV(hue, 1, 1)
            strokeMain.Color = corRgb
            titulo.TextColor3 = corRgb
        elseif not painelRgbAtivo and temaAtual == "padrao" then
            strokeMain.Color = corPainelAtual
            titulo.TextColor3 = corPainelAtual
        end
        task.wait()
    end
end)

local function hum()
    return p.Character and p.Character:FindFirstChildOfClass("Humanoid")
end

fechar.MouseButton1Click:Connect(function() f.Visible = false end)

walkFlingBtn.MouseButton1Click:Connect(function()
    walkFlingAtivado = not walkFlingAtivado
    walkFlingBtn.Text = walkFlingAtivado and "WalkFling: ON" or "WalkFling: OFF"
    walkFlingBtn.BackgroundColor3 = walkFlingAtivado and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    
    if walkFlingAtivado then
        noclipAtivo = true
        noclipBtn.Text = "Noclip: ON"
        noclipBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
        
        local hrp = p.Character and p.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            conexaoWalkFling = hrp.Touched:Connect(function(hit)
                if walkFlingAtivado and hit.Parent and hit.Parent:FindFirstChild("Humanoid") and hit.Parent.Name ~= p.Name then
                    local jogadorAlvo = Pessoas:GetPlayerFromCharacter(hit.Parent)
                    if jogadorAlvo then FlingNoAlvo(jogadorAlvo) end
                end
            end)
        end
    else
        if conexaoWalkFling then
            conexaoWalkFling:Disconnect()
            conexaoWalkFling = nil
        end
    end
end)

local function aplicarHitboxDireta()
    for _, otherPlayer in pairs(Pessoas:GetPlayers()) do
        if otherPlayer ~= p and otherPlayer.Character then
            local hrp = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
            local h = otherPlayer.Character:FindFirstChildOfClass("Humanoid")
            
            if hrp and h and h.Health > 0 then
                if not tamanhosOriginaisHrps[hrp] then
                    tamanhosOriginaisHrps[hrp] = hrp.Size
                end
                
                if hitboxSize > 2 then
                    hrp.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
                    hrp.Transparency = 0.7
                    hrp.Color = Color3.fromRGB(255, 0, 0)
                    hrp.Material = Enum.Material.Neon
                    hrp.CanCollide = false
                else
                    hrp.Size = tamanhosOriginaisHrps[hrp]
                    hrp.Transparency = 1
                end
            end
        end
    end
end

hitboxMais.MouseButton1Click:Connect(function()
    hitboxSize = math.min(hitboxSize + 10, 400)
    hitboxLabel.Text = "Hitbox Size: " .. hitboxSize
    aplicarHitboxDireta()
end)

hitboxMenos.MouseButton1Click:Connect(function()
    hitboxSize = math.max(hitboxSize - 10, 2)
    hitboxLabel.Text = "Hitbox Size: " .. hitboxSize
    aplicarHitboxDireta()
end)

aimbotBtn.MouseButton1Click:Connect(function()
    aimbotAtivo = not aimbotAtivo
    aimbotBtn.Text = aimbotAtivo and "Aimbot: ON" or "Aimbot: OFF"
    aimbotBtn.BackgroundColor3 = aimbotAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    if not aimbotApenasAssassinoAtivo then fovCircle.Visible = aimbotAtivo end
end)

aimbotParteBtn.MouseButton1Click:Connect(function()
    if aimbotParteAlvo == "Head" then
        aimbotParteAlvo = "Torso"
        aimbotParteBtn.Text = "Alvo Aimbot: Tronco"
    else
        aimbotParteAlvo = "Head"
        aimbotParteBtn.Text = "Alvo Aimbot: Cabeça"
    end
end)

local function obterAlvoAimbot()
    local melhorAlvo = nil
    local menorDistancia = math.huge
    local meuChar = p.Character
    if not meuChar or not meuChar:FindFirstChild("HumanoidRootPart") then return nil end
    local centroTela = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    for _, plr in pairs(Pessoas:GetPlayers()) do
        if plr ~= p and plr.Character then
            local h = plr.Character:FindFirstChildOfClass("Humanoid")
            if h and h.Health > 0 then
                if aimbotApenasAssassinoAtivo and detectarPapelMM2(plr) ~= "Murderer" then continue end

                local parteAlvoObj = nil
                if aimbotParteAlvo == "Head" then
                    parteAlvoObj = plr.Character:FindFirstChild("Head")
                else
                    parteAlvoObj = plr.Character:FindFirstChild("UpperTorso") or plr.Character:FindFirstChild("Torso")
                end
                
                if parteAlvoObj then
                    local screenPos, onScreen = Camera:WorldToViewportPoint(parteAlvoObj.Position)
                    if onScreen then
                        local distTela = (Vector2.new(screenPos.X, screenPos.Y) - centroTela).Magnitude
                        if distTela <= fovCircle.Radius and distTela < menorDistancia then
                            menorDistancia = distTela
                            melhorAlvo = parteAlvoObj
                        end
                    end
                end
            end
        end
    end
    return melhorAlvo
end

seletorAlvoBtn.MouseButton1Click:Connect(function()
    local backpack = p:FindFirstChildOfClass("Backpack")
    if not backpack then return end
    if p.Character and p.Character:FindFirstChild("Seletor Alvo") or backpack:FindFirstChild("Seletor Alvo") then return end

    local tool = Instance.new("Tool")
    tool.Name = "Seletor Alvo"
    tool.RequiresHandle = false
    tool.Parent = backpack

    tool.Activated:Connect(function()
        if Mouse.Target and Mouse.Target.Parent then
            local charEncontrado = Mouse.Target.Parent
            local playerEncontrado = Pessoas:GetPlayerFromCharacter(charEncontrado)
            if not playerEncontrado and charEncontrado.Parent then
                playerEncontrado = Pessoas:GetPlayerFromCharacter(charEncontrado.Parent)
            end
            
            if playerEncontrado and playerEncontrado ~= p then
                jogadorSelecionado = playerEncontrado
                miniLabel.Text = "Alvo: " .. playerEncontrado.Name
                miniPainel.Visible = true
            end
        end
    end)
end)

c00lkiddThemeBtn.MouseButton1Click:Connect(function()
    c00lkiddAtivo = not c00lkiddAtivo
    c00lkiddThemeBtn.Text = c00lkiddAtivo and "c00lkidd Fake: ON" or "c00lkidd Fake: OFF"
    c00lkiddThemeBtn.BackgroundColor3 = c00lkiddAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    
    if c00lkiddAtivo then
        pcall(function()
            local sky = Workspace:FindFirstChildOfClass("Sky") or Instance.new("Sky", Workspace)
            sky.SkyboxBk = "rbxassetid://0"
            sky.SkyboxDn = "rbxassetid://0"
            sky.SkyboxFt = "rbxassetid://0"
            sky.SkyboxLf = "rbxassetid://0"
            sky.SkyboxRt = "rbxassetid://0"
            sky.SkyboxUp = "rbxassetid://0"
            Workspace.Gravity = 50
        end)
    else
        pcall(function() Workspace.Gravity = 196.2 end)
    end
end)

rgbBonecoBtn.MouseButton1Click:Connect(function()
    rgbBonecoAtivo = not rgbBonecoAtivo
    rgbBonecoBtn.Text = rgbBonecoAtivo and "RGB Boneco (RBLGBT): ON" or "RGB Boneco (RBLGBT): OFF"
    rgbBonecoBtn.BackgroundColor3 = rgbBonecoAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
end)

task.spawn(function()
    while true do
        if rgbBonecoAtivo and p.Character then
            local hue = tick() % 5 / 5
            local corRgb = Color3.fromHSV(hue, 1, 1)
            for _, part in pairs(p.Character:GetDescendants()) do
                if part:IsA("BasePart") then part.Color = corRgb end
            end
        end
        task.wait(0.1)
    end
end)

invisivelBtn.MouseButton1Click:Connect(function()
    invisivelAtivo = not invisivelAtivo
    invisivelBtn.Text = invisivelAtivo and "Invisível: ON" or "Invisível: OFF"
    invisivelBtn.BackgroundColor3 = invisivelAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    
    local char = p.Character
    if char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") or part:IsA("Decal") then
                if invisivelAtivo then
                    if not transparenciaPersonagemOriginal[part] then
                        transparenciaPersonagemOriginal[part] = part.Transparency
                    end
                    part.Transparency = 1
                else
                    if transparenciaPersonagemOriginal[part] then
                        part.Transparency = transparenciaPersonagemOriginal[part]
                    else
                        part.Transparency = 0
                    end
                end
            end
        end
    end
end)

dropkickBtn.MouseButton1Click:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/platinww/CrustyMain/refs/heads/main/universal/DropKick.lua"))()
        dropkickBtn.Text = "DropKick: Executado!"
        task.delay(2, function() dropkickBtn.Text = "DropKick: Executar" end)
    end)
end)

speedMais.MouseButton1Click:Connect(function()
    ws = math.min(ws + 10, 200)
    speedLabel.Text = "Speed: " .. ws
end)

speedMenos.MouseButton1Click:Connect(function()
    ws = math.max(ws - 10, 16)
    speedLabel.Text = "Speed: " .. ws
end)

jumpMais.MouseButton1Click:Connect(function()
    jp = math.min(jp + 25, 400)
    jumpLabel.Text = "Jump: " .. jp
    if hum() then hum().JumpPower = jp end
end)

jumpMenos.MouseButton1Click:Connect(function()
    jp = math.max(jp - 25, 50)
    jumpLabel.Text = "Jump: " .. jp
    if hum() then hum().JumpPower = jp end
end)

noclipBtn.MouseButton1Click:Connect(function()
    noclipAtivo = not noclipAtivo
    noclipBtn.Text = noclipAtivo and "Noclip: ON" or "Noclip: OFF"
    noclipBtn.BackgroundColor3 = noclipAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
end)

flyBtn.MouseButton1Click:Connect(function()
    flyAtivo = not flyAtivo
    flyBtn.Text = flyAtivo and "Fly: ON" or "Fly: OFF"
    flyBtn.BackgroundColor3 = flyAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    if flyAtivo then
        pcall(function() loadstring(game:HttpGet("https://pastebin.com/raw/TV83kUPv", true))() end)
    end
end)

touchFlingBtn.MouseButton1Click:Connect(function()
    touchFlingAtivo = not touchFlingAtivo
    touchFlingBtn.Text = touchFlingAtivo and "Touch Fling: ON" or "Touch Fling: OFF"
    touchFlingBtn.BackgroundColor3 = touchFlingAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    pcall(function()
        if touchFlingAtivo then loadstring(game:HttpGet("https://pastebin.com/raw/LgZwZ7ZB", true))() end
    end)
end)

-- ==================== SISTEMA DE ESP & LINE ESP ====================
local function atualizarESP()
    for _, plr in pairs(Pessoas:GetPlayers()) do
        if plr ~= p then
            if espAtivo and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local box = espBoxes[plr]
                if not box or box.Parent ~= plr.Character then
                    if box then box:Destroy() end
                    box = Instance.new("Highlight")
                    box.FillTransparency = 1
                    box.OutlineTransparency = 0
                    box.OutlineColor = Color3.fromRGB(255, 0, 0)
                    box.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    box.Adornee = plr.Character
                    box.Parent = plr.Character
                    espBoxes[plr] = box
                end
                
                local tag = espTags[plr]
                local head = plr.Character:FindFirstChild("Head")
                if head and (not tag or tag.Parent ~= head) then
                    if tag then tag:Destroy() end
                    tag = Instance.new("BillboardGui")
                    tag.Size = UDim2.new(0, 100, 0, 40)
                    tag.StudsOffset = Vector3.new(0, 2.5, 0)
                    tag.AlwaysOnTop = true
                    tag.Adornee = head
                    
                    local txt = Instance.new("TextLabel", tag)
                    txt.Size = UDim2.new(1, 0, 1, 0)
                    txt.BackgroundTransparency = 1
                    txt.Text = "@" .. plr.Name
                    txt.TextColor3 = Color3.fromRGB(255, 255, 255)
                    txt.TextStrokeTransparency = 0
                    txt.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                    txt.Font = Enum.Font.Code
                    txt.TextSize = 14
                    
                    tag.Parent = head
                    espTags[plr] = tag
                end
            else
                if espBoxes[plr] then espBoxes[plr]:Destroy() espBoxes[plr] = nil end
                if espTags[plr] then espTags[plr]:Destroy() espTags[plr] = nil end
            end

            -- DESENHO DE LINHA ESP VERMELHA
            if espLineAtivo and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                local line = espTracerLines[plr]
                if not line then
                    line = Drawing.new("Line")
                    line.Thickness = 1.5
                    line.Color = Color3.fromRGB(255, 0, 0)
                    line.Transparency = 1
                    espTracerLines[plr] = line
                end
                
                local hrp = plr.Character.HumanoidRootPart
                local screenPos, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                
                if onScreen then
                    line.From = Vector2.new(Camera.ViewportSize.X / 2, 0) -- TOPO DA TELA
                    line.To = Vector2.new(screenPos.X, screenPos.Y)
                    line.Visible = true
                else
                    line.Visible = false
                end
            else
                if espTracerLines[plr] then
                    espTracerLines[plr].Visible = false
                end
            end
        end
    end
end

espBtn.MouseButton1Click:Connect(function()
    espAtivo = not espAtivo
    espBtn.Text = espAtivo and "ESP Box + @: ON" or "ESP Box + @: OFF"
    espBtn.BackgroundColor3 = espAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    if not espAtivo then
        for _, b in pairs(espBoxes) do if b then b:Destroy() end end
        for _, t in pairs(espTags) do if t then t:Destroy() end end
        espBoxes = {}
        espTags = {}
    end
end)

espLineBtn.MouseButton1Click:Connect(function()
    espLineAtivo = not espLineAtivo
    espLineBtn.Text = espLineAtivo and "ESP Line Players: ON" or "ESP Line Players: OFF"
    espLineBtn.BackgroundColor3 = espLineAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    if not espLineAtivo then
        for _, line in pairs(espTracerLines) do
            if line then line.Visible = false end
        end
    end
end)

infit.MouseButton1Click:Connect(function()
    infJumpAtivo = not infJumpAtivo
    infit.Text = infJumpAtivo and "Infit Jump: ON" or "Infit Jump: OFF"
    infit.BackgroundColor3 = infJumpAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
end)

UIS.JumpRequest:Connect(function()
    if infJumpAtivo and hum() then hum():ChangeState(Enum.HumanoidStateType.Jumping) end
end)

travarServeBtn.MouseButton1Click:Connect(function()
    travarServeAtivo = not travarServeAtivo
    travarServeBtn.Text = travarServeAtivo and "Cuidado vc vai ser lagado☢️: ON" or "Cuidado vc vai ser lagado☢️: OFF"
    travarServeBtn.BackgroundColor3 = travarServeAtivo and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)
    
    if travarServeAtivo then
        tamanhosOriginaisParts = {}
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                local isPlayerChar = false
                for _, pl in pairs(Pessoas:GetPlayers()) do
                    if pl.Character and obj:IsDescendantOf(pl.Character) then
                        isPlayerChar = true
                        break
                    end
                end
                if not isPlayerChar and obj.Size.Magnitude < 50 then
                    tamanhosOriginaisParts[obj] = obj.Size
                    obj.Size = Vector3.new(30, 30, 30)
                end
            end
        end
    else
        for obj, sz in pairs(tamanhosOriginaisParts) do
            if obj and obj.Parent then obj.Size = sz end
        end
        tamanhosOriginaisParts = {}
    end
end)

transMapBtn.MouseButton1Click:Connect(function()
    transparenciaAtiva = not transparenciaAtiva
    transMapBtn.Text = transparenciaAtiva and "Transparência: ON" or "Transparência: OFF"
    transMapBtn.BackgroundColor3 = transparenciaAtiva and Color3.fromRGB(180, 0, 0) or Color3.fromRGB(0, 0, 0)

    if transparenciaAtiva then
        originalTransparencias = {}
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                local isPlayerChar = false
                for _, pl in pairs(Pessoas:GetPlayers()) do
                    if pl.Character and obj:IsDescendantOf(pl.Character) then
                        isPlayerChar = true
                        break
                    end
                end
                if not isPlayerChar and obj.Name ~= "HumanoidRootPart" then
                    originalTransparencias[obj] = obj.Transparency
                    obj.Transparency = 0.9
                end
            end
        end
    else
        for obj, trans in pairs(originalTransparencias) do
            if obj and obj.Parent then obj.Transparency = trans end
        end
        originalTransparencias = {}
    end
end)

allItensBtn.MouseButton1Click:Connect(function()
    pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Ahma174/Fake-Gamepasses/refs/heads/main/V4"))()
    end)
end)

clickTpBtn.MouseButton1Click:Connect(function()
    local backpack = p:FindFirstChildOfClass("Backpack")
    if not backpack then return end
    if p.Character and p.Character:FindFirstChild("Click TP") or backpack:FindFirstChild("Click TP") then return end

    local tool = Instance.new("Tool")
    tool.Name = "Click TP"
    tool.RequiresHandle = false
    tool.Parent = backpack

    tool.Activated:Connect(function()
        local hrp = p.Character and p.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 3, 0))
        end
    end)
end)

RunService.RenderStepped:Connect(function()
    local char = p.Character
    local h = hum()

    if h then
        if ws > 16 then h.WalkSpeed = ws end
        if jp > 50 then h.JumpPower = jp end
    end

    if noclipAtivo and char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") then part.CanCollide = false end
        end
    end

    if hitboxSize > 2 then aplicarHitboxDireta() end

    if espAtivo or espLineAtivo then
        atualizarESP()
    end

    if aimbotAtivo or aimbotApenasAssassinoAtivo then
        fovCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
        local alvoParte = obterAlvoAimbot()
        if alvoParte then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, alvoParte.Position)
        end
    else
        if not aimbotAtivo and not aimbotApenasAssassinoAtivo then
            fovCircle.Visible = false
        end
    end

    if espectandoAtivo and jogadorSelecionado and jogadorSelecionado.Character then
        local targetHumanoid = jogadorSelecionado.Character:FindFirstChildOfClass("Humanoid")
        if targetHumanoid then
            Camera.CameraType = Enum.CameraType.Custom
            Camera.CameraSubject = targetHumanoid
        end
    end
end)

excluir.MouseButton1Click:Connect(function()
    for _, l in pairs(espTracerLines) do if l then l:Remove() end end
    fovCircle:Remove()
    gui:Destroy()
end)
