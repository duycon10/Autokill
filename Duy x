Eto 15+ above specific server 5 death detecting mag hohop

local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")

local player = Players.LocalPlayer
local deathCount = 0
local gameId = game.PlaceId

local function hopServer()
    local servers = {}
    local cursor = ""

    repeat
        local url = "https://games.roblox.com/v1/games/"..gameId.."/servers/Public?sortOrder=Asc&limit=100"..(cursor ~= "" and "&cursor="..cursor or "")
        local data = HttpService:JSONDecode(game:HttpGet(url))
        if data and data.data then
            for _, server in ipairs(data.data) do
                if server.playing >= 15 and server.playing < server.maxPlayers then
                    table.insert(servers, server)
                end
            end
            cursor = data.nextPageCursor or ""
        end
    until cursor == "" or #servers > 0

    if #servers > 0 then
        local chosen = servers[math.random(1, #servers)]
        TeleportService:TeleportToPlaceInstance(gameId, chosen.id, player)
    else
        warn("No servers with 15+ players found, retrying...")
    end
end

player.CharacterAdded:Connect(function(char)
    local humanoid = char:WaitForChild("Humanoid")
    humanoid.Died:Connect(function()
        deathCount += 1
        print("You died "..deathCount.." times.")
        if deathCount >= 5 then
            hopServer()
        end
    end)
end)
