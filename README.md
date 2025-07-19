local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local placeId = 15744125254 -- ID của game
local privateServerCode = "7ceba9b28e748b4db160fd559a9b5400"

repeat task.wait() until game:IsLoaded()

-- Kiểm tra xem có đang ở server riêng không
local isInPrivate = game.PrivateServerId ~= "" and game.PrivateServerOwnerId ~= 0

if not isInPrivate then
    -- Nếu không phải server riêng, teleport vào server riêng
    TeleportService:TeleportToPrivateServer(placeId, privateServerCode, {player})
else
    -- Nếu đã ở server riêng, tạo màn hình đen vĩnh viễn
    local blackScreen = Instance.new("ScreenGui")
    blackScreen.Name = "BlackScreen"
    blackScreen.IgnoreGuiInset = true
    blackScreen.ResetOnSpawn = false
    blackScreen.ZIndexBehavior = Enum.ZIndexBehavior.Global
    blackScreen.Parent = game:GetService("CoreGui")

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.Position = UDim2.new(0, 0, 0, 0)
    frame.BackgroundColor3 = Color3.new(0, 0, 0)
    frame.BorderSizePixel = 0
    frame.Parent = blackScreen

    -- Đợi nhân vật tải xong
    repeat task.wait() until player.Character and player.Character:FindFirstChild("HumanoidRootPart")

    local character = player.Character
    local hrp = character:WaitForChild("HumanoidRootPart")

    -- Tìm NPC Seller trong Workspace
    local sellerNPC = Workspace:FindFirstChild("Seller") or Workspace:FindFirstChildWhichIsA("Model", true)

    if sellerNPC and sellerNPC:FindFirstChild("HumanoidRootPart") then
        hrp.CFrame = sellerNPC.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0) -- Dịch người chơi đến gần NPC
        print("Dịch chuyển đến NPC Seller thành công.")
    else
        warn("Không tìm thấy NPC Seller trong Workspace.")
    end
end
