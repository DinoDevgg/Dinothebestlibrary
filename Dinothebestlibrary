local Library = {}
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "DynamicUI"
ScreenGui.Parent = CoreGui

local FloatingButton = nil
local MainWindow = nil
local IsWindowVisible = true
local ActiveTab = nil
local Tabs = {}
local WindowCreated = false

local function MakeDraggable(frame, dragHandle)
    local dragging = false
    local dragStart = nil
    local startPos = nil

    dragHandle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
        end
    end)

    dragHandle.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

local function CreateStyledButton(parent, text, size, position, callback, color)
    local btn = Instance.new("TextButton")
    btn.Size = size
    btn.Position = position
    btn.Text = text
    btn.BackgroundColor3 = color or Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.fromRGB(220, 220, 220)
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 14
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    btn.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(0, 200, 80)
    stroke.Thickness = 1
    stroke.Transparency = 0.7
    stroke.Parent = btn

    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15, Enum.EasingStyle.Quad), {BackgroundColor3 = Color3.fromRGB(60, 60, 60)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15, Enum.EasingStyle.Quad), {BackgroundColor3 = color or Color3.fromRGB(45, 45, 45)}):Play()
    end)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

function Library:CreateWindow()
    if WindowCreated then return MainWindow end
    WindowCreated = true

    MainWindow = Instance.new("Frame")
    MainWindow.Size = UDim2.new(0, 420, 0, 520)
    MainWindow.Position = UDim2.new(0.5, -210, 0.5, -260)
    MainWindow.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    MainWindow.BorderSizePixel = 0
    MainWindow.Parent = ScreenGui

    local mainCorner = Instance.new("UICorner")
    mainCorner.CornerRadius = UDim.new(0, 12)
    mainCorner.Parent = MainWindow

    local titleBar = Instance.new("Frame")
    titleBar.Size = UDim2.new(1, 0, 0, 40)
    titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    titleBar.BorderSizePixel = 0
    titleBar.Parent = MainWindow

    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 12)
    titleCorner.Parent = titleBar

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, -80, 1, 0)
    titleLabel.Position = UDim2.new(0, 15, 0, 0)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = "Dino Hub"
    titleLabel.TextColor3 = Color3.fromRGB(0, 200, 80)
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 18
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = titleBar

    local subLabel = Instance.new("TextLabel")
    subLabel.Size = UDim2.new(1, -80, 0, 18)
    subLabel.Position = UDim2.new(0, 15, 0, 22)
    subLabel.BackgroundTransparency = 1
    subLabel.Text = "By DinoDev"
    subLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
    subLabel.Font = Enum.Font.Gotham
    subLabel.TextSize = 12
    subLabel.TextXAlignment = Enum.TextXAlignment.Left
    subLabel.Parent = titleBar

    local closeBtn = Instance.new("TextButton")
    closeBtn.Size = UDim2.new(0, 30, 0, 30)
    closeBtn.Position = UDim2.new(1, -40, 0, 5)
    closeBtn.Text = "X"
    closeBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    closeBtn.TextColor3 = Color3.fromRGB(220, 220, 220)
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 16
    closeBtn.AutoButtonColor = false
    closeBtn.BorderSizePixel = 0
    closeBtn.Parent = titleBar

    local closeCorner = Instance.new("UICorner")
    closeCorner.CornerRadius = UDim.new(0, 6)
    closeCorner.Parent = closeBtn

    closeBtn.MouseButton1Click:Connect(function()
        IsWindowVisible = false
        TweenService:Create(MainWindow, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 1}):Play()
        for _, v in pairs(MainWindow:GetDescendants()) do
            if v:IsA("GuiObject") then
                TweenService:Create(v, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
            end
        end
        wait(0.2)
        MainWindow.Visible = false
        for _, v in pairs(MainWindow:GetDescendants()) do
            if v:IsA("GuiObject") and v ~= MainWindow then
                v.BackgroundTransparency = 0
                v.TextTransparency = 0
            end
        end
        MainWindow.BackgroundTransparency = 0
    end)

    MakeDraggable(MainWindow, titleBar)

    local tabBar = Instance.new("Frame")
    tabBar.Size = UDim2.new(1, 0, 0, 40)
    tabBar.Position = UDim2.new(0, 0, 0, 40)
    tabBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    tabBar.BackgroundTransparency = 0.5
    tabBar.BorderSizePixel = 0
    tabBar.Parent = MainWindow

    local container = Instance.new("Frame")
    container.Size = UDim2.new(1, -10, 1, -90)
    container.Position = UDim2.new(0, 5, 0, 85)
    container.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    container.BorderSizePixel = 0
    container.Parent = MainWindow

    local containerCorner = Instance.new("UICorner")
    containerCorner.CornerRadius = UDim.new(0, 8)
    containerCorner.Parent = container

    Tabs = {}

    local windowAPI = {}
    function windowAPI:CreateTab(name)
        local tabBtn = Instance.new("TextButton")
        tabBtn.Size = UDim2.new(0, 100, 1, 0)
        tabBtn.Text = name
        tabBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        tabBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
        tabBtn.Font = Enum.Font.GothamSemibold
        tabBtn.TextSize = 14
        tabBtn.AutoButtonColor = false
        tabBtn.BorderSizePixel = 0
        tabBtn.Parent = tabBar

        local tabCorner = Instance.new("UICorner")
        tabCorner.CornerRadius = UDim.new(0, 6)
        tabCorner.Parent = tabBtn

        local contentFrame = Instance.new("ScrollingFrame")
        contentFrame.Size = UDim2.new(1, -10, 1, -10)
        contentFrame.Position = UDim2.new(0, 5, 0, 5)
        contentFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
        contentFrame.BorderSizePixel = 0
        contentFrame.ScrollBarThickness = 4
        contentFrame.ScrollBarImageColor3 = Color3.fromRGB(0, 200, 80)
        contentFrame.Parent = container
        contentFrame.Visible = false

        local layout = Instance.new("UIListLayout")
        layout.Padding = UDim.new(0, 8)
        layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
        layout.Parent = contentFrame

        local padding = Instance.new("UIPadding")
        padding.PaddingTop = UDim.new(0, 8)
        padding.PaddingBottom = UDim.new(0, 8)
        padding.Parent = contentFrame

        tabBtn.MouseButton1Click:Connect(function()
            for _, v in pairs(Tabs) do
                v.btn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
                v.btn.TextColor3 = Color3.fromRGB(200, 200, 200)
                v.content.Visible = false
            end
            tabBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 80)
            tabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            contentFrame.Visible = true
        end)

        table.insert(Tabs, {btn = tabBtn, content = contentFrame})

        if not ActiveTab then
            tabBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 80)
            tabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            contentFrame.Visible = true
            ActiveTab = name
        end

        local tabAPI = {}
        function tabAPI:CreateButton(text, callback)
            local btn = CreateStyledButton(contentFrame, text, UDim2.new(0, 200, 0, 35), nil, callback, Color3.fromRGB(35, 35, 40))
            btn.Parent = contentFrame
            return btn
        end

        function tabAPI:CreateToggle(text, default)
            local toggleFrame = Instance.new("Frame")
            toggleFrame.Size = UDim2.new(0, 200, 0, 35)
            toggleFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
            toggleFrame.BorderSizePixel = 0
            toggleFrame.Parent = contentFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = toggleFrame

            local label = Instance.new("TextLabel")
            label.Size = UDim2.new(0.7, 0, 1, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Color3.fromRGB(220, 220, 220)
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = toggleFrame

            local toggleBtn = Instance.new("TextButton")
            toggleBtn.Size = UDim2.new(0, 50, 0, 25)
            toggleBtn.Position = UDim2.new(1, -60, 0.5, -12.5)
            toggleBtn.Text = default and "ON" or "OFF"
            toggleBtn.BackgroundColor3 = default and Color3.fromRGB(0, 200, 80) or Color3.fromRGB(60, 60, 60)
            toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
            toggleBtn.Font = Enum.Font.GothamBold
            toggleBtn.TextSize = 12
            toggleBtn.AutoButtonColor = false
            toggleBtn.BorderSizePixel = 0
            toggleBtn.Parent = toggleFrame

            local btnCorner = Instance.new("UICorner")
            btnCorner.CornerRadius = UDim.new(0, 4)
            btnCorner.Parent = toggleBtn

            local state = default
            local changeCallbacks = {}

            local function updateUI()
                toggleBtn.Text = state and "ON" or "OFF"
                toggleBtn.BackgroundColor3 = state and Color3.fromRGB(0, 200, 80) or Color3.fromRGB(60, 60, 60)
                for _, cb in pairs(changeCallbacks) do
                    cb(state)
                end
            end

            toggleBtn.MouseButton1Click:Connect(function()
                state = not state
                updateUI()
            end)

            local result = {}
            function result.OnChanged(callback)
                table.insert(changeCallbacks, callback)
                callback(state)
            end
            function result.SetValue(val)
                state = val
                updateUI()
            end
            function result.GetValue()
                return state
            end
            return result
        end

        function tabAPI:CreateSlider(text, min, max, default)
            local sliderFrame = Instance.new("Frame")
            sliderFrame.Size = UDim2.new(0, 200, 0, 65)
            sliderFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
            sliderFrame.BorderSizePixel = 0
            sliderFrame.Parent = contentFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = sliderFrame

            local label = Instance.new("TextLabel")
            label.Size = UDim2.new(0.7, 0, 0, 20)
            label.Position = UDim2.new(0, 5, 0, 5)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Color3.fromRGB(220, 220, 220)
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = sliderFrame

            local valueBox = Instance.new("TextBox")
            valueBox.Size = UDim2.new(0, 50, 0, 25)
            valueBox.Position = UDim2.new(1, -60, 0, 5)
            valueBox.Text = tostring(default)
            valueBox.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
            valueBox.TextColor3 = Color3.fromRGB(0, 200, 80)
            valueBox.Font = Enum.Font.Gotham
            valueBox.TextSize = 14
            valueBox.BorderSizePixel = 0
            valueBox.Parent = sliderFrame

            local valCorner = Instance.new("UICorner")
            valCorner.CornerRadius = UDim.new(0, 4)
            valCorner.Parent = valueBox

            local track = Instance.new("Frame")
            track.Size = UDim2.new(0.9, 0, 0, 4)
            track.Position = UDim2.new(0.05, 0, 1, -12)
            track.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
            track.BorderSizePixel = 0
            track.Parent = sliderFrame

            local fill = Instance.new("Frame")
            fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
            fill.BackgroundColor3 = Color3.fromRGB(0, 200, 80)
            fill.BorderSizePixel = 0
            fill.Parent = track

            local knob = Instance.new("TextButton")
            knob.Size = UDim2.new(0, 12, 0, 12)
            knob.Position = UDim2.new((default - min) / (max - min), -6, 0, -4)
            knob.BackgroundColor3 = Color3.fromRGB(0, 200, 80)
            knob.Text = ""
            knob.AutoButtonColor = false
            knob.BorderSizePixel = 0
            knob.Parent = track

            local knobCorner = Instance.new("UICorner")
            knobCorner.CornerRadius = UDim.new(1, 0)
            knobCorner.Parent = knob

            local value = default
            local changeCallbacks = {}

            local function updateSlider(newVal)
                newVal = math.clamp(newVal, min, max)
                value = newVal
                valueBox.Text = tostring(math.floor(value))
                local percent = (value - min) / (max - min)
                fill.Size = UDim2.new(percent, 0, 1, 0)
                knob.Position = UDim2.new(percent, -6, 0, -4)
                for _, cb in pairs(changeCallbacks) do
                    cb(value)
                end
            end

            local dragging = false
            knob.MouseButton1Down:Connect(function()
                dragging = true
            end)
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = false
                end
            end)
            track.MouseMove:Connect(function()
                if dragging then
                    local mousePos = UserInputService:GetMouseLocation()
                    local trackPos = track.AbsolutePosition
                    local percent = (mousePos.X - trackPos.X) / track.AbsoluteSize.X
                    updateSlider(min + percent * (max - min))
                end
            end)
            valueBox.FocusLost:Connect(function()
                local num = tonumber(valueBox.Text)
                if num then
                    updateSlider(num)
                else
                    valueBox.Text = tostring(value)
                end
            end)

            local result = {}
            function result.OnChanged(callback)
                table.insert(changeCallbacks, callback)
                callback(value)
            end
            function result.SetValue(val)
                updateSlider(val)
            end
            function result.GetValue()
                return value
            end
            return result
        end

        function tabAPI:CreateDropdown(text, options, default)
            local dropFrame = Instance.new("Frame")
            dropFrame.Size = UDim2.new(0, 200, 0, 35)
            dropFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
            dropFrame.BorderSizePixel = 0
            dropFrame.Parent = contentFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = UDim.new(0, 6)
            corner.Parent = dropFrame

            local label = Instance.new("TextLabel")
            label.Size = UDim2.new(0.5, 0, 1, 0)
            label.Position = UDim2.new(0, 5, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Color3.fromRGB(220, 220, 220)
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = dropFrame

            local selected = default or options[1]
            local selectBtn = Instance.new("TextButton")
            selectBtn.Size = UDim2.new(0, 80, 0, 25)
            selectBtn.Position = UDim2.new(1, -90, 0.5, -12.5)
            selectBtn.Text = selected
            selectBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
            selectBtn.TextColor3 = Color3.fromRGB(0, 200, 80)
            selectBtn.Font = Enum.Font.Gotham
            selectBtn.TextSize = 12
            selectBtn.AutoButtonColor = false
            selectBtn.BorderSizePixel = 0
            selectBtn.Parent = dropFrame

            local btnCorner = Instance.new("UICorner")
            btnCorner.CornerRadius = UDim.new(0, 4)
            btnCorner.Parent = selectBtn

            local dropdownMenu = Instance.new("Frame")
            dropdownMenu.Size = UDim2.new(0, 80, 0, 0)
            dropdownMenu.Position = UDim2.new(1, -90, 1, 2)
            dropdownMenu.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
            dropdownMenu.BorderSizePixel = 0
            dropdownMenu.ClipsDescendants = true
            dropdownMenu.Visible = false
            dropdownMenu.Parent = dropFrame

            local menuCorner = Instance.new("UICorner")
            menuCorner.CornerRadius = UDim.new(0, 6)
            menuCorner.Parent = dropdownMenu

            local listLayout = Instance.new("UIListLayout")
            listLayout.Padding = UDim.new(0, 2)
            listLayout.Parent = dropdownMenu

            local changeCallbacks = {}

            local function updateMenu()
                for _, child in pairs(dropdownMenu:GetChildren()) do
                    if child:IsA("TextButton") then child:Destroy() end
                end
                local height = 0
                for _, opt in pairs(options) do
                    local optBtn = Instance.new("TextButton")
                    optBtn.Size = UDim2.new(1, 0, 0, 28)
                    optBtn.Text = opt
                    optBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
                    optBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
                    optBtn.Font = Enum.Font.Gotham
                    optBtn.TextSize = 12
                    optBtn.AutoButtonColor = false
                    optBtn.BorderSizePixel = 0
                    optBtn.Parent = dropdownMenu

                    local optCorner = Instance.new("UICorner")
                    optCorner.CornerRadius = UDim.new(0, 4)
                    optCorner.Parent = optBtn

                    optBtn.MouseButton1Click:Connect(function()
                        selected = opt
                        selectBtn.Text = selected
                        dropdownMenu.Visible = false
                        for _, cb in pairs(changeCallbacks) do
                            cb(selected)
                        end
                    end)
                    height = height + 30
                end
                dropdownMenu.Size = UDim2.new(0, 80, 0, height)
            end
            updateMenu()

            selectBtn.MouseButton1Click:Connect(function()
                dropdownMenu.Visible = not dropdownMenu.Visible
            end)

            local result = {}
            function result.OnChanged(callback)
                table.insert(changeCallbacks, callback)
                callback(selected)
            end
            function result.SetValue(opt)
                if table.find(options, opt) then
                    selected = opt
                    selectBtn.Text = selected
                    for _, cb in pairs(changeCallbacks) do
                        cb(selected)
                    end
                end
            end
            function result.GetValue()
                return selected
            end
            return result
        end

        return tabAPI
    end

    MainWindow.Visible = true
    return windowAPI
end

FloatingButton = Instance.new("ImageButton")
FloatingButton.Size = UDim2.new(0, 50, 0, 50)
FloatingButton.Position = UDim2.new(1, -70, 0.5, -25)
FloatingButton.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
FloatingButton.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"
FloatingButton.BorderSizePixel = 0
FloatingButton.Parent = ScreenGui

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 12)
btnCorner.Parent = FloatingButton

