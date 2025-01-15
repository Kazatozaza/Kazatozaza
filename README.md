-- UI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MonsterFarmingUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 500)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -250)
mainFrame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
mainFrame.Visible = false  -- Initially hide the UI
mainFrame.Parent = ScreenGui

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
titleLabel.Text = "Monster Farming"
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 24
titleLabel.Parent = mainFrame

local monsterDropdown = Instance.new("TextButton")
monsterDropdown.Size = UDim2.new(1, -20, 0, 40)
monsterDropdown.Position = UDim2.new(0, 10, 0, 60)
monsterDropdown.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
monsterDropdown.Text = "Select Monster"
monsterDropdown.TextColor3 = Color3.new(1, 1, 1)
monsterDropdown.Font = Enum.Font.SourceSansBold
monsterDropdown.TextSize = 18
monsterDropdown.Parent = mainFrame

local combatDropdown = Instance.new("TextButton")
combatDropdown.Size = UDim2.new(1, -20, 0, 40)
combatDropdown.Position = UDim2.new(0, 10, 0, 110)
combatDropdown.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
combatDropdown.Text = "Select Combat"
combatDropdown.TextColor3 = Color3.new(1, 1, 1)
combatDropdown.Font = Enum.Font.SourceSansBold
combatDropdown.TextSize = 18
combatDropdown.Parent = mainFrame

local startButton = Instance.new("TextButton")
startButton.Size = UDim2.new(1, -20, 0, 40)
startButton.Position = UDim2.new(0, 10, 0, 160)
startButton.BackgroundColor3 = Color3.new(0.2, 0.8, 0.2)
startButton.Text = "Start Farming"
startButton.TextColor3 = Color3.new(1, 1, 1)
startButton.Font = Enum.Font.SourceSansBold
startButton.TextSize = 18
startButton.Parent = mainFrame

local stopButton = Instance.new("TextButton")
stopButton.Size = UDim2.new(1, -20, 0, 40)
stopButton.Position = UDim2.new(0, 10, 0, 210)
stopButton.BackgroundColor3 = Color3.new(0.8, 0.2, 0.2)
stopButton.Text = "Stop Farming"
stopButton.TextColor3 = Color3.new(1, 1, 1)
stopButton.Font = Enum.Font.SourceSansBold
stopButton.TextSize = 18
stopButton.Parent = mainFrame

local logLabel = Instance.new("TextLabel")
logLabel.Size = UDim2.new(1, -20, 0, 100)
logLabel.Position = UDim2.new(0, 10, 0, 260)
logLabel.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
logLabel.Text = "Status: Idle"
logLabel.TextColor3 = Color3.new(1, 1, 1)
logLabel.TextWrapped = true
logLabel.Font = Enum.Font.SourceSans
logLabel.TextSize = 16
logLabel.Parent = mainFrame

-- Variables
local autoFarming = false
local selectedMonster = nil
local selectedCombat = nil
local monsters = {
    "Bandit [Lv.1]", 
    "Asta [Lv.450]", 
    "Bandit Boss [Lv.25]", 
    "Do [Lv.225]", 
    "Kraken [Lv.100]", 
    "Kung [Lv.700]", 
    "Savage [Lv.150]", 
    "Shadow [Lv.450]", 
    "Shadow God [Lv.500]",
    "Wade [Lv.400]"  -- Added new monster
}

-- Dropdown for monsters
monsterDropdown.MouseButton1Click:Connect(function()
    local dropdownFrame = Instance.new("Frame")
    dropdownFrame.Size = UDim2.new(1, 0, 1, 0)
    dropdownFrame.Position = UDim2.new(0, 0, 0, 0)
    dropdownFrame.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
    dropdownFrame.Parent = mainFrame

    for i, monster in pairs(monsters) do
        local monsterButton = Instance.new("TextButton")
        monsterButton.Size = UDim2.new(1, -20, 0, 30)
        monsterButton.Position = UDim2.new(0, 10, 0, (i - 1) * 35)
        monsterButton.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
        monsterButton.Text = monster
        monsterButton.TextColor3 = Color3.new(1, 1, 1)
        monsterButton.Font = Enum.Font.SourceSans
        monsterButton.TextSize = 18
        monsterButton.Parent = dropdownFrame

        monsterButton.MouseButton1Click:Connect(function()
            selectedMonster = monster
            monsterDropdown.Text = "Selected: " .. monster
            dropdownFrame:Destroy()
        end)
    end
end)

