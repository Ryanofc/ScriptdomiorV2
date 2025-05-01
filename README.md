-- Função para gerar a Key do dia
local function gerarKey()
    local data = os.date("*t")
    return tostring(data.day) .. tostring(data.month) .. tostring(data.year)
end

local keyCorreta = gerarKey()

-- GUI de Key
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

-- Esconde GUI principal até passar pela Key
gui.Enabled = false
