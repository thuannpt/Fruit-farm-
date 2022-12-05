--Be nice and leave credits in!! I worked hard on this, and I'm offering it for free. As compensation, all I ask for is that I am properly credited.
if not isfolder("Blox Fruits Fruit Farm") then
	makefolder("Blox Fruits Fruit Farm")
end
if not isfile("Blox Fruits Fruit Farm/Empty Servers.txt") then
    writefile("Blox Fruits Fruit Farm/Empty Servers.txt", "0")
end
if not isfile("Blox Fruits Fruit Farm/Servers With Fruits.txt") then
    writefile("Blox Fruits Fruit Farm/Servers With Fruits.txt", "0")
end
if not isfile("Blox Fruits Fruit Farm/Total Servers.txt") then
    writefile("Blox Fruits Fruit Farm/Total Servers.txt", "0")
end
if not isfile("Fruit Farm Logs.txt") then
    writefile("Fruit Farm Logs.txt", "Stored fruits, servers you've visited, and more will be logged here. Empty Servers: 0 | Servers With Fruits: 0 | Total Servers: 0 | \n")
end

warn("Check Fruit Farm Logs.txt in your executor's workspace folder to have an accurate log of what fruits youve stored.")
print(readfile("Fruit Farm Logs.txt"))

local Servers_Total = tonumber(readfile("Blox Fruits Fruit Farm/Total Servers.txt"))
local Servers_wFruit = tonumber(readfile("Blox Fruits Fruit Farm/Servers With Fruits.txt"))
local Servers_Empty = tonumber(readfile("Blox Fruits Fruit Farm/Empty Servers.txt"))
Servers_Total = Servers_Total + 1

if not game:IsLoaded() then
    game.Loaded:Wait()
end
repeat
    task.wait()
until game:GetService("Players") and game:GetService("Workspace") and game:GetService("ReplicatedStorage")
spawn(function()while task.wait(.2)do setfpscap(600)game.Workspace.StreamingEnabled=false game:GetService("RunService"):Set3dRenderingEnabled(false)end end)

local LocalPlayer = game:GetService("Players").LocalPlayer
loadstring(game:HttpGet("https://pastebin.com/raw/tUUGAeaH", true))()

local function returnHRP()
    if not LocalPlayer.Character then
        return
    end
    if not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        return
    else
        return LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    end
end
local function returnHUM()
    if not LocalPlayer.Character then
        return
    end
    if not LocalPlayer.Character:FindFirstChild("Humanoid") then
        return
    else
        return LocalPlayer.Character:FindFirstChild("Humanoid")
    end
end
repeat
    task.wait()
until returnHRP() and returnHUM()
local HrpTable = {
    Velocity = returnHRP().Velocity,
    Transparency = returnHRP().Transparency,
    Rotation = returnHRP().Rotation,
    Size = returnHRP().Size,
    Orientation = returnHRP().Orientation,
    Anchored = returnHRP().Anchored
}

local function spoofHRP()
    for i, v in pairs(HrpTable) do
        spoof(returnHRP(), tostring(i), returnHRP():GetAttribute(v))
    end

    return true
end

local function TpTo(CFrame, Refresh)
    if Refresh then
        returnHUM().Health = 0
        LocalPlayer.CharacterAdded:Wait()
        repeat
            task.wait()
        until returnHRP() and returnHUM()
        spoofHRP()
        spoofHUM()
    else
        spoofHRP()
    end

    returnHRP().CFrame = CFrame

    return true
end
local function ServerHop()
	writefile("Blox Fruits Fruit Farm/Empty Servers.txt", tostring(Servers_Empty))
	writefile("Blox Fruits Fruit Farm/Servers With Fruits.txt", tostring(Servers_wFruit))
	writefile("Blox Fruits Fruit Farm/Total Servers.txt", tostring(Servers_Total))
	
	Original_Txt = readfile("Fruit Farm Logs.txt")
	Original_Split = string.split(Original_Txt, " | ")
	New_String = "Stored fruits, servers you've visited, and more will be logged here. Empty Servers: "..tostring(Servers_Empty).." | Servers With Fruit:"..tostring(Servers_wFruit).." | Total Servers: "..tostring(Servers_Total).." | "..Original_Split[4]
	writefile("Fruit Farm Logs.txt", New_String)
	task.wait(.5)
	
	local module = loadstring(game:HttpGet"https://raw.githubusercontent.com/LeoKholYt/roblox/main/lk_serverhop.lua")()
    module:Teleport(game.PlaceId)
end

local Fruit_InServer = false
local Fruits_InServer = {}
local Fruit_InHand = nil

for _,v in ipairs(workspace:GetChildren()) do
	if v:IsA("Tool") then
		Fruit_InServer = true
		table.insert(Fruits_InServer, v)
	end
end

if Fruit_InServer then
	appendfile("Fruit Farm Logs.txt","\n\n"..tostring(#Fruits_InServer).." fruits detected in this server.	"..os.date("%I")..":"..os.date("%M").." "..os.date("%p"))
	Servers_wFruit = Servers_wFruit + 1
	
	repeat
		game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(table.unpack({    [1] = "SetTeam",    [2] = "Pirates",}))
		task.wait(.4)
	until LocalPlayer.Team == game:GetService("Teams")["Pirates"]
	appendfile("Fruit Farm Logs.txt","\nSuccesfully switched to pirate team.")
	
	for _,v in pairs(Fruits_InServer) do
		returnHRP().CFrame=v.Handle.CFrame
		task.wait(.1)
		Fruit_InHand = string.gsub(v.Name, " Fruit", "")
		Fruit_InHand = Fruit_InHand.."-"..Fruit_InHand
		for _=1,20 do spawn(function()task.wait()game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(table.unpack({    [1] = "StoreFruit",    [2] = Fruit_InHand,    [3] = returnHRP().Parent:FindFirstChildOfClass("Tool"),}))end) end
		task.wait(.1)
		
		appendfile("Fruit Farm Logs.txt","\nSuccesfully got the "..Fruit_InHand.." fruit. [".._.."/"..#Fruits_InServer.."]")
		ServerHop()
	end
	
	task.wait(.5)
	appendfile("Fruit Farm Logs.txt","\nGot every fruit in this server. Moving onto the next server. | Powered by Extorius Scripts")
	appendfile("Fruit Farm Logs.txt","\n------------------------------------------------------------")
else
	Servers_Empty = Servers_Empty + 1
	appendfile("Fruit Farm Logs.txt","\nThere are no fruits in this server. Moving on to the next one.	"..os.date("%I")..":"..os.date("%M").." "..os.date("%p"))
	ServerHop()
end
spawn(function()
        local ohString1 = "SetTeam"
        local ohString2 = "Pirates"
        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(ohString1, ohString2)
     end)

if Possible == true then
                     table.insert(AllIDs, ID)
                     wait()
                     pcall(function()
                         writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                         wait()
                         game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                     end)
                     wait(4)
                 end
