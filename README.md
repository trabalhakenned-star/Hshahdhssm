--========================================================--
-- ARENA FANTASY - K MOBILE TEST PANEL
-- MOBILE ONLY
-- LocalScript
-- StarterPlayer > StarterPlayerScripts
--========================================================--

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CollectionService = game:GetService("CollectionService")

local Player = Players.LocalPlayer

--========================================================--
-- MOBILE ONLY
--========================================================--

if not UserInputService.TouchEnabled then
	return
end

--========================================================--
-- CONFIG
--========================================================--

local CONFIG = {
	MaxSpeed = 100,
	SafeZoneHeight = 150,
	FragmentTag = "Fragment"
}

--========================================================--
-- STATE
--========================================================--

local State = {
	InfiniteJump = false,
	Noclip = false,
	SafeZone = false,
	CollectFragments = false,
	Speed = false,

	WalkSpeed = 16
}

local Character
local Humanoid
local RootPart

local SafeConnection
local SafeZonePosition

local NoclipConnection
local CollectConnection

--========================================================--
-- CHARACTER
--========================================================--

local function SetupCharacter(CharacterModel)

	Character = CharacterModel

	Humanoid = Character:WaitForChild("Humanoid")
	RootPart = Character:WaitForChild("HumanoidRootPart")

	if State.Speed then
		Humanoid.WalkSpeed = State.WalkSpeed
	else
		Humanoid.WalkSpeed = 16
	end

end

if Player.Character then
	SetupCharacter(Player.Character)
end

Player.CharacterAdded:Connect(function(CharacterModel)

	SetupCharacter(CharacterModel)

end)

--========================================================--
-- GUI
--========================================================--

local Gui = Instance.new("ScreenGui")
Gui.Name = "ArenaFantasyKMobile"
Gui.ResetOnSpawn = false
Gui.IgnoreGuiInset = true
Gui.Parent = Player:WaitForChild("PlayerGui")

--========================================================--
-- K BUTTON
--========================================================--

local KButton = Instance.new("TextButton")

KButton.Name = "KButton"

KButton.Size = UDim2.fromOffset(60, 60)

KButton.Position =
	UDim2.new(0, 18, 0.5, -30)

KButton.BackgroundColor3 =
	Color3.fromRGB(85, 30, 135)

KButton.Text = "K"

KButton.TextColor3 =
	Color3.fromRGB(255, 255, 255)

KButton.TextSize = 30

KButton.Font =
	Enum.Font.GothamBold

KButton.Parent = Gui

local KCorner = Instance.new("UICorner")

KCorner.CornerRadius =
	UDim.new(1, 0)

KCorner.Parent = KButton

local KStroke = Instance.new("UIStroke")

KStroke.Color =
	Color3.fromRGB(190, 100, 255)

KStroke.Thickness = 2

KStroke.Parent = KButton

--========================================================--
-- PAINEL
--========================================================--

local Panel = Instance.new("Frame")

Panel.Name = "MainPanel"

Panel.Size =
	UDim2.fromOffset(330, 470)

Panel.Position =
	UDim2.new(0.5, -165, 0.5, -235)

Panel.BackgroundColor3 =
	Color3.fromRGB(15, 10, 20)

Panel.BorderSizePixel = 0

Panel.Visible = false

Panel.Parent = Gui

local PanelCorner = Instance.new("UICorner")

PanelCorner.CornerRadius =
	UDim.new(0, 14)

PanelCorner.Parent = Panel

local PanelStroke = Instance.new("UIStroke")

PanelStroke.Color =
	Color3.fromRGB(130, 50, 190)

PanelStroke.Thickness = 2

PanelStroke.Parent = Panel

--========================================================--
-- TÍTULO
--========================================================--

local Title = Instance.new("TextLabel")

Title.Size =
	UDim2.new(1, -20, 0, 40)

Title.Position =
	UDim2.fromOffset(10, 5)

Title.BackgroundTransparency = 1

Title.Text = "ARENA FANTASY"

Title.TextColor3 =
	Color3.fromRGB(255, 255, 255)

Title.TextSize = 22

Title.Font =
	Enum.Font.GothamBold

Title.Parent = Panel

local Subtitle = Instance.new("TextLabel")

Subtitle.Size =
	UDim2.new(1, -20, 0, 20)

Subtitle.Position =
	UDim2.fromOffset(10, 42)

Subtitle.BackgroundTransparency = 1

