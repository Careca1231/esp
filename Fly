-- Criando GUI
local ScreenGui = Instance.new("ScreenGui")
local FlyFrame = Instance.new("Frame")
local TitleBar = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")
local dragging, dragInput, dragStart, startPos

-- Propriedades da GUI
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "FlyGUI"

FlyFrame.Name = "FlyFrame"
FlyFrame.Parent = ScreenGui
FlyFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
FlyFrame.Position = UDim2.new(0.35, 0, 0.35, 0)
FlyFrame.Size = UDim2.new(0, 200, 0, 50)

UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = FlyFrame

TitleBar.Name = "TitleBar"
TitleBar.Parent = FlyFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
TitleBar.Size = UDim2.new(1, 0, 0.4, 0)
TitleBar.Text = "Clique para Voar"
TitleBar.TextColor3 = Color3.new(1, 1, 1)
TitleBar.TextScaled = true
TitleBar.BackgroundTransparency = 0.1

-- Função de arrastar
TitleBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = FlyFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

TitleBar.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then
		dragInput = input
	end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		local delta = input.Position - dragStart
		FlyFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

-- Script de voo
local flying = false
local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local bodyGyro = Instance.new("BodyGyro")
local bodyVelocity = Instance.new("BodyVelocity")

function startFlying()
	bodyGyro.P = 9e4
	bodyGyro.Parent = humanoidRootPart
	bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.CFrame = humanoidRootPart.CFrame

	bodyVelocity.Velocity = Vector3.new(0, 0, 0)
	bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
	bodyVelocity.Parent = humanoidRootPart

	while flying do
		task.wait()
		local moveDirection = Vector3.new()
		if UIS:IsKeyDown(Enum.KeyCode.W) then moveDirection = moveDirection + workspace.CurrentCamera.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.S) then moveDirection = moveDirection - workspace.CurrentCamera.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.A) then moveDirection = moveDirection - workspace.CurrentCamera.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.D) then moveDirection = moveDirection + workspace.CurrentCamera.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.Space) then moveDirection = moveDirection + Vector3.new(0,1,0) end
		if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then moveDirection = moveDirection - Vector3.new(0,1,0) end

		bodyVelocity.Velocity = moveDirection.Unit * 50
		bodyGyro.CFrame = workspace.CurrentCamera.CFrame
	end
end

function stopFlying()
	flying = false
	bodyGyro:Destroy()
	bodyVelocity:Destroy()
end

-- Ativação do voo ao clicar na barra
TitleBar.MouseButton1Click:Connect(function()
	flying = not flying
	if flying then
		TitleBar.Text = "Voando! (clique para parar)"
		startFlying()
	else
		TitleBar.Text = "Clique para Voar"
		stopFlying()
	end
end)
