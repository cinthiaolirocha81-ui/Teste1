-- 🐸 Frog Hub - Simplificado com Float v2 e Copiar Link
local keyNeeded = "Hubfrog123438"
local linkKey = "https://rkns.link/xi35f"

local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")

-- GUI principal
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 320, 0, 200)
Frame.Position = UDim2.new(0.3, 0, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
Frame.Active = true
Frame.Draggable = true
Frame.ClipsDescendants = true
Frame.BackgroundTransparency = 0.05

-- Rainbow animado
task.spawn(function()
    while task.wait() do
        Frame.BackgroundColor3 = Color3.fromHSV((tick() * 0.2) % 1, 0.6, 0.2)
    end
end)

-- Título
local Titulo = Instance.new("TextLabel", Frame)
Titulo.Size = UDim2.new(1, -20, 0, 50)
Titulo.Position = UDim2.new(0, 10, 0, 10)
Titulo.BackgroundTransparency = 1
Titulo.Text = "🐸 Frog Hub"
Titulo.TextColor3 = Color3.fromRGB(0, 255, 150)
Titulo.Font = Enum.Font.GothamBlack
Titulo.TextSize = 30
Titulo.TextScaled = true

-- Caixa de Key
local CaixaKey = Instance.new("TextBox", Frame)
CaixaKey.Size = UDim2.new(1, -20, 0, 40)
CaixaKey.Position = UDim2.new(0, 10, 0, 70)
CaixaKey.PlaceholderText = "Digite a Key..."
CaixaKey.TextScaled = true
CaixaKey.TextColor3 = Color3.fromRGB(255,255,255)
CaixaKey.BackgroundColor3 = Color3.fromRGB(40,40,50)
CaixaKey.BorderSizePixel = 0

-- Botão Copiar Link
local CopiarLink = Instance.new("TextButton", Frame)
CopiarLink.Size = UDim2.new(1, -20, 0, 30)
CopiarLink.Position = UDim2.new(0, 10, 0, 120)
CopiarLink.Text = "📋 Copiar Link"
CopiarLink.TextScaled = true
CopiarLink.BackgroundColor3 = Color3.fromRGB(0,255,150)
CopiarLink.TextColor3 = Color3.fromRGB(20,20,20)
CopiarLink.BorderSizePixel = 0
CopiarLink.MouseButton1Click:Connect(function()
    setclipboard(linkKey)
end)

-- Frame funções
local FuncoesContainer = Instance.new("ScrollingFrame", Frame)
FuncoesContainer.Size = UDim2.new(1, -20, 0, 60)
FuncoesContainer.Position = UDim2.new(0, 10, 0, 160)
FuncoesContainer.BackgroundTransparency = 1
FuncoesContainer.CanvasSize = UDim2.new(0,0,0,0)
FuncoesContainer.ScrollBarThickness = 6
FuncoesContainer.Visible = false

local UIListLayout = Instance.new("UIListLayout", FuncoesContainer)
UIListLayout.Padding = UDim.new(0,5)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    FuncoesContainer.CanvasSize = UDim2.new(0,0,0,UIListLayout.AbsoluteContentSize.Y)
end)

-- Variáveis
local floatV2Bloco
local lastClick = 0

-- Função criar botões
local function criarBotao(texto, func)
    local btn = Instance.new("TextButton", FuncoesContainer)
    btn.Size = UDim2.new(1,0,0,35)
    btn.Text = texto
    btn.TextScaled = true
    btn.BackgroundColor3 = Color3.fromRGB(0,255,150)
    btn.TextColor3 = Color3.fromRGB(20,20,20)
    btn.BorderSizePixel = 0
    btn.MouseButton1Click:Connect(func)
end

-- Checar Key
CaixaKey.FocusLost:Connect(function()
    if CaixaKey.Text == keyNeeded then
        FuncoesContainer.Visible = true

        -- Float v2 com duplo clique para parar
        criarBotao("💠 Float v2", function()
            local now = tick()
            if now - lastClick < 0.3 then
                -- Duplo clique: para o float
                if floatV2Bloco then
                    floatV2Bloco:Destroy()
                    floatV2Bloco = nil
                end
                return
            end
            lastClick = now

            -- Se não existir, cria o bloco invisível
            if not floatV2Bloco then
                floatV2Bloco = Instance.new("Part", workspace)
                floatV2Bloco.Size = Vector3.new(6,1,6)
                floatV2Bloco.Anchored = true
                floatV2Bloco.CanCollide = true
                floatV2Bloco.Transparency = 1
                floatV2Bloco.Position = player.Character.HumanoidRootPart.Position - Vector3.new(0,3,0)

                task.spawn(function()
                    while floatV2Bloco and task.wait(0.1) do
                        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            floatV2Bloco.Position = player.Character.HumanoidRootPart.Position - Vector3.new(0,3,0)
                        end
                    end
                end)
            end
        end)

    else
        CaixaKey.Text = "❌ Key Errada"
    end
end)