Subtitle.Text =
	"K • MOBILE DEVELOPER TEST"

Subtitle.TextColor3 =
	Color3.fromRGB(175, 120, 220)

Subtitle.TextSize = 11

Subtitle.Font =
	Enum.Font.Gotham

Subtitle.Parent = Panel

--========================================================--
-- SCROLL
--========================================================--

local Scroll = Instance.new("ScrollingFrame")

Scroll.Size =
	UDim2.new(1, -20, 1, -70)

Scroll.Position =
	UDim2.fromOffset(10, 68)

Scroll.BackgroundTransparency = 1

Scroll.BorderSizePixel = 0

Scroll.ScrollBarThickness = 4

Scroll.ScrollBarImageColor3 =
	Color3.fromRGB(140, 50, 210)

Scroll.CanvasSize =
	UDim2.fromOffset(0, 0)

Scroll.Parent = Panel

local Layout = Instance.new("UIListLayout")

Layout.Padding =
	UDim.new(0, 8)

Layout.SortOrder =
	Enum.SortOrder.LayoutOrder

Layout.Parent = Scroll

Layout:GetPropertyChangedSignal(
	"AbsoluteContentSize"
):Connect(function()

	Scroll.CanvasSize =
		UDim2.fromOffset(
			0,
			Layout.AbsoluteContentSize.Y + 10
		)

end)

--========================================================--
-- OPÇÃO ON/OFF
--========================================================--

local function CreateOption(Name, Callback)

	local Container =
		Instance.new("Frame")

	Container.Size =
		UDim2.new(1, -5, 0, 52)

	Container.BackgroundColor3 =
		Color3.fromRGB(27, 20, 34)

	Container.BorderSizePixel = 0

	Container.Parent = Scroll

	local Corner =
		Instance.new("UICorner")

	Corner.CornerRadius =
		UDim.new(0, 9)

	Corner.Parent = Container

	local Stroke =
		Instance.new("UIStroke")

	Stroke.Color =
		Color3.fromRGB(65, 40, 80)

	Stroke.Thickness = 1

	Stroke.Parent = Container

	local Label =
		Instance.new("TextLabel")

	Label.Size =
		UDim2.new(1, -90, 1, 0)

	Label.Position =
		UDim2.fromOffset(12, 0)

	Label.BackgroundTransparency = 1

	Label.Text = Name

	Label.TextXAlignment =
		Enum.TextXAlignment.Left

	Label.TextColor3 =
		Color3.fromRGB(255, 255, 255)

	Label.TextSize = 15

	Label.Font =
		Enum.Font.GothamMedium

	Label.Parent = Container

	local Toggle =
		Instance.new("TextButton")

	Toggle.Size =
		UDim2.fromOffset(62, 30)

	Toggle.Position =
		UDim2.new(1, -72, 0.5, -15)

	Toggle.BackgroundColor3 =
		Color3.fromRGB(55, 45, 60)

	Toggle.Text = "OFF"

	Toggle.TextColor3 =
		Color3.fromRGB(210, 210, 210)

	Toggle.TextSize = 12

	Toggle.Font =
		Enum.Font.GothamBold

	Toggle.Parent = Container

	local ToggleCorner =
		Instance.new("UICorner")

	ToggleCorner.CornerRadius =
		UDim.new(0, 8)

	ToggleCorner.Parent = Toggle

	local Enabled = false

	Toggle.Activated:Connect(function()

		Enabled = not Enabled

		if Enabled then

			Toggle.Text = "ON"

			Toggle.BackgroundColor3 =
				Color3.fromRGB(120, 45, 190)

			Toggle.TextColor3 =
				Color3.fromRGB(255, 255, 255)

		else

			Toggle.Text = "OFF"

			Toggle.BackgroundColor3 =
				Color3.fromRGB(55, 45, 60)

			Toggle.TextColor3 =
				Color3.fromRGB(210, 210, 210)

		end

		Callback(Enabled)

	end)

end

--========================================================--
-- SLIDER SPEED
--========================================================--

