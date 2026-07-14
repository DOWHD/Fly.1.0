-- Удаляем старое меню
if game.CoreGui:FindFirstChild("MobileControlUI") then
    game.CoreGui.MobileControlUI:Destroy()
end

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local Camera = workspace.CurrentCamera

local Flying = false
local Noclip = false
local GodMode = false
local MovingForward = false
local Speed = 50
local VerticalSpeed = 0

-- UI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "MobileControlUI"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 200, 0, 480)
MainFrame.Position = UDim2.new(0.02, 0, 0.1, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)

-- Функции создания кнопок
local function AddBtn(text, pos, color, callback)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(0, 180, 0, 40)
    btn.Position = pos
    btn.Text = text
    btn.BackgroundColor3 = color or Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.Gotham
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 5)
    
    local active = false
    btn.MouseButton1Click:Connect(function()
        active = not active
        btn.BackgroundColor3 = active and Color3.fromRGB(0, 150, 0) or (color or Color3.fromRGB(50, 50, 50))
        callback(active)
    end)
end

-- Логика полёта
local bv = Instance.new("BodyVelocity")
bv.MaxForce = Vector3.new(1/0, 1/0, 1/0)
local bg = Instance.new("BodyGyro")
bg.MaxTorque = Vector3.new(1/0, 1/0, 1/0)
bg.P = 10000

RunService.RenderStepped:Connect(function()
    if Flying and Character and RootPart then
        Humanoid.PlatformStand = true
        bv.Parent = RootPart
        bg.Parent = RootPart
        local forward = MovingForward and Camera.CFrame.LookVector * Speed or Vector3.new(0,0,0)
        bv.Velocity = forward + Vector3.new(0, VerticalSpeed, 0)
        bg.CFrame = Camera.CFrame
    else
        bv.Parent = nil
        bg.Parent = nil
        if Humanoid then Humanoid.PlatformStand = false end
        VerticalSpeed = 0
    end
    if Humanoid then Humanoid.WalkSpeed = Speed end
end)

-- Noclip & GodMode
RunService.Stepped:Connect(function()
    if Noclip and Character then
        for _, p in pairs(Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide = false end end
    end
    if GodMode and Humanoid then Humanoid.Health = Humanoid.MaxHealth end
end)

-- ИНТЕРФЕЙС
AddBtn("Полёт", UDim2.new(0, 10, 0, 10), nil, function(v) Flying = v end)
AddBtn("Сквозь стены", UDim2.new(0, 10, 0, 60), nil, function(v) Noclip = v end)
AddBtn("Гуд Мод", UDim2.new(0, 10, 0, 110), nil, function(v) GodMode = v end)

-- Кнопка ВПЕРЁД
local btnForward = Instance.new("TextButton", MainFrame)
btnForward.Size = UDim2.new(0, 180, 0, 40)
btnForward.Position = UDim2.new(0, 10, 0, 160)
btnForward.Text = "ВПЕРЁД (Зажать)"
btnForward.BackgroundColor3 = Color3.fromRGB(100, 0, 100)
btnForward.MouseButton1Down:Connect(function() MovingForward = true end)
btnForward.MouseButton1Up:Connect(function() MovingForward = false end)

-- Кнопки Вверх/Вниз
local btnUp = Instance.new("TextButton", MainFrame)
btnUp.Size = UDim2.new(0, 85, 0, 40)
btnUp.Position = UDim2.new(0, 10, 0, 210)
btnUp.Text = "ВВЕРХ"
btnUp.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
btnUp.MouseButton1Down:Connect(function() VerticalSpeed = Speed end)
btnUp.MouseButton1Up:Connect(function() VerticalSpeed = 0 end)

local btnDown = Instance.new("TextButton", MainFrame)
btnDown.Size = UDim2.new(0, 85, 0, 40)
btnDown.Position = UDim2.new(0, 105, 0, 210)
btnDown.Text = "ВНИЗ"
btnDown.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
btnDown.MouseButton1Down:Connect(function() VerticalSpeed = -Speed end)
btnDown.MouseButton1Up:Connect(function() VerticalSpeed = 0 end)

-- Поле ввода скорости
local SpeedInput = Instance.new("TextBox", MainFrame)
SpeedInput.Size = UDim2.new(0, 180, 0, 40)
SpeedInput.Position = UDim2.new(0, 10, 0, 260)
SpeedInput.PlaceholderText = "Введите скорость..."
SpeedInput.Text = tostring(Speed)
SpeedInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
SpeedInput.TextColor3 = Color3.new(1,1,1)

local ApplyBtn = Instance.new("TextButton", MainFrame)
ApplyBtn.Size = UDim2.new(0, 180, 0, 40)
ApplyBtn.Position = UDim2.new(0, 10, 0, 310)
ApplyBtn.Text = "Применить скорость"
ApplyBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 50)
ApplyBtn.TextColor3 = Color3.new(1,1,1)
ApplyBtn.MouseButton1Click:Connect(function()
    local newSpeed = tonumber(SpeedInput.Text)
    if newSpeed then
        Speed = newSpeed
    end
end)

AddBtn("Закрыть", UDim2.new(0, 10, 0, 360), Color3.fromRGB(150, 30, 30), function() ScreenGui:Destroy() end)