-- Dropdown for combat
combatDropdown.MouseButton1Click:Connect(function()
    local dropdownFrame = Instance.new("Frame")
    dropdownFrame.Size = UDim2.new(1, 0, 1, 0)
    dropdownFrame.Position = UDim2.new(0, 0, 0, 0)
    dropdownFrame.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
    dropdownFrame.Parent = mainFrame

    local tools = game.Players.LocalPlayer.Backpack:GetChildren()
    for i, tool in pairs(tools) do
        if tool:IsA("Tool") then
            local toolButton = Instance.new("TextButton")
            toolButton.Size = UDim2.new(1, -20, 0, 30)
            toolButton.Position = UDim2.new(0, 10, 0, (i - 1) * 35)
            toolButton.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
            toolButton.Text = tool.Name
            toolButton.TextColor3 = Color3.new(1, 1, 1)
            toolButton.Font = Enum.Font.SourceSans
            toolButton.TextSize = 18
            toolButton.Parent = dropdownFrame

            toolButton.MouseButton1Click:Connect(function()
                selectedCombat = tool.Name
                combatDropdown.Text = "Selected: " .. tool.Name
                dropdownFrame:Destroy()
            end)
        end
    end
end)

-- Move to monster and face it for proper attack
function moveToMonster(monsterName)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")

    -- Scan for the monster in all parts of the workspace
    for _, obj in pairs(game:GetDescendants()) do
        if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj.Name == monsterName then
            local monsterHRP = obj:FindFirstChild("HumanoidRootPart")
            if monsterHRP then
                -- Move the player behind the monster and face it
                humanoidRootPart.CFrame = monsterHRP.CFrame + Vector3.new(0, 5, -5)  -- Behind the monster
                humanoidRootPart.CFrame = CFrame.lookAt(humanoidRootPart.Position, monsterHRP.Position) -- Facing the monster
                return monsterHRP
            end
        end
    end
    return nil
end

-- Attack function
function attackMonster(monsterName)
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local tool = player.Backpack:FindFirstChild(selectedCombat) or character:FindFirstChild(selectedCombat)

    if not tool then
        logLabel.Text = "Combat tool not found!"
        return
    end

    tool.Parent = character

    -- Move to the monster and face it
    local monsterHRP = moveToMonster(monsterName)
    if not monsterHRP then
        logLabel.Text = "Monster not found!"
        return
    end

    -- Activate the tool to attack the monster
    wait(0.2)  -- Wait for a moment before attacking
    tool:Activate()
end

-- Start Farming
startButton.MouseButton1Click:Connect(function()
    if not selectedMonster then
        logLabel.Text = "Please select a monster!"
        return
    end

    if not selectedCombat then
        logLabel.Text = "Please select a combat method!"
        return
    end

    autoFarming = true
    logLabel.Text = "Farming started on: " .. selectedMonster

    -- Farming loop
    while autoFarming do
        -- Attack and move towards the monster continuously
        attackMonster(selectedMonster)
        wait(0.5)  -- Delay to avoid too frequent updates
    end
end)

-- Stop Farming
stopButton.MouseButton1Click:Connect(function()
    autoFarming = false
    logLabel.Text = "Farming stopped"
end)

-- Toggle UI Visibility with a button
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.BackgroundColor3 = Color3.new(0.5, 0.5, 0.5)
toggleButton.Text = "Toggle UI"
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 18
toggleButton.Parent = ScreenGui

toggleButton.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
end)
