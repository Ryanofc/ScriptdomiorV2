local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Referências da GUI
local gui = player.PlayerGui:WaitForChild("MainGUI") -- Nome da sua GUI
local correrButton = gui:WaitForChild("CorrerButton")
local teleportButton = gui:WaitForChild("TeleportButton")
local applyStyleButton = gui:WaitForChild("ApplyStyleButton")
local styleTextBox = gui:WaitForChild("StyleTextBox")

-- Função para correr mais rápido
local function correrRapido()
    -- Supondo que o personagem tenha um "Humanoid" com a propriedade "WalkSpeed"
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 100  -- Aumente a velocidade conforme necessário
    end
end

-- Função para se teleportar para a bola no Blue Lock Rivals
local function teleportarParaBola()
    -- Encontrar a bola no jogo
    local bola = workspace:FindFirstChild("Bola") -- Nome da bola no jogo
    if bola then
        player.Character:SetPrimaryPartCFrame(bola.CFrame)
    end
end

-- Função para aplicar o estilo
local function aplicarEstilo()
    local nomeEstilo = styleTextBox.Text
    local estilosValidos = {
        "Kaiser",
        "Sae",
        "Shidou",
        "Rin",
        "Bachira",
        "Nagi",
        "Kunigami",
        "Reo",
        "Karasu",
        "Otoya",
        "Gagamaru",
        "Don Lorenzo",
        "King",
        "Yukimiya",
        "Isagi",
        "NEL Isagi",
        "NEL Bachira"
    }

    -- Verificar se o nome digitado é um estilo válido
    for _, estilo in ipairs(estilosValidos) do
        if string.lower(nomeEstilo) == string.lower(estilo) then
            -- Aqui você deve chamar o RemoteEvent para aplicar o estilo no servidor
            -- Exemplo: game.ReplicatedStorage.Remotes.SetStyle:FireServer(estilo)
            print("Estilo aplicado: " .. estilo)
            return
        end
    end

    print("Estilo inválido!")
end

-- Eventos de clique nos botões
correrButton.MouseButton1Click:Connect(correrRapido)
teleportButton.MouseButton1Click:Connect(teleportarParaBola)
applyStyleButton.MouseButton1Click:Connect(aplicarEstilo)

-- Criar um botão para esconder/exibir a GUI
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 50)
toggleButton.Position = UDim2.new(0, 0, 1, -60)
toggleButton.Text = "Toggle GUI"
toggleButton.Parent = gui

local guiVisible = true
toggleButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    gui.Visible = guiVisible
end)

-- Inicialização
gui.Visible = false  -- Começar com a GUI invisível
