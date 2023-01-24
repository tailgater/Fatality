local Players, Uis, RService, Inset, CurrentCamera = 
game:GetService("Players"), 
game:GetService("UserInputService"), 
game:GetService("RunService"),
game:GetService("GuiService"):GetGuiInset().Y,
game:GetService("Workspace").CurrentCamera

local Client = Players.LocalPlayer;

local Mouse, Camera = Client:GetMouse(), workspace.CurrentCamera

local Circle = Drawing.new("Circle")

local CF, RNew, Vec3, Vec2 = CFrame.new, Ray.new, Vector3.new, Vector2.new

local OldAimPart = getgenv().Settings.Part.AimPart

local AimlockTarget, MousePressed, CanNotify = nil, false, false

getgenv().UpdateFOV = function()
    if (not Circle) then
        return (Circle)
    end
    Circle.Color = Settings.Visual.FovColor
    Circle.Visible = Settings.Visual.Fov
    Circle.Radius = Settings.Visual.FovRadius
    Circle.Thickness = Settings.Visual.FovThickness
    Circle.Position = Vec2(Mouse.X, Mouse.Y + Inset)
    return (Circle)
end

RService.Heartbeat:Connect(UpdateFOV)

-- // Functions

getgenv().WallCheck = function(destination, ignore)
    local Origin = Camera.CFrame.p
    local CheckRay = RNew(Origin, destination - Origin)
    local Hit = game.workspace:FindPartOnRayWithIgnoreList(CheckRay, ignore)
    return Hit == nil
end

getgenv().WTS = function(Object)
    local ObjectVector = Camera:WorldToScreenPoint(Object.Position)
    return Vec2(ObjectVector.X, ObjectVector.Y)
end

getgenv().IsOnScreen = function(Object)
    local IsOnScreen = Camera:WorldToScreenPoint(Object.Position)
    return IsOnScreen
end

getgenv().FilterObjs = function(Object)
    if string.find(Object.Name, "Gun") then
        return
    end
    if table.find({"Part", "MeshPart", "BasePart"}, Object.ClassName) then
        return true
    end
end

getgenv().GetClosestBodyPart = function(character)
    local ClosestDistance = 1 / 0
    local BodyPart = nil
    if (character and character:GetChildren()) then
        for _, x in next, character:GetChildren() do
            if FilterObjs(x) and IsOnScreen(x) then
                local Distance = (WTS(x) - Vec2(Mouse.X, Mouse.Y)).Magnitude
                if (Circle.Radius > Distance and Distance < ClosestDistance) then
                    ClosestDistance = Distance
                    BodyPart = x
                end
            end
        end
    end
    return BodyPart
end

getgenv().WorldToViewportPoint = function(P)
    return Camera:WorldToViewportPoint(P)
end

getgenv().WorldToScreenPoint = function(P)
    return Camera.WorldToScreenPoint(Camera, P)
end

getgenv().GetObscuringObjects = function(T)
    if T and T:FindFirstChild(getgenv().Settings.Part.AimPart) and Client and Client.Character:FindFirstChild("Head") then
        local RayPos =
            workspace:FindPartOnRay(RNew(T[getgenv().Settings.Part.AimPart].Position, Client.Character.Head.Position))
        if RayPos then
            return RayPos:IsDescendantOf(T)
        end
    end
end

getgenv().GetNearestTarget = function()
    local AimlockTarget, Closest = nil, 1 / 0

    for _, v in pairs(game:GetService("Players"):GetPlayers()) do
        if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
            local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
            local Distance = (Vec2(Position.X, Position.Y) - Vec2(Mouse.X, Mouse.Y)).Magnitude
            if Settings.Main.CheckForWalls then
                if
                    (Circle.Radius > Distance and Distance < Closest and OnScreen and
                        getgenv().WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}))
                 then
                    Closest = Distance
                    AimlockTarget = v
                end
            elseif Settings.Main.UseCircleRadius then
                if
                    (Circle.Radius > Distance and Distance < Closest and OnScreen and
                        getgenv().WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}))
                 then
                    Closest = Distance
                    AimlockTarget = v
                end
            else
                if (Circle.Radius > Distance and Distance < Closest and OnScreen) then
                    Closest = Distance
                    AimlockTarget = v
                end
            end
        end
    end
    return AimlockTarget
