-- 1. อ้างถึง Player ก่อน เพราะ Player Object จะไม่ถูกทำลายเมื่อ Respawn
local Player = game.Players.LocalPlayer
local StarterGui = game:GetService("StarterGui") -- ดึง Service สำหรับจัดการ UI

-- ฟังก์ชันหลักที่จะทำการล็อคค่า Cooldown 
local function LockCooldownLoop()
    -- **สำคัญ:** ดึง Backpack ใหม่ทุกครั้งที่ Character ถูกสร้างขึ้น
    local Backpack = Player:WaitForChild("Backpack") 
    
    -- *** เพิ่มการแจ้งเตือนเมื่อฟังก์ชันเริ่มทำงาน ***
    StarterGui:SetCore("SendNotification", {
        Title = "⚡ Exploiter Script",
        Text = "Cooldown Lock: ACTIVE (0.1s)",
        Duration = 5, -- แสดง 5 วินาที
        Button1 = "OK",
    })
    
    -- รันเสียง (ถ้ามี)
    -- local SoundService = game:GetService("SoundService")
    -- SoundService:PlayLocalSound("rbxassetid://106318991") -- ตัวอย่าง SoundID
    print("Script Status: Cooldown Lock Loop Started.")

    -- ลูปตลอดกาล (Infinity Loop)
    while true do
        local AllItems = Backpack:GetChildren() 
        
        for _, Item in pairs(AllItems) do
            if Item:GetAttribute("Cooldown") ~= nil then
                Item:SetAttribute("Cooldown", 0.1)
            end
        end

        wait(0.05) 
    end
end

-- 2. รันฟังก์ชันเมื่อ Character ถูกเพิ่ม (เกิดครั้งแรก)
Player.CharacterAdded:Connect(LockCooldownLoop)

-- 3. รันฟังก์ชันครั้งแรกทันที (เผื่อกรณีที่ Character ถูกโหลดก่อน Script นี้รัน)
if Player.Character then
    LockCooldownLoop()
end
