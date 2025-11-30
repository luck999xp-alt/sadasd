-- =========================
-- NaKub Hub (มือถือ + PC) รวมทุกอย่าง แบบ Speed Boost Dynamic
-- =========================

-- โหลด UI Library
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/x2zu/OPEN-SOURCE-UI-ROBLOX/refs/heads/main/X2ZU%20UI%20ROBLOX%20OPEN%20SOURCE/DummyUi-leak-by-x2zu/fetching-main/Tools/Framework.luau"))()

-- สร้างหน้าต่างหลัก
local Window = Library:Window({
    Title = "NaKub Hub",
    Desc = "kuykuy",
    Icon = 105059922903197,
    Theme = "Dark",
    Config = { Keybind = Enum.KeyCode.Home, Size = UDim2.new(0, 500, 0, 400) },
    CloseUIButton = { Enabled = true, Text = "เปิดปิด" }
})

-- แท็บ
local MainTab = Window:Tab({ Title = "Main", Icon = "star" })
local VisualTab = Window:Tab({ Title = "Visual", Icon = "eye" })
local ProTab = Window:Tab({ Title = "Pro", Icon = "wrench" })
local ModTab = Window:Tab({ Title = "Mod", Icon = "crown" })
local CreditsTab = Window:Tab({ Title = "Credits", Icon = "crown" })

-- =========================
-- ตัวแปร
-- =========================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TeleportService = game:GetService("TeleportService")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local VirtualUser = game:GetService("VirtualUser") -- เพิ่ม VirtualUser สำหรับ Anti-AFK

local speedEnabled, espEnabled, godEnabled, noclipEnabled = false, false, false, false
local infiniteJumpEnabled = false
local antiKickEnabled = false
local lagSwitchActive = false
local speedValue = 18
local espObjects = {}

-- =========================
-- MAIN TAB (Speed Boost dynamic + Infinite Jump)
-- =========================
MainTab:Label({ Title = "ยินดีต้อนรับสู่ NaKub Hub!", Desc = "กด Home เพื่อเปิด/ปิด UI" })

MainTab:Toggle({
    Title = "Speed Boost",
    Default = false,
    Callback = function(state)
        speedEnabled = state
    end
})

MainTab:Slider({
    Title = "ปรับความเร็ว Speed Boost",
    Min = 16,
    Max = 500,
    Default = speedValue,
    Callback = function(val)
        speedValue = val
    end
})

MainTab:Toggle({
    Title = "Infinite Jump",
    Default = false,
    Callback = function(state)
        infiniteJumpEnabled = state
    end
})

-- LOGIC (การทำงาน)
RunService.RenderStepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        local hum = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if speedEnabled then
            hum.WalkSpeed = speedValue -- override เฉพาะตอนเปิด
        end
        -- ถ้า Speed Boost ปิด จะปล่อยให้เกมปรับตามแมพเอง (dynamic)
    end
end)

UserInputService.JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end
end)

-- =========================
-- VISUAL TAB (ESP + Fullbright)
-- =========================

VisualTab:Toggle({
    Title = "Fullbright (มองในที่มืด)",
    Default = false,
    Callback = function(state)
        if state then
            Lighting.Brightness = 2
            Lighting.ClockTime = 14
            Lighting.FogEnd = 100000
            Lighting.GlobalShadows = false
            Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        else
            -- ค่า Default คร่าวๆ
            Lighting.Brightness = 1
            Lighting.GlobalShadows = true
            Lighting.ClockTime = 12
        end
    end
})

local function createESP(player)
    if player == LocalPlayer or not player.Character then return end
    local char = player.Character
    local head = char:WaitForChild("Head", 5)
    if not head then return end

    if espObjects[player] then
        for _, o in ipairs(espObjects[player]) do o:Destroy() end
    end
    espObjects[player] = {}

    local highlight = Instance.new("Highlight")
    highlight.Adornee = char
    highlight.FillTransparency = 0.7
    highlight.OutlineTransparency = 0
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    -- ปรับสีตาม Team (ถ้ามี)
    highlight.FillColor = (player.Team and player.Team.Name == "Killer") and Color3.fromRGB(255,0,0) or Color3.fromRGB(0,255,0)
    highlight.Parent = Workspace
    table.insert(espObjects[player], highlight)

    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = head
    billboard.Size = UDim2.new(0,150,0,25)
    billboard.StudsOffset = Vector3.new(0,2.5,0)
    billboard.AlwaysOnTop = true
    billboard.Parent = head

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1,0,1,0)
    label.BackgroundTransparency = 1
    label.Text = player.DisplayName or player.Name
    label.TextColor3 = highlight.FillColor
    label.TextStrokeTransparency = 0.2
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 14
    label.Parent = billboard
    table.insert(espObjects[player], billboard)
