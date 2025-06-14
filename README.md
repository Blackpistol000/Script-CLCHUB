# Script-CLCHUB
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- GUI Principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "CLCHubPanel"
gui.ResetOnSpawn = false

-- Tamanhos
local originalSize = UDim2.new(0, 340, 0, 300)
local minimizedSize = UDim2.new(0, 80, 0, 80)

-- Frame principal
local frame = Instance.new("Frame", gui)
frame.Size = originalSize
frame.Position = UDim2.new(0.5, -170, 0.5, -150)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", frame).Color = Color3.fromRGB(255, 0, 200)

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0.25, 0)
title.BackgroundTransparency = 1
title.Text = "CLC HUB | Os Hackers do Turvo"
title.TextColor3 = Color3.fromRGB(255, 0, 200)
title.TextStrokeTransparency = 0
title.TextScaled = true
title.Font = Enum.Font.GothamBlack

-- Subtítulo
local subtitle = Instance.new("TextLabel", frame)
subtitle.Size = UDim2.new(1, -20, 0.15, 0)
subtitle.Position = UDim2.new(0, 10, 0.25, 0)
subtitle.BackgroundTransparency = 1
subtitle.Text = "⚡ Somos os deuses do Brookhaven ⚡"
subtitle.TextColor3 = Color3.fromRGB(200, 0, 255)
subtitle.TextScaled = true
subtitle.Font = Enum.Font.FredokaOne
subtitle.TextStrokeTransparency = 0.5

-- Botão de minimizar
local minimize = Instance.new("TextButton", frame)
minimize.Size = UDim2.new(0, 30, 0, 30)
minimize.Position = UDim2.new(1, -35, 0, 5)
minimize.Text = "-"
minimize.BackgroundColor3 = Color3.fromRGB(255, 0, 200)
minimize.TextScaled = true
minimize.Font = Enum.Font.GothamBold
minimize.TextColor3 = Color3.new(1, 1, 1)

-- Botão minimizado com Goku Black
local minimizedButton = Instance.new("ImageButton", gui)
minimizedButton.Size = minimizedSize
minimizedButton.Position = frame.Position
minimizedButton.AnchorPoint = Vector2.new(0.5, 0.5)
minimizedButton.BackgroundColor3 = Color3.fromRGB(128, 0, 128)
minimizedButton.Image = "rbxassetid://7160114984"
minimizedButton.BorderSizePixel = 0
minimizedButton.Visible = false
minimizedButton.Active = true
minimizedButton.Draggable = true
minimizedButton.ZIndex = 10

-- Container de botões
local buttonsFrame = Instance.new("Frame", frame)
buttonsFrame.Size = UDim2.new(1, -20, 0.5, 0)
buttonsFrame.Position = UDim2.new(0, 10, 0.45, 0)
buttonsFrame.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", buttonsFrame)
layout.FillDirection = Enum.FillDirection.Vertical
layout.Padding = UDim.new(0, 5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Botão Noclip
local noclipEnabled = false
local noclipButton = Instance.new("TextButton", buttonsFrame)
noclipButton.Size = UDim2.new(1, 0, 0, 40)
noclipButton.Text = "Noclip: Desativado"
noclipButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
noclipButton.TextColor3 = Color3.new(1, 1, 1)
noclipButton.Font = Enum.Font.GothamBold
noclipButton.TextScaled = true
Instance.new("UICorner", noclipButton).CornerRadius = UDim.new(0, 5)

noclipButton.MouseButton1Click:Connect(function()
	noclipEnabled = not noclipEnabled
	noclipButton.Text = noclipEnabled and "Noclip: Ativado" or "Noclip: Desativado"
	noclipButton.BackgroundColor3 = noclipEnabled and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(50, 50, 50)

	if character and character:FindFirstChild("HumanoidRootPart") then
		character.HumanoidRootPart.CanCollide = not noclipEnabled
		humanoid.PlatformStand = noclipEnabled
	end
end)

-- Botão Teleporte para Casa
local teleportButton = Instance.new("TextButton", buttonsFrame)
teleportButton.Size = UDim2.new(1, 0, 0, 40)
teleportButton.Text = "Teleportar para Casa"
teleportButton.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
teleportButton.TextColor3 = Color3.new(1, 1, 1)
teleportButton.Font = Enum.Font.GothamBold
teleportButton.TextScaled = true
Instance.new("UICorner", teleportButton).CornerRadius = UDim.new(0, 5)

teleportButton.MouseButton1Click:Connect(function()
	if character and character:FindFirstChild("HumanoidRootPart") then
		for _, obj in pairs(workspace:GetChildren()) do
			if obj:IsA("Model") and string.find(string.lower(obj.Name), string.lower(player.Name)) and obj.PrimaryPart then
				character.HumanoidRootPart.CFrame = obj.PrimaryPart.CFrame + Vector3.new(0, 5, 0)
				return
			end
		end
		-- Localização padrão
		character.HumanoidRootPart.CFrame = CFrame.new(-325, 10, 200)
	end
end)

-- Minimizar e Restaurar com Tween
minimize.MouseButton1Click:Connect(function()
	local tween = TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quad), {Size = minimizedSize})
	tween:Play()
	tween.Completed:Connect(function()
		minimizedButton.Position = frame.Position
		frame.Visible = false
		minimizedButton.Visible = true
	end)
