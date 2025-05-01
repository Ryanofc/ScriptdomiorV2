-- Função para gerar a Key do dia com formato de unidade
local function gerarKey()
    local data = os.date("*t")
    
    -- Formata o dia e mês com zero à esquerda se necessário
    local dia = data.day < 10 and "0" .. data.day or tostring(data.day)
    local mes = data.month < 10 and "0" .. data.month or tostring(data.month)
    
    -- Retorna a Key no formato: dia + mês + ano
    return dia .. mes .. tostring(data.year)
end

local keyCorreta = gerarKey()

-- GUI de Key
local player = game.Players.LocalPlayer
local guiKey = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
guiKey.Name = "KeyGUI"
guiKey.ResetOnSpawn = false

local frameKey = Instance.new("Frame", guiKey)
frameKey.Size = UDim2.new(0, 300, 0, 180)
frameKey.Position = UDim2.new(0.5, -150, 0.5, -90)
frameKey.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frameKey.Active = true
frameKey.Draggable = true

local uiList = Instance.new("UIListLayout", frameKey)
uiList.Padding = UDim.new(0, 10)
uiList.FillDirection = Enum.FillDirection.Vertical
uiList.HorizontalAlignment = Enum.HorizontalAlignment.Center
uiList.VerticalAlignment = Enum.VerticalAlignment.Top

-- Caixa de texto para digitar a Key
local inputBox = Instance.new("TextBox", frameKey)
inputBox.Size = UDim2.new(0, 260, 0, 40)
inputBox.PlaceholderText = "Digite a Key"
inputBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
inputBox.TextColor3 = Color3.new(1, 1, 1)
inputBox.Font = Enum.Font.Gotham
inputBox.TextSize = 16

-- Botão Confirmar
local btnConfirmar = Instance.new("TextButton", frameKey)
btnConfirmar.Size = UDim2.new(0, 260, 0, 40)
btnConfirmar.Text = "Confirmar"
btnConfirmar.BackgroundColor3 = Color3.fromRGB(60, 180, 60)
btnConfirmar.TextColor3 = Color3.new(1, 1, 1)
btnConfirmar.Font = Enum.Font.GothamBold
btnConfirmar.TextSize = 16

btnConfirmar.MouseButton1Click:Connect(function()
    if inputBox.Text == keyCorreta then
        guiKey:Destroy()
        gui.Enabled = true -- Mostra a GUI principal
    else
        inputBox.Text = "Key incorreta!"
    end
end)

-- Botão Copiar Link
local btnCopiar = Instance.new("TextButton", frameKey)
btnCopiar.Size = UDim2.new(0, 260, 0, 40)
btnCopiar.Text = "Copiar o Link"
btnCopiar.BackgroundColor3 = Color3.fromRGB(80, 80, 255)
btnCopiar.TextColor3 = Color3.new(1, 1, 1)
btnCopiar.Font = Enum.Font.GothamBold
btnCopiar.TextSize = 16

btnCopiar.MouseButton1Click:Connect(function()
    setclipboard("https://linktr.ee/ryangamer7")
    btnCopiar.Text = "Link copiado!"
end)

-- Criar GUI principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "BlueLockGUI"
gui.ResetOnSpawn = false
gui.Enabled = false -- Esconde a GUI até a key ser validada

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 250, 0, 250)
frame.Position = UDim2.new(0.5, -125, 0.5, -125)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true

local uiList = Instance.new("UIListLayout", frame)
uiList.Padding = UDim.new(0, 6)
uiList.FillDirection = Enum.FillDirection.Vertical
uiList.HorizontalAlignment = Enum.HorizontalAlignment.Center
uiList.VerticalAlignment = Enum.VerticalAlignment.Top

-- Função utilitária para criar botões
local function criarBotao(texto, cor)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 200, 0, 40)
    btn.Text = texto
    btn.BackgroundColor3 = cor
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    return btn
end

-- Botão de correr mais rápido
local btnCorrer = criarBotao("Correr Mais Rápido", Color3.fromRGB(70, 130, 180))
btnCorrer.MouseButton1Click:Connect(function()
    humanoid.WalkSpeed = 100
end)
btnCorrer.Parent = frame

-- Botão de teleportar para a bola
local btnTPBola = criarBotao("Teleportar pra Bola", Color3.fromRGB(255, 140, 0))
btnTPBola.MouseButton1Click:Connect(function()
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj.Name == "Ball" and obj:IsA("BasePart") then
            hrp.CFrame = obj.CFrame + Vector3.new(0, 2, 0)
            break
        end
    end
end)
btnTPBola.Parent = frame

-- Campo de estilo
local estiloBox = Instance.new("TextBox", frame)
estiloBox.Size = UDim2.new(0, 200, 0, 40)
estiloBox.PlaceholderText = "Digite o estilo (ex: Kaiser)"
estiloBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
estiloBox.TextColor3 = Color3.new(1, 1, 1)
estiloBox.Font = Enum.Font.Gotham
estiloBox.TextSize = 16

estiloBox.FocusLost:Connect(function()
    local estilo = estiloBox.Text
    local remote = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Knit"):WaitForChild("Services"):WaitForChild("StatsService"):WaitForChild("RF"):WaitForChild("ChangeStyle")
    remote:InvokeServer(estilo)
    print("Estilo ativado:", estilo)
end)

-- Botão Auto Goal
local autoGoal = false
local btnAutoGoal = criarBotao("Auto Goal: OFF", Color3.fromRGB(255, 60, 60))

btnAutoGoal.MouseButton1Click:Connect(function()
    autoGoal = not autoGoal
    btnAutoGoal.Text = "Auto Goal: " .. (autoGoal and "ON" or "OFF")
    btnAutoGoal.BackgroundColor3 = autoGoal and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(255, 60, 60)

    while autoGoal do
        local bola
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj.Name == "Ball" and obj:IsA("BasePart") then
                bola = obj
                break
            end
        end
        if bola then
            hrp.CFrame = bola.CFrame + Vector3.new(0, 2, 0)
            task.wait(0.2)

            -- Ajustar a direção e força conforme o gol do seu time
            local args = {
                100, -- força
                [4] = Vector3.new(0, 0, 1) -- direção
            }

            ReplicatedStorage.Packages.Knit.Services.BallService.RE.Shoot:FireServer(unpack(args))
        end
        task.wait(3)
    end
end)
btnAutoGoal.Parent = frame