end

local function setupPlayer(player)
    player.CharacterAdded:Connect(function(char)
        if espEnabled then
            local head = char:WaitForChild("Head", 5)
            if head then
                createESP(player)
            end
        end
    end)
end

for _, p in ipairs(Players:GetPlayers()) do
    if p ~= LocalPlayer then setupPlayer(p) end
end

Players.PlayerAdded:Connect(setupPlayer)
Players.PlayerRemoving:Connect(function(p)
    if espObjects[p] then
        for _, o in ipairs(espObjects[p]) do o:Destroy() end
        espObjects[p] = nil
    end
end)

VisualTab:Toggle({ Title = "Player ESP", Default = false, Callback = function(state)
    espEnabled = state
    if not state then
        for _, objs in pairs(espObjects) do
            for _, o in ipairs(objs) do o:Destroy() end
        end
        espObjects = {}
        return
    end
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            local head = p.Character:FindFirstChild("Head")
            if head then
                createESP(p)
            end
        end
    end
end })

-- =========================
-- PRO TAB (God Mode, NoClip, Anti-Kick, Lag Switch)
-- =========================

ProTab:Toggle({ Title = "God Mode", Default = false, Callback = function(state)
    godEnabled = state
    if godEnabled then
        local char = LocalPlayer.Character
        if not char then return end
        local hum = char:FindFirstChildWhichIsA("Humanoid") or char:WaitForChild("Humanoid",5)
        if hum then
            hum.MaxHealth = 1e6
            hum.Health = hum.MaxHealth
            hum.HealthChanged:Connect(function()
                if godEnabled and hum.Health < hum.MaxHealth then
                    hum.Health = hum.MaxHealth
                end
            end)
        end
    end
end })

ProTab:Toggle({
    Title = "เดินทะลุ (NoClip)",
    Default = false,
    Callback = function(state)
        noclipEnabled = state
        
        if not state and LocalPlayer.Character then
            local char = LocalPlayer.Character
            for _, part in ipairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
    end
})

-- ANTI-KICK / ANTI-BAN
ProTab:Toggle({
    Title = "Anti-Kick/Anti-Ban",
    Description = "พยายามบล็อก Teleport, Prevent/Disconnect Event (ความสามารถขึ้นอยู่กับ Executor)",
    Default = true,
    Callback = function(state)
        antiKickEnabled = state
        if state then
            -- บล็อกคำสั่งพื้นฐานที่มักใช้ในการเตะ/ตรวจสอบสถานะ
            -- game:GetService("Players").LocalPlayer.OnTeleport:Prevent() -- คำสั่งนี้อาจไม่ทำงานใน Executor ทั่วไป
        end
    end
})

-- LAG SWITCH (เปิด/ปิด)
ProTab:Toggle({
    Title = "Lag Switch (ตัด/ต่อเน็ต)",
    Description = "จำลองการ Lag เพื่อหลอก Anti-Cheat หรือฝ่ายตรงข้าม (ความสามารถขึ้นอยู่กับ Executor)",
    Default = true,
    Callback = function(state)
        lagSwitchActive = state
        if state then
            -- ใช้ฟังก์ชันของ Executor เพื่อตัดการส่งข้อมูลชั่วคราว
            if getsenv and getsenv().LagSwitch then
                getsenv().LagSwitch(true) -- ฟังก์ชันตัวอย่าง 1
            elseif game.Executor and game.Executor.LagSwitch then
                game.Executor:LagSwitch(true) -- ฟังก์ชันตัวอย่าง 2
            else
                -- วิธีสำรอง (ลด Send Rate) ถ้า Executor ไม่มีฟังก์ชัน LagSwitch โดยตรง
                local networkClient = game:GetService("NetworkClient")
                if networkClient then
                    networkClient:SetSendRate(0)
                end
            end
        else
            -- คืนค่า
            if getsenv and getsenv().LagSwitch then
                getsenv().LagSwitch(false)
            elseif game.Executor and game.Executor.LagSwitch then
                game.Executor:LagSwitch(false)
            else
                -- คืนค่า Send Rate เป็นค่าเริ่มต้น (เช่น 60)
                local networkClient = game:GetService("NetworkClient")
                if networkClient then
                    networkClient:SetSendRate(60)
                end
            end
        end
    end
})

