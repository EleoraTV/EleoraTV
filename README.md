- 👋 Hi, I’m @EleoraTV
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
EleoraTV/EleoraTV is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

local re = game.ReplicatedStorage:WaitForChild("MessageSentRE")
 
local messagesFolder = Instance.new("Folder", game.ReplicatedStorage)
messagesFolder.Name = "MessageFolder"
 
 
local cooldown = 2
local onCooldown = {}
 
local charLimit = 200
 
 
 
re.OnServerEvent:Connect(function(plr, msg)
    
    
    if #msg > charLimit or #string.gsub(msg, " ", "") == 0 then return end
    
    if onCooldown[plr] then return end
    
    onCooldown[plr] = true
    
    
    local filtered
    
    pcall(function()
        
        filtered = game:GetService("TextService"):FilterStringAsync(msg, plr.UserId)
        
        filtered = filtered:GetNonChatStringForBroadcastAsync()
    end)
    
    
    if not filtered then return end
    
    
    local fullMessage = "[" .. plr.Name .. "]: " .. filtered
    
    
    local str = Instance.new("StringValue")
    str.Value = fullMessage
    str.Parent = messagesFolder
    
    
    game:GetService("Debris"):AddItem(str, 10)
    
    
    wait(cooldown)
    onCooldown[plr] = nil
end)
