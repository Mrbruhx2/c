local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("KUNAI HUB | MM2 (โปรน้องแมวจีโน่คุง)", "Midnight")
local Tab = Window:NewTab("หน้าหลัก")
local Section = Tab:NewSection("วาร์ปไปหาผู้เล่น")
Plr = {}
for i,v in pairs(game:GetService("Players"):GetChildren()) do
    table.insert(Plr,v.Name) 
end
local drop = Section:NewDropdown("", "กดเพื่อวาร์ป", Plr, function(t)
   PlayerTP = t
end)
Section:NewButton("กดเพื่อวาร์ป", "", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players[PlayerTP].Character.HumanoidRootPart.CFrame
end)
Section:NewToggle("วาร์ปอัตโนมัติ", "", function(t)
_G.TPPlayer = t
while _G.TPPlayer do wait()
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players[PlayerTP].Character.HumanoidRootPart.CFrame
end
end)

Section:NewButton("Refresh Dropdown","Refresh Dropdown", function()
  drop:Refresh(Plr)
end)
local Section = Tab:NewSection("SPEEDS")
Section:NewSlider("เลื่อนเพื่อเพิ่มความเร็ว", "เลื่อนขึ้นเพื่อวิ่ง (สามารถปรับได้เต็มที่ถึง 500)", 500, 15, function(s) -- 500 (MaxValue) | 15 (MinValue)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = s
end)
local Section = Tab:NewSection("JPOWERS-JUMPPOWERS")
Section:NewSlider("เลื่อนเพื่อเพิ่มพลังกระโดด", "เลื่อนขึ้นเพื่อเพิ่มพลังกระโดดของมึง (สามารถปรับได้เต็มที่ถึง 500)", 500, 50, function(s) -- 500 (MaxValue) | 50 (MinValue)
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = s
end)

local Tab = Window:NewTab("การตั้งค่า")
local Section = Tab:NewSection("เปิด-ปิด Ui")
Section:NewKeybind("ตั้งค่าปุ่มเปิดปิด Ui", "KeybindInfo", Enum.KeyCode.RightControl, function()
	Library:ToggleUI()
end)
Section:NewTextBox("ไม่มีไร", "ควย", function(txt)
	print(txt)
end)
Section:NewLabel("ถ้าไม่พอใจที่ผมก็อป Src มาก็อย่ามาวีนใส่เลยครับผมแค่เอามาเล่นคนเดียว")
local Section = Tab:NewSection("Esp")
Section:NewButton("ESP - Button", "ButtonInfo", function()
    print("Clicked")
    while wait() do
     pcall(function()
       for i,v in pairs(game.Players:GetChildren()) do
            if not v.Character.Head:FindFirstChild("ESP") then
                local BillboardGui = Instance.new("BillboardGui")
                local TextLabel = Instance.new("TextLabel")
                BillboardGui.Parent = v.Character.Head
                BillboardGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
                BillboardGui.Active = true
                BillboardGui.Name = "ESP"
                BillboardGui.AlwaysOnTop = true
                BillboardGui.LightInfluence = 1.000
                BillboardGui.Size = UDim2.new(0, 200, 0, 50)
                BillboardGui.StudsOffset = Vector3.new(0, 2.5, 0)
                TextLabel.Parent = BillboardGui
                TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                TextLabel.BackgroundTransparency = 1.000
                TextLabel.Size = UDim2.new(0, 200, 0, 50)
                TextLabel.Font = Enum.Font.GothamBold
                TextLabel.Text = v.Name
                TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                TextLabel.TextScaled = true
                TextLabel.TextSize = 14.000
                TextLabel.TextStrokeTransparency = 0.000
                TextLabel.TextWrapped = true
            end
        end
    end) 
end
end)
local FlyTab = Window:NewTab("Fly")
local FlySection = FlyTab:NewSection("Fly Settings")
 
-- SETUP
_G.FlySpeed = 50
if _G.Setup then
    game.Players.LocalPlayer:Kick("[WARNING] Already executed CFrameFly!")
else
    _G.Setup = true
end
 
-- UI And Fly Functions
FlySection:NewButton("Fly (KeyBind 'G')", "Set Player to Fly (KeyBind = G)!", function()
    spawn(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/LegitH3x0R/Roblox-Scripts/main/AEBypassing/RootAnchor.lua"))()
 
        local UIS = game:GetService("UserInputService")
        local OnRender = game:GetService("RunService").RenderStepped
        
        local Player = game:GetService("Players").LocalPlayer
        local Character = Player.Character or Player.CharacterAdded:Wait()
        
        local Camera = workspace.CurrentCamera
        local Root = Character:WaitForChild("HumanoidRootPart")
        local C1, C2, C3;
        local Nav = {Flying = false, Forward = false, Backward = false, Left = false, Right = false}
        C1 = UIS.InputBegan:Connect(function(Input)
            if Input.UserInputType == Enum.UserInputType.Keyboard then
                if Input.KeyCode == Enum.KeyCode.G then
                    Nav.Flying = not Nav.Flying
                    Root.Anchored = Nav.Flying
                elseif Input.KeyCode == Enum.KeyCode.W then
                    Nav.Forward = true
                elseif Input.KeyCode == Enum.KeyCode.S then
                    Nav.Backward = true
                elseif Input.KeyCode == Enum.KeyCode.A then
                    Nav.Left = true
                elseif Input.KeyCode == Enum.KeyCode.D then
                    Nav.Right = true
                end
            end
        end)
        
        C2 = UIS.InputEnded:Connect(function(Input)
            if Input.UserInputType == Enum.UserInputType.Keyboard then
                if Input.KeyCode == Enum.KeyCode.W then
                    Nav.Forward = false
                elseif Input.KeyCode == Enum.KeyCode.S then
                    Nav.Backward = false
                elseif Input.KeyCode == Enum.KeyCode.A then
                    Nav.Left = false
                elseif Input.KeyCode == Enum.KeyCode.D then
                    Nav.Right = false
                end
            end
        end)
        
        C3 = Camera:GetPropertyChangedSignal("CFrame"):Connect(function()
            if Nav.Flying then
                Root.CFrame = CFrame.new(Root.CFrame.Position, Root.CFrame.Position + Camera.CFrame.LookVector)
            end
        end)
        
        while true do
            local Delta = OnRender:Wait()
            if Nav.Flying then
                if Nav.Forward then
                    Root.CFrame = Root.CFrame + (Camera.CFrame.LookVector * (Delta * _G.FlySpeed))
                end
                if Nav.Backward then
                    Root.CFrame = Root.CFrame + (-Camera.CFrame.LookVector * (Delta * _G.FlySpeed))
                end
                if Nav.Left then
                    Root.CFrame = Root.CFrame + (-Camera.CFrame.RightVector * (Delta * _G.FlySpeed))
                end
                if Nav.Right then
                    Root.CFrame = Root.CFrame + (Camera.CFrame.RightVector * (Delta * _G.FlySpeed))
                end
            end
        end
    end)
end)
 
FlySection:NewSlider("Fly Speed", "Speed of the Fly!", 250, 10, function(s) -- 250 (MaxValue) | 0 (MinValue)
    _G.FlySpeed = s
end)
 

FlySection:NewKeybind("Toggle GUI!", "The Key to Toggle the UI!", Enum.KeyCode.RightShift, function()
    Library:ToggleUI()
end)