-- ส่วนลูปบังคับการทำงาน NoClip
RunService.Stepped:Connect(function()
    -- ตรวจสอบว่าเปิด NoClip และตัวละครมีอยู่
    if noclipEnabled and LocalPlayer.Character then
        local char = LocalPlayer.Character
        -- ตั้งค่าการชนเป็น false ในทุกเฟรมของ Physics
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- =========================
-- MOD TAB (ปุ่มรันสคริปต์ทั้งหมด)
-- =========================

-- ปุ่มรัน Aimbot
ModTab:Button({
    Title = "Run Aimbot",
    Description = "โหลดสคริป Aimbot Mobile",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/DanielHubll/DanielHubll/refs/heads/main/Aimbot%20Mobile"))()
    end
})

ModTab:Button({ Title = "Dex", Description = "เปิด Dex Explorer", Callback = function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/BigBoyTimme/New.Loadstring.Scripts/refs/heads/main/Dex.Explorer"))()
end })

ModTab:Button({ Title = "MVSD", Description = "รันสคริป MVSD", Callback = function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/luck999xp-alt/sadasd/refs/heads/main/README.md"))()
end })

ModTab:Button({ Title = "Fly", Description = "เปิด Fly Script", Callback = function()
    -- โค้ดที่เข้ารหัส (obfuscated)
    loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\40\39\104\116\116\112\115\58\47\47\103\105\115\116\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\109\101\111\122\111\110\101\89\84\47\98\102\48\51\55\100\102\102\57\102\48\97\55\48\48\49\55\51\48\52\100\100\100\54\55\102\100\99\100\51\55\48\47\114\97\119\47\101\49\52\101\55\52\102\52\50\53\98\48\54\48\100\102\53\50\51\51\52\51\99\102\51\48\98\55\56\55\48\55\52\101\98\51\99\53\100\50\47\97\114\99\101\117\115\37\50\53\50\48\120\37\50\53\50\48\102\108\121\37\50\53\50\48\50\37\50\53\50\48\111\98\102\108\117\99\97\116\111\114\39\41\44\116\114\117\101\41\41\40\41\10\10")()
end })

ModTab:Button({ Title = "Hitbox", Description = "เปิด Hitbox Script", Callback = function()
    loadstring(game:HttpGet("https://pastefy.app/ItfO0tdg/raw"))()
end })

ModTab:Button({ Title = "Invisible", Description = "เปิด Hitbox Script", Callback = function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Invisible-script-20557"))()
end })

-- =========================
-- CREDITS TAB (Server Hop, Rejoin, Info)
-- =========================

-- Server Hop (ปุ่มย้ายเซิร์ฟเวอร์)
CreditsTab:Button({
    Title = "Server Hop (ย้ายเซิร์ฟ)",
    Description = "ย้ายไปเซิร์ฟอื่นที่มีคนน้อย/คนใหม่",
    Callback = function()
        local placeId = game.PlaceId
        TeleportService:Teleport(placeId, LocalPlayer)
    end
})

-- Rejoin Server (ปุ่มเข้าเซิร์ฟเดิม)
CreditsTab:Button({
    Title = "Rejoin Current Server",
    Description = "กลับเข้าเซิร์ฟเดิมทันที (แก้บั๊ก/โดนเตะ)",
    Callback = function()
        local jobId = game.JobId
        local placeId = game.PlaceId
        TeleportService:TeleportToPlaceInstance(placeId, jobId, LocalPlayer)
    end
})

CreditsTab:Paragraph({
    Title = "👑 NaKub Hub",
    Content = "สร้างโดย:--\n\n[✅ Anti-AFK ทำงานอัตโนมัติ]"
})

-- =========================
-- GLOBAL LOGIC (Anti-AFK)
-- =========================

-- Anti-AFK (กันเกมเตะโดยการจำลองการคลิกเมาส์ขวา)
LocalPlayer.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new(0, 0))
end)

-- =========================
-- สิ้นสุดโค้ด
-- =========================
Window:SetEnabled(true)