end

-- // Use KeyBoardKey Function

Uis.InputBegan:connect(
    function(input)
        if
            input.KeyCode == Settings.Main.KeyBoardKey and Settings.Main.UseKeyBoardKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.Main.HoldKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        end
    end
)Uis.InputEnded:connect(
    function(input)
        if
            input.KeyCode == Settings.Main.KeyBoardKey and getgenv().Settings.Main.HoldKey == true and
                Settings.Main.UseKeyBoardKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget ~= nil
         then
            AimlockTarget = nil
            MousePressed = false
        end
    end
)

Uis.InputBegan:Connect(
    function(keyinput, stupid)
        if
            keyinput.KeyCode == Settings.Resolver.UnderGroundKey and getgenv().Settings.Main.Enable == true and
                Settings.Resolver.UseUnderGroundKeybind == true
         then
            if Settings.Resolver.UnderGround == true then
                Settings.Resolver.UnderGround = false
                if getgenv().Settings.Resolver.SendNotification then
                    game.StarterGui:SetCore(
                        "SendNotification",
                        {
                            Title = "Sanky Box",
                            Text = "Disabled UnderGround Resolver",
                            Icon = "rbxassetid://11394475200",
                            Duration = 1
                        }
                    )
                end
            else
                Settings.Resolver.UnderGround = true
                if getgenv().Settings.Resolver.SendNotification then
                    game.StarterGui:SetCore(
                        "SendNotification",
                        {
                            Title = "Sanky Box",
                            Text = "Enabled UnderGround Resolver",
                            Icon = "rbxassetid://11394475200",
                            Duration = 1
                        }
                    )
                end
            end
        end
    end
)

Uis.InputBegan:Connect(
    function(keyinput, stupid)
        if
            keyinput.KeyCode == Settings.Resolver.DetectDesyncKey and getgenv().Settings.Main.Enable == true and
                Settings.Resolver.UseDetectDesyncKeybind == true
         then
            if Settings.Resolver.DetectDesync == true then
                Settings.Resolver.DetectDesync = false
                if getgenv().Settings.Resolver.SendNotification then
                    game.StarterGui:SetCore(
                        "SendNotification",
                        {
                            Title = "Sanky Box",
                            Text = "Disabled Desync Resolver",
                            Icon = "rbxassetid://11394475200",
                            Duration = 1
                        }
                    )
                end
            else
                Settings.Resolver.DetectDesync = true
                if getgenv().Settings.Resolver.SendNotification then
                    game.StarterGui:SetCore(
                        "SendNotification",
                        {
                            Title = "Sanky Box",
                            Text = "Enabled Desync Resolver",
                            Icon = "rbxassetid://11394475200",
                            Duration = 1
                        }
                    )
                end
            end
        end
    end
)

Uis.InputBegan:Connect(
    function(keyinput, stupid)
        if
            keyinput.KeyCode == Settings.Main.KeyBoardKey and Settings.Main.UseKeyBoardKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.Main.ToggleKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        elseif
            keyinput.KeyCode == Settings.Main.KeyBoardKey and Settings.Main.UseKeyBoardKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget ~= nil
         then
            AimlockTarget = nil
            MousePressed = false
        end
    end
)

-- // Use MouseKey Function

Uis.InputBegan:connect(
    function(input)
        if
            input.UserInputType == Settings.Main.MouseKey and Settings.Main.UseMouseKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.Main.HoldKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        end
    end
)Uis.InputEnded:connect(
    function(input)
        if
            input.UserInputType == Settings.Main.MouseKey and getgenv().Settings.Main.HoldKey == true and
                Settings.Main.UseMouseKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget ~= nil
         then
            AimlockTarget = nil
            MousePressed = false
        end
    end
)

