-- 1. อ้างถึง Player ก่อน เพราะ Player Object จะไม่ถูกทำลายเมื่อ Respawn
local Player = game.Players.LocalPlayer

-- ฟังก์ชันหลักที่จะทำการล็อคค่า Cooldown 
local function LockCooldownLoop()
    -- **สำคัญ:** ดึง Backpack ใหม่ทุกครั้งที่ Character ถูกสร้างขึ้น
    local Backpack = Player:WaitForChild("Backpack") 
    
    -- ลูปตลอดกาล (Infinity Loop)
    while true do
        local AllItems = Backpack:GetChildren() 
        
        for _, Item in pairs(AllItems) do
            -- ตรวจสอบว่า Item นั้นมี Attribute ชื่อ "Cooldown" หรือไม่
            if Item:GetAttribute("Cooldown") ~= nil then
                -- ใช้ฟังก์ชัน SetAttribute เพื่อกำหนดค่าใหม่เป็น 0.1
                Item:SetAttribute("Cooldown", 0.1)
            end
        end

        -- หน่วงเวลา 
        wait(0.05) 
    end
end

-- 2. รันฟังก์ชันเมื่อ Character ถูกเพิ่ม (เกิดครั้งแรก)
Player.CharacterAdded:Connect(LockCooldownLoop)

-- 3. รันฟังก์ชันครั้งแรกทันที (เผื่อกรณีที่ Character ถูกโหลดก่อน Script นี้รัน)
if Player.Character then
    LockCooldownLoop()
end