local function CreateSlider(
	TitleText,
	MinValue,
	MaxValue,
	DefaultValue,
	Callback
)

	local Container =
		Instance.new("Frame")

	Container.Size =
		UDim2.new(1, -5, 0, 70)

	Container.BackgroundColor3 =
		Color3.fromRGB(27, 20, 34)

	Container.BorderSizePixel = 0

	Container.Parent = Scroll

	local Corner =
		Instance.new("UICorner")

	Corner.CornerRadius =
		UDim.new(0, 9)

	Corner.Parent = Container

	local Label =
		Instance.new("TextLabel")

	Label.Size =
		UDim2.new(1, -25, 0, 25)

	Label.Position =
		UDim2.fromOffset(12, 5)

	Label.BackgroundTransparency = 1

	Label.Text =
		TitleText .. ": " .. DefaultValue

	Label.TextXAlignment =
		Enum.TextXAlignment.Left

	Label.TextColor3 =
		Color3.fromRGB(255, 255, 255)

	Label.TextSize = 14

	Label.Font =
		Enum.Font.GothamMedium

	Label.Parent = Container

	local Bar =
		Instance.new("Frame")

	Bar.Size =
		UDim2.new(1, -30, 0, 8)

	Bar.Position =
		UDim2.fromOffset(15, 43)

	Bar.BackgroundColor3 =
		Color3.fromRGB(55, 45, 60)

	Bar.BorderSizePixel = 0

	Bar.Parent = Container

	local BarCorner =
		Instance.new("UICorner")

	BarCorner.CornerRadius =
		UDim.new(1, 0)

	BarCorner.Parent = Bar

	local Fill =
		Instance.new("Frame")

	local Percent =
		(DefaultValue - MinValue) /
		(MaxValue - MinValue)

	Fill.Size =
		UDim2.new(Percent, 0, 1, 0)

	Fill.BackgroundColor3 =
		Color3.fromRGB(140, 50, 210)

	Fill.BorderSizePixel = 0

	Fill.Parent = Bar

	local FillCorner =
		Instance.new("UICorner")

	FillCorner.CornerRadius =
		UDim.new(1, 0)

	FillCorner.Parent = Fill

	local Dragging = false

	local function SetValue(X)

		local NewPercent =
			math.clamp(
				(X - Bar.AbsolutePosition.X) /
				Bar.AbsoluteSize.X,
				0,
				1
			)

		local Value =
			math.floor(
				MinValue +
				((MaxValue - MinValue) *
				NewPercent)
				+ 0.5
			)

		Fill.Size =
			UDim2.new(
				NewPercent,
				0,
				1,
				0
			)

		Label.Text =
			TitleText .. ": " .. Value

		Callback(Value)

	end

	Bar.InputBegan:Connect(function(Input)

		if Input.UserInputType ==
			Enum.UserInputType.Touch then

			Dragging = true

			SetValue(
				Input.Position.X
			)

		end

	end)

	UserInputService.InputChanged:Connect(function(Input)

		if not Dragging then
			return
		end

		if Input.UserInputType ==
			Enum.UserInputType.Touch then

			SetValue(
				Input.Position.X
			)

		end

	end)

	UserInputService.InputEnded:Connect(function(Input)

		if Input.UserInputType ==
			Enum.UserInputType.Touch then

			Dragging = false

		end

	end)

end

--========================================================--
-- INFINITE JUMP
--========================================================--

UserInputService.JumpRequest:Connect(function()

	if State.InfiniteJump
		and Humanoid then

		Humanoid:ChangeState(
			Enum.HumanoidStateType.Jumping
		)

	end

end)

--========================================================--
-- NOCLIP
--========================================================--

local function SetNoclip(Enabled)

	State.Noclip = Enabled

	if NoclipConnection then

		NoclipConnection:Disconnect()

		NoclipConnection = nil

	end

	if not Enabled then

		if Character then

			for _, Part in ipairs(
				Character:GetDescendants()
			) do

				if Part:IsA("BasePart") then

					Part.CanCollide = true

				end

			end

		end

		return
	end

	--====================================================--
	-- NOCLIP ATIVO
	--====================================================--

	NoclipConnection =
		RunService.Stepped:Connect(function()

			if not Character then
				return
			end

			for _, Part in ipairs(
				Character:GetDescendants()
			) do

				if Part:IsA("BasePart") then

					-- Desativa a colisão do personagem
					-- permitindo atravessar paredes.
					Part.CanCollide = false

				end

			end

		end)

end

--========================================================--
-- SAFE ZONE
--========================================================--