end)

minimizedButton.MouseButton1Click:Connect(function()
	frame.Size = minimizedSize
	frame.Position = minimizedButton.Position
	frame.Visible = true
	minimizedButton.Visible = false

	TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quad), {Size = originalSize}):Play()
end)

-- Som ao abrir (Kamehameha)
local sound = Instance.new("Sound")
sound.Name = "KamehamehaSound"
sound.SoundId = "rbxassetid://9118823100"
sound.Volume = 1
sound.Parent = SoundService

sound.Loaded:Connect(function()
	sound:Play()
end)

task.delay(1, function()
	if not sound.IsPlaying then sound:Play() end
end)

-- Corrige posição inicial do botão minimizado
task.wait()
minimizedButton.Position = frame.Position
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- GUI Principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "CLCHubPanel"
gui.ResetOnSpawn = false

-- Tamanhos
local originalSize = UDim2.new(0, 340, 0, 300)
local minimizedSize = UDim2.new(0, 80, 0, 80)

-- Frame principal
local frame = Instance.new("Frame", gui)
frame.Size = originalSize
frame.Position = UDim2.new(0.5, -170, 0.5, -150)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", frame).Color = Color3.fromRGB(255, 0, 200)

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0.25, 0)
title.BackgroundTransparency = 1
title.Text = "CLC HUB | Os Hackers do Turvo"
title.TextColor3 = Color3.fromRGB(255, 0, 200)
title.TextStrokeTransparency = 0
title.TextScaled = true
title.Font = Enum.Font.GothamBlack

-- Subtítulo
local subtitle = Instance.new("TextLabel", frame)
subtitle.Size = UDim2.new(1, -20, 0.15, 0)
subtitle.Position = UDim2.new(0, 10, 0.25, 0)
subtitle.BackgroundTransparency = 1
subtitle.Text = "⚡ Somos os deuses do Brookhaven ⚡"
subtitle.TextColor3 = Color3.fromRGB(200, 0, 255)
subtitle.TextScaled = true
subtitle.Font = Enum.Font.FredokaOne
subtitle.TextStrokeTransparency = 0.5

-- Botão de minimizar
local minimize = Instance.new("TextButton", frame)
minimize.Size = UDim2.new(0, 30, 0, 30)
minimize.Position = UDim2.new(1, -35, 0, 5)
minimize.Text = "-"
minimize.BackgroundColor3 = Color3.fromRGB(255, 0, 200)
minimize.TextScaled = true
minimize.Font = Enum.Font.GothamBold
minimize.TextColor3 = Color3.new(1, 1, 1)

