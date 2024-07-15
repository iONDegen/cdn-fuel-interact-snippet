# Credit where Credit is Due

# Thanks to @BubbleDude aka bruhbubble. Without his preview in the project-sloth discord, This wouldn't have been done. 
# Thanks to the CodineDev team for creating such an immersive & feature packed fuel script.
# Thanks to the Project Sloth Community. Without the support on my first release this wouldn't be possible.

# Snippet

1. Go to client/fuel_cl.lua & Navigate until you find this

```
CreateThread(function()
	local bones = {
		"petroltank",
		"petroltank_l",
		"petroltank_r",
		"wheel_rf",
		"wheel_rr",
		"petrolcap ",
		"seat_dside_r",
		"engine",
	}

	if Config.TargetResource == 'ox_target' then
		local options = {
			[1] = {
				name = 'cdn-fuel:options:1',
				icon = "fas fa-gas-pump",
				label = tostring(Lang:t("input_insert_nozzle")),
				canInteract = function()
					if inGasStation and not refueling and holdingnozzle then
						return true
					end
				end,
				event = 'cdn-fuel:client:RefuelMenu'
			},
			[2] = {
				name = 'cdn-fuel:options:2',
				icon = "fas fa-bolt",
				label = tostring(Lang:t("insert_electric_nozzle")),
				canInteract = function()
					if Config.ElectricVehicleCharging == true then
						if inGasStation and not refueling and IsHoldingElectricNozzle() then
							return true
						else
							return false
						end
					else
						return false
					end
				end,
				event = "cdn-fuel:client:electric:RefuelMenu",
			}
		}

		exports.ox_target:addGlobalVehicle(options)

		local modelOptions = {
			[1] = {
				name = "cdn-fuel:modelOptions:option_1",
				num = 1,
				type = "client",
				event = "cdn-fuel:client:grabnozzle",
				icon = "fas fa-gas-pump",
				label = Lang:t("grab_nozzle"),
				canInteract = function()
					if PlayerInSpecialFuelZone then return false end
					if not IsPedInAnyVehicle(PlayerPedId()) and not holdingnozzle and not HoldingSpecialNozzle and inGasStation == true and not PlayerInSpecialFuelZone then
						return true
					end
				end,
			},
			[2] = {
				name = "cdn-fuel:modelOptions:option_2",
				num = 2,
				type = "client",
				event = "cdn-fuel:client:purchasejerrycan",
				icon = "fas fa-fire-flame-simple",
				label = Lang:t("buy_jerrycan"),
				canInteract = function()
					if not IsPedInAnyVehicle(PlayerPedId()) and not holdingnozzle and not HoldingSpecialNozzle and inGasStation == true then
						return true
					end
				end,
			},
			[3] = {
				name = "cdn-fuel:modelOptions:option_3",
				num = 3,
				type = "client",
				event = "cdn-fuel:client:returnnozzle",
				icon = "fas fa-hand",
				label = Lang:t("return_nozzle"),
				canInteract = function()
					if holdingnozzle and not refueling then
						return true
					end
				end,
			},
			[4] = {
				name = "cdn-fuel:modelOptions:option_4",
				num = 4,
				type = "client",
				event = "cdn-fuel:client:grabnozzle:special",
				icon = "fas fa-gas-pump",
				label = Lang:t("grab_special_nozzle"),
				canInteract = function()
					if Config.FuelDebug then print("Is Player In Special Fuel Zone?: "..tostring(PlayerInSpecialFuelZone)) end
					if not HoldingSpecialNozzle and not IsPedInAnyVehicle(PlayerPedId()) and PlayerInSpecialFuelZone then
						return true
					end
				end,
			},
			[5] = {
				name = "cdn-fuel:modelOptions:option_5",
				num = 5,
				type = "client",
				event = "cdn-fuel:client:returnnozzle:special",
				icon = "fas fa-hand",
				label = Lang:t("return_special_nozzle"),
				canInteract = function()
					if HoldingSpecialNozzle and not IsPedInAnyVehicle(PlayerPedId()) then
						return true
					end
				end
			},
		}

		exports.ox_target:addModel(props, modelOptions)
	else
		exports[Config.TargetResource]:AddTargetBone(bones, {
			options = {
				{
					type = "client",
					action = function ()
						TriggerEvent('cdn-fuel:client:RefuelMenu')
					end,
					icon = "fas fa-gas-pump",
					label = Lang:t("input_insert_nozzle"),
					canInteract = function()
						if inGasStation and not refueling and holdingnozzle then
							return true
						end
					end
				},
				{
					type = "client",
					action = function()
						TriggerEvent('cdn-fuel:client:electric:RefuelMenu')
					end,
					icon = "fas fa-bolt",
					label = Lang:t("insert_electric_nozzle"),
					canInteract = function()
						if Config.ElectricVehicleCharging == true then
							if inGasStation and not refueling and IsHoldingElectricNozzle() then
								return true
							else
								return false
							end
						else
							return false
						end
					end
				},
			},
			distance = 1.5,
		})

		exports[Config.TargetResource]:AddTargetModel(props, {
			options = {
				{
					num = 1,
					type = "client",
					event = "cdn-fuel:client:grabnozzle",
					icon = "fas fa-gas-pump",
					label = Lang:t("grab_nozzle"),
					canInteract = function()
						if PlayerInSpecialFuelZone then return false end
						if not IsPedInAnyVehicle(PlayerPedId()) and not holdingnozzle and not HoldingSpecialNozzle and inGasStation == true and not PlayerInSpecialFuelZone then
							return true
						end
					end,
				},
				{
					num = 2,
					type = "client",
					event = "cdn-fuel:client:purchasejerrycan",
					icon = "fas fa-fire-flame-simple",
					label = Lang:t("buy_jerrycan"),
					canInteract = function()
						if not IsPedInAnyVehicle(PlayerPedId()) and not holdingnozzle and not HoldingSpecialNozzle and inGasStation == true then
							return true
						end
					end,
				},
				{
					num = 3,
					type = "client",
					event = "cdn-fuel:client:returnnozzle",
					icon = "fas fa-hand",
					label = Lang:t("return_nozzle"),
					canInteract = function()
						if holdingnozzle and not refueling then
							return true
						end
					end,
				},
				{
					num = 4,
					type = "client",
					event = "cdn-fuel:client:grabnozzle:special",
					icon = "fas fa-gas-pump",
					label = Lang:t("grab_special_nozzle"),
					canInteract = function()
						if Config.FuelDebug then print("Is Player In Special Fuel Zone?: "..tostring(PlayerInSpecialFuelZone)) end
						if not HoldingSpecialNozzle and not IsPedInAnyVehicle(PlayerPedId()) and PlayerInSpecialFuelZone then
							return true
						end
					end,
				},
				{
					num = 5,
					type = "client",
					event = "cdn-fuel:client:returnnozzle:special",
					icon = "fas fa-hand",
					label = Lang:t("return_special_nozzle"),
					canInteract = function()
						if HoldingSpecialNozzle and not IsPedInAnyVehicle(PlayerPedId()) then
							return true
						end
					end
				},
			},
			distance = 2.0
		})
	end
end)
```