Uis.InputBegan:Connect(
    function(keyinput, stupid)
        if
            keyinput.UserInputType == Settings.Main.MouseKey and Settings.Main.UseMouseKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget == nil and
                getgenv().Settings.Main.ToggleKey == true
         then
            pcall(
                function()
                    MousePressed = true
                    AimlockTarget = GetNearestTarget()
                end
            )
        elseif
            keyinput.UserInputType == Settings.Main.MouseKey and Settings.Main.UseMouseKey == true and
                getgenv().Settings.Main.Enable == true and
                AimlockTarget ~= nil
         then
            AimlockTarget = nil
            MousePressed = false
        end
    end
)

-- // Main Functions. RunService HeartBeat.

task.spawn(
    function()
        while task.wait() do
            if MousePressed == true and getgenv().Settings.Main.Enable == true then
                if AimlockTarget and AimlockTarget.Character then
                    if getgenv().Settings.Part.GetClosestPart == true then
                        getgenv().Settings.Part.AimPart = tostring(GetClosestBodyPart(AimlockTarget.Character))
                    end
                end
                if getgenv().Settings.Main.DisableOutSideCircle == true and AimlockTarget and AimlockTarget.Character then
                    if
                        Circle.Radius <
                            (Vec2(
                                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).X,
                                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).Y
                            ) - Vec2(Mouse.X, Mouse.Y)).Magnitude
                     then
                        AimlockTarget = nil
                    end
                end
            end
        end
    end
)

