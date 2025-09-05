--[[ GUI Invisível / Visível (com botão de abrir/fechar e draggable)
     Criado para KRNL / executores Roblox ]]--

-- Referências do jogador
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()

-- Criando GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "InvisGUI"

-- Botão de abrir/fechar o menu
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Parent = ScreenGui
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0.6, 0)
toggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleButton.Text = "Abrir Menu"
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.TextScaled = true

-- Janela principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Parent = ScreenGui
mainFrame.Size = UDim2.new(0, 250, 0, 150)
mainFrame.Position = UDim2.new(0.5, -125, 0.5, -75)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.Visible = false

-- Arredondamento
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

-- Draggable
mainFrame.Active = true
mainFrame.Draggable = true

-- Botão: Ficar Invisível
local invisButton = Instance.new("TextButton")
invisButton.Parent = mainFrame
invisButton.Size = UDim2.new(0.8, 0, 0.3, 0)
invisButton.Position = UDim2.new(0.1, 0, 0.15, 0)
invisButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
invisButton.Text = "Ficar Invisível"
invisButton.TextColor3 = Color3.new(1, 1, 1)
invisButton.TextScaled = true
Instance.new("UICorner", invisButton)

-- Botão: Ficar Visível
local visButton = Instance.new("TextButton")
visButton.Parent = mainFrame
visButton.Size = UDim2.new(0.8, 0, 0.3, 0)
visButton.Position = UDim2.new(0.1, 0, 0.55, 0)
visButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
visButton.Text = "Ficar Visível"
visButton.TextColor3 = Color3.new(1, 1, 1)
visButton.TextScaled = true
Instance.new("UICorner", visButton)

-- Invisibilidade (esconde partes)
local function tornarInvisivel()
	for _, part in pairs(char:GetDescendants()) do
		if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
			part.Transparency = 1
			if part:FindFirstChildOfClass("Decal") then
				for _, decal in pairs(part:GetDescendants()) do
					if decal:IsA("Decal") then
						decal.Transparency = 1
					end
				end
			end
		end
	end
end

-- Visibilidade
local function tornarVisivel()
	for _, part in pairs(char:GetDescendants()) do
		if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
			part.Transparency = 0
			if part:FindFirstChildOfClass("Decal") then
				for _, decal in pairs(part:GetDescendants()) do
					if decal:IsA("Decal") then
						decal.Transparency = 0
					end
				end
			end
		end
	end
end

-- Funções dos botões
invisButton.MouseButton1Click:Connect(function()
	tornarInvisivel()
end)

visButton.MouseButton1Click:Connect(function()
	tornarVisivel()
end)

-- Alternar visibilidade do menu
local aberto = false
toggleButton.MouseButton1Click:Connect(function()
	aberto = not aberto
	mainFrame.Visible = aberto
	toggleButton.Text = aberto and "Fechar Menu" or "Abrir Menu"
end)
