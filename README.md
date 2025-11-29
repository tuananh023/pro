-- SCRIPT G?P - KHÔNG CH?NH S?A GÌ

-- Ð?i game t?i
wait(5)

-- D?ch v? và bi?n co b?n
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local HttpService = game:GetService("HttpService")
local LocalPlayer = Players.LocalPlayer

-- Ch? các Module c?n thi?t
local ClientData = ReplicatedStorage:WaitForChild("ClientModules"):WaitForChild("Core"):WaitForChild("ClientData")

-- Script 2: Auto farm + webhook
task.spawn(function()
    setfpscap(3)
    script_key = "zCOUXXiuwgdHOkmQuhwzjcPnPnFLiHtt"
    getgenv().Config = {
        ["PetFarmAutoSwitchFullGrown"] = false,
        ["PetFarmActive"] = false,
        ["EggFarmActive"] = false,
        ["LitePetFarmActive"] = true,
        ["Blur_username"] = false,
        ["Blazing_Lion_Log"] = true,
        ["DiscordId"] = "710726834240618526",
        ["Webhook"] = "https://discord.com/api/webhooks/1286202963353931817/AeMMxeR_NKH9-PtdZx9bUjF3LfyyEw0fK4FR9c-Eg3It6Bay75oUVs-5n2Vnzx8j9A7y",
    }
    loadstring(game:HttpGet("https://api.luarmor.net/files/v3/loaders/66567bfd337b57eb059b58dbe1badb89.lua"))()
end)

-- Hàm l?y s? potion
function GetPotions()
    local Foods = {}
    for l,o in pairs(require(ClientData).get_data()[LocalPlayer.Name].inventory.food) do
        if o.id == "pet_age_potion" then
            table.insert(Foods, l)
        end
    end
    return #Foods
end

-- Hàm l?y ti?n s? ki?n
function GetEventCurrency()
    return require(ClientData).get_data()[LocalPlayer.Name][require(ReplicatedStorage.SharedModules.SharedDB.AltCurrencyData)["name"]] or 0
end

-- Hàm l?y bucks (kèm pcall d? tránh l?i)
function GetBucks()
    local success, result = pcall(function()
        return require(ClientData).get_data()[LocalPlayer.Name].money
    end)
    if success and result ~= nil then
        return result
    else
        return nil
    end
end

-- Hàm l?y t?ng stats
local function GetStats()
    if not game:IsLoaded() then
        return ""
    end
    local currentData = {
        potions = GetPotions(),
        event_currency = GetEventCurrency(),
        bucks = GetBucks(),
    }
    return currentData
end

-- Script 1: Ki?m tra d? di?u ki?n ghi file
function CheckAndWrite()
    while true do
        local potions = GetPotions()
        local bucks = GetBucks()

        if potions >= 100 and bucks and bucks >= 60000 then
            writefile(LocalPlayer.Name..".txt", "Completed-AccHoanThanh-Potion+Money")
        end

        task.wait(5)
    end
end

task.spawn(CheckAndWrite)

-- Script 3: Auto load heart farm sau 90s
task.spawn(function()
    task.wait(90)
    loadstring(game:HttpGet("https://raw.githubusercontent.com/diwserenityhub/other/refs/heads/main/heart_farm.lua"))()
end)

-- Script 4: Ki?m tra UI load l?i sau 60s thì shutdown
task.spawn(function()
    wait(60)
    if game:GetService("Players").LocalPlayer.PlayerGui.AssetLoadUI.Enabled == true then
        game:Shutdown()
    end
end)

-- Script 5: Kick n?u không tang bucks
task.spawn(function()
    wait(30)
    local oldBucks = GetBucks()
    if oldBucks == nil then
        LocalPlayer:Kick("Can't read bucks value.")
        return
    end

    while true do
        task.wait(300)
        local newBucks = GetBucks()
        if newBucks == nil then
            LocalPlayer:Kick("Can't read bucks value.")
            break
        end
        if newBucks <= oldBucks then
            LocalPlayer:Kick("No bucks increase detected.")
            break
        end
        oldBucks = newBucks
    end
end)