-- Botão minimizado com Goku Black
local minimizedButton = Instance.new("ImageButton", gui)
minimizedButton.Size = minimizedSize
minimizedButton.Position = frame.Position
minimizedButton.AnchorPoint = Vector2.new(0.5, 0.5)
minimizedButton.BackgroundColor3 = Color3.fromRGB(128, 0, 128)
minimizedButton.Image = "rbxassetid://7160114984"
minimizedButton.BorderSizePixel = 0
minimizedButton.Visible = false
minimizedButton.Active = true
minimizedButton.Draggable = true
minimizedButton.ZIndex = 10

-- Container de botões
local buttonsFrame = Instance.new("Frame", frame)
buttonsFrame.Size = UDim2.new(1, -20, 0.5, 0)
buttonsFrame.Position = UDim2.new(0, 10, 0.45, 0)
buttonsFrame.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", buttonsFrame)
layout.FillDirection = Enum.FillDirection.Vertical
layout.Padding = UDim.new(0, 5)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Botão Noclip
local noclipEnabled = false
local noclipButton = Instance.new("TextButton", buttonsFrame)
noclipButton.Size = UDim2.new(1, 0, 0, 40)
noclipButton.Text = "Noclip: Desativado"
noclipButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
noclipButton.TextColor3 = Color3.new(1, 1, 1)
noclipButton.Font = Enum.Font.GothamBold
noclipButton.TextScaled = true
Instance.new("UICorner", noclipButton).CornerRadius = UDim.new(0, 5)

noclipButton.MouseButton1Click:Connect(function()
	noclipEnabled = not noclipEnabled
	noclipButton.Text = noclipEnabled and "Noclip: Ativado" or "Noclip: Desativado"
	noclipButton.BackgroundColor3 = noclipEnabled and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(50, 50, 50)

	if character and character:FindFirstChild("HumanoidRootPart") then
		character.HumanoidRootPart.CanCollide = not noclipEnabled
		humanoid.PlatformStand = noclipEnabled
	end
end)

-- Botão Teleporte para Casa
local teleportButton = Instance.new("TextButton", buttonsFrame)
teleportButton.Size = UDim2.new(1, 0, 0, 40)
teleportButton.Text = "Teleportar para Casa"
teleportButton.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
teleportButton.TextColor3 = Color3.new(1, 1, 1)
teleportButton.Font = Enum.Font.GothamBold
teleportButton.TextScaled = true
Instance.new("UICorner", teleportButton).CornerRadius = UDim.new(0, 5)

teleportButton.MouseButton1Click:Connect(function()
	if character and character:FindFirstChild("HumanoidRootPart") then
		for _, obj in pairs(workspace:GetChildren()) do
			if obj:IsA("Model") and string.find(string.lower(obj.Name), string.lower(player.Name)) and obj.PrimaryPart then
				character.HumanoidRootPart.CFrame = obj.PrimaryPart.CFrame + Vector3.new(0, 5, 0)
				return
			end
		end
		-- Localização padrão
		character.HumanoidRootPart.CFrame = CFrame.new(-325, 10, 200)
	end
end)

-- Minimizar e Restaurar com Tween
minimize.MouseButton1Click:Connect(function()
	local tween = TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quad), {Size = minimizedSize})
	tween:Play()
	tween.Completed:Connect(function()
		minimizedButton.Position = frame.Position
		frame.Visible = false
		minimizedButton.Visible = true
	end)
end)

minimizedButton.MouseButton1Click:Connect(function()
	frame.Size = minimizedSize
	frame.Position = minimizedButton.Position
	frame.Visible = true
	minimizedButton.Visible = false

	TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quad), {Size = originalSize}):Play()
end)

-- Som ao abrir (Kamehameha)
local sound = Instance.new("Sound")
sound.Name = "KamehamehaSound"
sound.SoundId = "rbxassetid://9118823100"
sound.Volume = 1
sound.Parent = SoundService

sound.Loaded:Connect(function()
	sound:Play()
end)

task.delay(1, function()
	if not sound.IsPlaying then sound:Play() end
end)

-- Corrige posição inicial do botão minimizado
task.wait()
minimizedButton.Position = frame.Position
