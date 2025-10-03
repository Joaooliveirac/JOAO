local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Coloque seu UserId aqui
local Admins = {
    [7325357684] = true
}

-- Cria RemoteEvents automaticamente
local function getEvent(name)
    local e = ReplicatedStorage:FindFirstChild(name)
    if not e then
        e = Instance.new("RemoteEvent")
        e.Name = name
        e.Parent = ReplicatedStorage
    end
    return e
end

local KickEvent = getEvent("KickEvent")
local BanEvent = getEvent("BanEvent")
local TpEvent = getEvent("TpEvent")
local BringEvent = getEvent("BringEvent")

local function isAdmin(player)
    return Admins[player.UserId] == true
end

KickEvent.OnServerEvent:Connect(function(player, targetName)
    if isAdmin(player) then
        local target = Players:FindFirstChild(targetName)
        if target then
            target:Kick("Expulso por "..player.Name)
        end
    end
end)

BanEvent.OnServerEvent:Connect(function(player, targetName)
    if isAdmin(player) then
        local target = Players:FindFirstChild(targetName)
        if target then
            target:Kick("Banido por "..player.Name)
            Admins[target.UserId] = false
        end
    end
end)

TpEvent.OnServerEvent:Connect(function(player, targetName)
    if isAdmin(player) then
        local target = Players:FindFirstChild(targetName)
        if target and target.Character and player.Character and target.Character.PrimaryPart and player.Character.PrimaryPart then
            player.Character:MoveTo(target.Character.PrimaryPart.Position)
        end
    end
end)

BringEvent.OnServerEvent:Connect(function(player, targetName)
    if isAdmin(player) then
        local target = Players:FindFirstChild(targetName)
        if target and target.Character and player.Character and target.Character.PrimaryPart and player.Character.PrimaryPart then
            target.Character:MoveTo(player.Character.PrimaryPart.Position)
        end
    end
end)
