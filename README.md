-- Gui Principal criada por script (MainGui)
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "MainGui"
ScreenGui.Enabled = false

local frame = Instance.new("Frame", ScreenGui)
frame.Size = UDim2.new(0, 200, 0, 180)
frame.Position = UDim2.new(0, 10, 0, 150)
frame.BackgroundColor3 = Color3.new(0, 0, 0.2)

local title = Instance.new("TextLabel", frame)
title.Text = "O Mior"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.new(0.1, 0.1, 0.4)
title.TextColor3 = Color3.new(1, 1, 1)

local correrBtn = Instance.new("TextButton", frame)
correrBtn.Text = "Correr + Rápido"
correrBtn.Size = UDim2.new(1, -20, 0, 30)
correrBtn.Position = UDim2.new(0, 10, 0, 40)
correrBtn.BackgroundColor3 = Color3.new(0, 0.5, 1)
correrBtn.TextColor3 = Color3.new(1, 1, 1)

local tpBtn = Instance.new("TextButton", frame)
tpBtn.Text = "TP pra Bola"
tpBtn.Size = UDim2.new(1, -20, 0, 30)
tpBtn.Position = UDim2.new(0, 10, 0, 80)
tpBtn.BackgroundColor3 = Color3.new(0, 0.5, 1)
tpBtn.TextColor3 = Color3.new(1, 1, 1)

local estiloBox = Instance.new("TextBox", frame)
estiloBox.PlaceholderText = "Nome do estilo"
estiloBox.Size = UDim2.new(1, -20, 0, 30)
estiloBox.Position = UDim2.new(0, 10, 0, 120)
estiloBox.BackgroundColor3 = Color3.new(1, 1, 1)
estiloBox.TextColor3 = Color3.new(0, 0, 0)

-- Toggle de visibilidade
local toggleBtn = Instance.new("TextButton", ScreenGui)
toggleBtn.Text = "Mostrar/Esconder GUI"
toggleBtn.Size = UDim2.new(0, 160, 0, 30)
toggleBtn.Position = UDim2.new(0, 10, 0, 340)
toggleBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
toggleBtn.TextColor3 = Color3.new(1, 1, 1)

toggleBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

-- Key GUI
local keyGui = Instance.new("ScreenGui", game.CoreGui)
local keyFrame = Instance.new("Frame", keyGui)
keyFrame.Size = UDim2.new(0, 240, 0, 180)
keyFrame.Position = UDim2.new(0.5, -120, 0.5, -90)
keyFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)

local keyBox = Instance.new("TextBox", keyFrame)
keyBox.PlaceholderText = "Insira a Key"
keyBox.Size = UDim2.new(0.8, 0, 0, 30)
keyBox.Position = UDim2.new(0.1, 0, 0.1, 0)

local aviso = Instance.new("TextLabel", keyFrame)
aviso.Text = ""
aviso.Size = UDim2.new(1, -20, 0, 20)
aviso.Position = UDim2.new(0, 10, 0.35, 0)
aviso.TextColor3 = Color3.new(1, 1, 1)
aviso.BackgroundTransparency = 1

local copyButton = Instance.new("TextButton", keyFrame)
copyButton.Text = "Copiar o link"
copyButton.Size = UDim2.new(0.8, 0, 0, 30)
copyButton.Position = UDim2.new(0.1, 0, 0.5, 0)

copyButton.MouseButton1Click:Connect(function()
    setclipboard("https://linktr.ee/ryangamer7")
    aviso.Text = "Link copiado!"
end)

local confirmButton = Instance.new("TextButton", keyFrame)
confirmButton.Text = "Confirmar"
confirmButton.Size = UDim2.new(0.8, 0, 0, 30)
confirmButton.Position = UDim2.new(0.1, 0, 0.7, 0)

-- Verificação automática de Key diária
confirmButton.MouseButton1Click:Connect(function()
    local dataHoje = os.date("%d%m%Y")
    if keyBox.Text == dataHoje then
        keyGui:Destroy()
        ScreenGui.Enabled = true
    else
        aviso.Text = "Key incorreta! Pegue no link."
    end
end)

-- Botão de correr (velocidade global)
correrBtn.MouseButton1Click:Connect(function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        char.Humanoid.WalkSpeed = 100
    end
end)

-- Botão de teleportar pra bola
tpBtn.MouseButton1Click:Connect(function()
    local bola = workspace:FindFirstChild("Football") or workspace:FindFirstChildWhichIsA("Part", true, function(part)
        return part.Name:lower():find("ball")
    end)
    if bola then
        local char = game.Players.LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = bola.CFrame + Vector3.new(0, 3, 0)
        end
    end
end)

-- Campo de estilo (simula estilo do jogador)
estiloBox.FocusLost:Connect(function()
    local estilo = estiloBox.Text:lower()
    if estilo ~= "" then
        print("Estilo ativado:", estilo)
        -- Aqui você pode adicionar lógica para cada estilo específico se quiser
    end
end)

-- Adicionando o código da função estiloBox.FocusLost:Connect(...) no final
estiloBox.FocusLost:Connect(function()
    local estilo = estiloBox.Text:lower()
    if estilo == "kaiser" then
        print("Estilo Kaiser ativado!")
        -- Aqui, adicione a lógica para o estilo Kaiser
    elseif estilo == "rifa" then
        print("Estilo Rifa ativado!")
        -- Adicione a lógica para o estilo Rifa
    elseif estilo == "rei" then
        print("Estilo Rei ativado!")
        -- Lógica para o estilo Rei
    elseif estilo == "reigen" then
        print("Estilo Reigen ativado!")
        -- Lógica para o estilo Reigen
    else
        print("Estilo não encontrado!")
    end
end)