RService.Heartbeat:Connect(
    function()
        if getgenv().Settings.Main.Enable == true and MousePressed == true then
            if getgenv().Settings.Main.UseShake == true and AimlockTarget and AimlockTarget.Character then
                pcall(
                    function()
                        local TargetVelv1 = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVelv1.Velocity =
                            Vec3(TargetVelv1.Velocity.X, TargetVelv1.Velocity.Y, TargetVelv1.Velocity.Z) +
                            Vec3(
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower),
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower),
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower)
                            ) *
                                0.1
                        TargetVelv1.AssemblyLinearVelocity =
                            Vec3(TargetVelv1.Velocity.X, TargetVelv1.Velocity.Y, TargetVelv1.Velocity.Z) +
                            Vec3(
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower),
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower),
                                math.random(-getgenv().Settings.Main.ShakePower, getgenv().Settings.Main.ShakePower)
                            ) *
                                0.1
                    end
                )
            end
            if getgenv().Settings.Resolver.UnderGround == true and AimlockTarget and AimlockTarget.Character then
                pcall(
                    function()
                        local TargetVelv2 = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVelv2.Velocity = Vec3(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                        TargetVelv2.AssemblyLinearVelocity = Vec3(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                    end
                )
            end
            if
                getgenv().Settings.Resolver.DetectDesync == true and AimlockTarget and AimlockTarget.Character and
                    AimlockTarget.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude >
                        getgenv().Settings.Resolver.Detection
             then
                pcall(
                    function()
                        local TargetVel = AimlockTarget.Character[getgenv().Settings.Part.AimPart]
                        TargetVel.Velocity = Vec3(0, 0, 0)
                        TargetVel.AssemblyLinearVelocity = Vec3(0, 0, 0)
                    end
                )
            end
            if getgenv().Settings.Main.ThirdPerson == true and getgenv().Settings.Main.FirstPerson == true then
                if
                    (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 or
                        (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1
                 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            elseif getgenv().Settings.Main.ThirdPerson == true and getgenv().Settings.Main.FirstPerson == false then
                if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            elseif getgenv().Settings.Main.ThirdPerson == false and getgenv().Settings.Main.FirstPerson == true then
                if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude <= 1 then
                    CanNotify = true
                else
                    CanNotify = false
                end
            end
            if getgenv().Settings.Main.AutoPingSets == true and getgenv().Settings.Horizontal.PredictionVelocity then
                local pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
                local split = string.split(pingvalue, "(")
                local ping = tonumber(split[1])
                if ping > 190 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.206547
                elseif ping > 180 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.19284
                elseif ping > 170 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.1923111
                elseif ping > 160 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.1823111
                elseif ping > 150 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.171
                elseif ping > 140 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.165773
                elseif ping > 130 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.1223333
                elseif ping > 120 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.143765
                elseif ping > 110 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.1455
                elseif ping > 100 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.130340
                elseif ping > 90 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.136
                elseif ping > 80 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.1347
                elseif ping > 70 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.119
                elseif ping > 60 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.12731
                elseif ping > 50 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.127668
                elseif ping > 40 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.125
                elseif ping > 30 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.11
                elseif ping > 20 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.12588
                elseif ping > 10 then
                    getgenv().Settings.Horizontal.PredictionVelocity = 0.9
                end
            end
            if getgenv().Settings.Check.CheckIfKo == true and AimlockTarget and AimlockTarget.Character then
                local KOd = AimlockTarget.Character:WaitForChild("BodyEffects")["K.O"].Value
                local Grabbed = AimlockTarget.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
                if AimlockTarget.Character.Humanoid.health < 1 or KOd or Grabbed then
                    if MousePressed == true then
                        AimlockTarget = nil
                        MousePressed = false
                    end
                end
            end
            if
                getgenv().Settings.Check.DisableOnTargetDeath == true and AimlockTarget and
                    AimlockTarget.Character:FindFirstChild("Humanoid")
             then
                if AimlockTarget.Character.Humanoid.health < 1 then
                    if MousePressed == true then
                        AimlockTarget = nil
                        MousePressed = false
                    end
                end
            end
            if
                getgenv().Settings.Check.DisableOnPlayerDeath == true and Client.Character and
                    Client.Character:FindFirstChild("Humanoid") and
                    Client.Character.Humanoid.health < 1
             then
                if MousePressed == true then
                    AimlockTarget = nil
                    MousePressed = false
                end
            end
            if getgenv().Settings.Part.CheckIfJumped == true and getgenv().Settings.Part.GetClosestPart == false then
                if AimlockTarget and AimlockTarget.Character then
                    if AimlockTarget.Character.Humanoid.FloorMaterial == Enum.Material.Air then
                        getgenv().Settings.Part.AimPart = getgenv().Settings.Part.CheckIfJumpedAimPart
                    else
                        getgenv().Settings.Part.AimPart = OldAimPart
                    end
                end
            end
            if
                AimlockTarget and AimlockTarget.Character and
                    AimlockTarget.Character:FindFirstChild(getgenv().Settings.Part.AimPart)
             then
                if getgenv().Settings.Main.FirstPerson == true then
                    if CanNotify == true then
                        if getgenv().Settings.Horizontal.PredictMovement == true then
                            if getgenv().Settings.Smooth.Smoothness == true then
                                local Main =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position +
                                        AimlockTarget.Character[getgenv().Settings.Part.AimPart].Velocity *
                                            getgenv().Settings.Horizontal.PredictionVelocity
                                )

                                Camera.CFrame =
                                    Camera.CFrame:Lerp(
                                    Main,
                                    getgenv().Settings.Smooth.SmoothnessAmount,
                                    Enum.EasingStyle.Elastic,
                                    Enum.EasingDirection.InOut
                                )
                            else
                                Camera.CFrame =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position +
                                        AimlockTarget.Character[getgenv().Settings.Part.AimPart].Velocity *
                                            getgenv().Settings.Horizontal.PredictionVelocity + Vector3
                                )
                            end
                        elseif getgenv().Settings.Horizontal.PredictMovement == false then
                            if getgenv().Settings.Smooth.Smoothness == true then
                                local Main =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position
                                )
                                Camera.CFrame =
                                    Camera.CFrame:Lerp(
                                    Main,
                                    getgenv().Settings.Smooth.SmoothnessAmount,
                                    getgenv().Settings.Smooth.SmoothMethod,
                                    getgenv().Settings.Smooth.SmoothMethodV2
                                )
                            else
                                Camera.CFrame =
                                    CF(
                                    Camera.CFrame.p,
                                    AimlockTarget.Character[getgenv().Settings.Part.AimPart].Position
                                )
                            end
                        end
                    end
                end
            end
        end
    end
)
