-- Kalleby Hub RGB Lento

local gui = Instance.new("ScreenGui")
gui.Name = "Kalleby Hub RGB Lento"
gui.Parent = game.CoreGui
gui.ZIndexBehavior = Enum.ZIndexBehavior.Global

local dragging = nil
local dragInput = nil
local dragStart = nil
local startPos = nil

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 200, 0, 150)
mainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.new(0, 0, 0) -- Preto
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = false
mainFrame.Parent = gui

-- Função para atualizar a cor RGB
local function updateRGB()
    local tick = os.time() % 60 -- Usa o tempo para criar um ciclo de cores
    local r = math.sin(tick * 0.1 + 0) * 0.5 + 0.5
    local g = math.sin(tick * 0.1 + 2) * 0.5 + 0.5
    local b = math.sin(tick * 0.1 + 4) * 0.5 + 0.5
    mainFrame.BackgroundColor3 = Color3.new(r, g, b)
end

game:GetService("RunService").RenderStepped:Connect(updateRGB)

-- Função para arrastar a interface
mainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragInput = input
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)

mainFrame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        if dragging and input == dragInput then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end
end)

mainFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
        dragInput = nil
    end
end)

-- Botões
local desyncButton = Instance.new("TextButton")
desyncButton.Size = UDim2.new(0.9, 0, 0.25, 0)
desyncButton.Position = UDim2.new(0.05, 0, 0.05, 0)
desyncButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
desyncButton.TextColor3 = Color3.new(1, 1, 1)
desyncButton.Text = "Desync"
desyncButton.Font = Enum.Font.SourceSansBold
desyncButton.Parent = mainFrame
desyncButton.MouseButton1Click:Connect(function()
    -- Lógica do Desync (Clone automático e ocultação)
    local character = game.Players.LocalPlayer.Character
    if character then
        local clone = character:Clone()
        clone.Parent = workspace
        clone:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame())
        -- Simular ocultação (mover para longe e criar ilusão)
        character:MoveTo(character:GetPrimaryPartCFrame().p + Vector3.new(1000, 0, 0)) -- Move o personagem para longe
        wait(0.5)
        clone:Destroy() -- Remove o clone após um curto período
    end
end)

local stealFlorButton = Instance.new("TextButton")
stealFlorButton.Size = UDim2.new(0.9, 0, 0.25, 0)
stealFlorButton.Position = UDim2.new(0.05, 0, 0.35, 0)
stealFlorButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
stealFlorButton.TextColor3 = Color3.new(1, 1, 1)
stealFlorButton.Text = "Steal Flor"
stealFlorButton.Font = Enum.Font.SourceSansBold
stealFlorButton.Parent = mainFrame
stealFlorButton.MouseButton1Click:Connect(function()
    -- Lógica do Steal Flor (Voar e usar Laser Cape)
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character:FindFirstChild("Humanoid")
        humanoid.JumpPower = 150
        humanoid.WalkSpeed = 50
        -- Simular voo (aumentar a gravidade para dar a ilusão de voo)
        game.Workspace.Gravity = 20
        -- Simular Laser Cape (atacar jogadores próximos)
        for i, v in pairs(game.Players:GetPlayers()) do
            if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local distance = (character:GetPrimaryPartCFrame().p - v.Character:GetPrimaryPartCFrame().p).Magnitude
                if distance < 15 then
                    -- Simular ataque (danificar o jogador próximo)
                    v.Character.Humanoid:TakeDamage(10)
                end
            end
        end
    end
end)

local infiniteJumpButton = Instance.new("TextButton")
infiniteJumpButton.Size = UDim2.new(0.9, 0, 0.25, 0)
infiniteJumpButton.Position = UDim2.new(0.05, 0, 0.65, 0)
infiniteJumpButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
infiniteJumpButton.TextColor3 = Color3.new(1, 1, 1)
infiniteJumpButton.Text = "Infinite Jump"
infiniteJumpButton.Font = Enum.Font.SourceSansBold
infiniteJumpButton.Parent = mainFrame
infiniteJumpButton.MouseButton1Click:Connect(function()
    -- Lógica do Infinite Jump
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character:FindFirstChild("Humanoid")
        humanoid.JumpPower = 150
        humanoid.UseJumpPower = true
        humanoid.MaxJumpHeight = 100

        local function onJumpRequest()
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end

        game:GetService("UserInputService").JumpRequest:Connect(onJumpRequest)
    end
end)

print("Kalleby Hub RGB Lento carregado!")