local function SetSafeZone(Enabled)

	State.SafeZone = Enabled

	if SafeConnection then

		SafeConnection:Disconnect()

		SafeConnection = nil

	end

	if not Enabled then

		SafeZonePosition = nil

		return
	end

	if not RootPart then
		return
	end

	SafeZonePosition =
		RootPart.Position +
		Vector3.new(
			0,
			CONFIG.SafeZoneHeight,
			0
		)

	RootPart.CFrame =
		CFrame.new(
			SafeZonePosition
		)

	RootPart.AssemblyLinearVelocity =
		Vector3.zero

	RootPart.AssemblyAngularVelocity =
		Vector3.zero

	SafeConnection =
		RunService.Heartbeat:Connect(function()

			if not State.SafeZone then
				return
			end

			if not RootPart then
				return
			end

			RootPart.CFrame =
				CFrame.new(
					SafeZonePosition
				)

			RootPart.AssemblyLinearVelocity =
				Vector3.zero

			RootPart.AssemblyAngularVelocity =
				Vector3.zero

		end)

end

--========================================================--
-- SPEED
--========================================================--

local function SetSpeed(Enabled)

	State.Speed = Enabled

	if not Humanoid then
		return
	end

	if Enabled then

		Humanoid.WalkSpeed =
			State.WalkSpeed

	else

		Humanoid.WalkSpeed =
			16

	end

end

--========================================================--
-- COLLECT FRAGMENTS
--========================================================--

local function SetCollectFragments(Enabled)

	State.CollectFragments =
		Enabled

	if CollectConnection then

		CollectConnection:Disconnect()

		CollectConnection = nil

	end

	if not Enabled then
		return
	end

	CollectConnection =
		RunService.Heartbeat:Connect(function()

			if not RootPart then
				return
			end

			for _, Fragment in ipairs(
				CollectionService:GetTagged(
					CONFIG.FragmentTag
				)
			) do

				if Fragment:IsA("BasePart") then

					local Distance =
						(
							Fragment.Position -
							RootPart.Position
						).Magnitude

					if Distance <= 10 then

						local Event =
							Fragment:FindFirstChild(
								"Collect"
							)

						if Event
							and Event:IsA(
								"RemoteEvent"
							) then

							Event:FireServer(
								Fragment
							)

						end

					end

				end

			end

		end)

end

--========================================================--
-- CONTROLES DO PAINEL
--========================================================--

CreateOption(
	"Infinite Jump",
	function(Enabled)

		State.InfiniteJump =
			Enabled

	end
)

CreateOption(
	"Noclip",
	SetNoclip
)

CreateOption(
	"Safe Zone",
	SetSafeZone
)

CreateOption(
	"Collect Fragments",
	SetCollectFragments
)

CreateOption(
	"Speed",
	SetSpeed
)

CreateSlider(
	"Speed",
	1,
	CONFIG.MaxSpeed,
	State.WalkSpeed,

	function(Value)

		State.WalkSpeed =
			Value

		if State.Speed
			and Humanoid then

			Humanoid.WalkSpeed =
				Value

		end

	end
)

--========================================================--
-- ARRASTAR BOTÃO K
--========================================================--

local DraggingButton = false
local DragStart
local StartPosition
local WasDragging = false

KButton.InputBegan:Connect(function(Input)

	if Input.UserInputType ==
		Enum.UserInputType.Touch then

		DraggingButton = true

		WasDragging = false

		DragStart =
			Input.Position

		StartPosition =
			KButton.Position

	end

end)

UserInputService.InputChanged:Connect(function(Input)

	if not DraggingButton then
		return
	end

	if Input.UserInputType ~=
		Enum.UserInputType.Touch then

		return
	end

	local Delta =
		Input.Position -
		DragStart

	if Delta.Magnitude > 8 then

		WasDragging = true

	end

	KButton.Position =
		UDim2.new(
			StartPosition.X.Scale,
			StartPosition.X.Offset + Delta.X,

			StartPosition.Y.Scale,
			StartPosition.Y.Offset + Delta.Y
		)

end)

UserInputService.InputEnded:Connect(function(Input)

	if Input.UserInputType ==
		Enum.UserInputType.Touch then

		DraggingButton = false

	end

end)

--========================================================--
-- ABRIR / FECHAR PAINEL
--========================================================--

KButton.Activated:Connect(function()

	if not WasDragging then

		Panel.Visible =
			not Panel.Visible

	end

	WasDragging = false

end)

print("================================")
print("ARENA FANTASY")
print("K MOBILE TEST PANEL")
print("Fly removido.")
print("Speed restaurado.")
print("Noclip corrigido.")
print("================================")