local btnStroke = Instance.new("UIStroke")
btnStroke.Color = Color3.fromRGB(0, 200, 80)
btnStroke.Thickness = 2
btnStroke.Parent = FloatingButton

local dragDetector = Instance.new("UIDragDetector")
dragDetector.Parent = FloatingButton

local originalSize = FloatingButton.Size
FloatingButton.MouseEnter:Connect(function()
    TweenService:Create(FloatingButton, TweenInfo.new(0.15), {Size = UDim2.new(0, 55, 0, 55)}):Play()
end)
FloatingButton.MouseLeave:Connect(function()
    TweenService:Create(FloatingButton, TweenInfo.new(0.15), {Size = originalSize}):Play()
end)

FloatingButton.MouseButton1Click:Connect(function()
    TweenService:Create(FloatingButton, TweenInfo.new(0.1, Enum.EasingStyle.Back), {Size = UDim2.new(0, 45, 0, 45)}):Play()
    wait(0.1)
    TweenService:Create(FloatingButton, TweenInfo.new(0.1), {Size = originalSize}):Play()
    if MainWindow then
        if MainWindow.Visible then
            IsWindowVisible = false
            TweenService:Create(MainWindow, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 1}):Play()
            for _, v in pairs(MainWindow:GetDescendants()) do
                if v:IsA("GuiObject") then
                    TweenService:Create(v, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
                end
            end
            wait(0.2)
            MainWindow.Visible = false
            for _, v in pairs(MainWindow:GetDescendants()) do
                if v:IsA("GuiObject") and v ~= MainWindow then
                    v.BackgroundTransparency = 0
                    v.TextTransparency = 0
                end
            end
            MainWindow.BackgroundTransparency = 0
        else
            MainWindow.Visible = true
            MainWindow.BackgroundTransparency = 1
            for _, v in pairs(MainWindow:GetDescendants()) do
                if v:IsA("GuiObject") then
                    v.BackgroundTransparency = 1
                    v.TextTransparency = 1
                end
            end
            TweenService:Create(MainWindow, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 0}):Play()
            for _, v in pairs(MainWindow:GetDescendants()) do
                if v:IsA("GuiObject") and v ~= MainWindow then
                    TweenService:Create(v, TweenInfo.new(0.2, Enum.EasingStyle.Quad), {BackgroundTransparency = 0, TextTransparency = 0}):Play()
                end
            end
            IsWindowVisible = true
        end
    end
end)

function Library:SetIcon(imageId)
    if FloatingButton then
        FloatingButton.Image = "rbxassetid://" .. tostring(imageId)
    end
end

return Library
