# Acknowledgements

## Overextended
I would like to fully acknowledge and give full credit the Overextended team for creating this amazing inventory resource for Five M. This code is not mine, but I re-added QBCore support back into the resource.
This aforementioned code was originally written by the Overextended team as well, and all credit goes to them for originally supporting QBCore up until recently.

## Community Ox
I would like to acknowledge and thank the team at Community Ox for continuing on with Overextended's legacy and maintaining this resource.

# Warning
Please Note That Neither Overextended nor Community Ox Will Offer Support For This Version Of Ox Inventory Due To Both Of Them Dropping QBCore Support In Its Entirety.

# QB-Core Server Installation
In order to successfully install ox_inventory into your QB-Core server, please complete the following installation steps.

## Server CFG
In your server.cfg file, make sure that you have ox_inventory and its dependencies ensured correctly. The ensure order should look like the following:
- ```ensure oxmysql```
- ```ensure ox_lib```
- ```ensure qb-core```
- ```ensure ox_target```
- ```ensure ox_inventory```

If you have a config.cfg file included with either ox_target or ox_inventory, the ensure order will look like the following:
- ```ensure oxmysql```
- ```ensure ox_lib```
- ```ensure qb-core```
- ```exec @ox_target/config.cfg```
- ```ensure ox_target```
- ```exec @ox_inventory/config.cfg```
- ```ensure ox_inventory```

## Server Database
Run the provided SQL file in your server's database.

## QB-Core Code Adjustments
Some code changes are required in order for ox_inventory to run properly in your QB-Core server.

In ```qb-core/server/events.lua```, locate the following event handler on line 21:
```lua
AddEventHandler("onResourceStop", function(resName)
    for i,v in pairs(QBCore.UsableItems) do
        if v.resource == resName then
            QBCore.UsableItems[i] = nil
        end
    end
end)
```
Either comment out or delete this snippet of code.

In ```qb-core/server/functions.lua```, locate the following function beginning on line 468:
```lua
---Create a usable item
---@param item string
---@param data function
function QBCore.Functions.CreateUseableItem(item, data)
    local rawFunc = nil

    if type(data) == 'table' then
        if rawget(data, '__cfx_functionReference') then
            rawFunc = data
        elseif data.cb and rawget(data.cb, '__cfx_functionReference') then
            rawFunc = data.cb
        elseif data.callback and rawget(data.callback, '__cfx_functionReference') then
            rawFunc = data.callback
        end
    elseif type(data) == 'function' then
        rawFunc = data
    end

    if rawFunc then
        QBCore.UsableItems[item] = {
            func = rawFunc,
            resource = GetInvokingResource()
        }
    end
end
```
Delete this snippet of code, and replace it with the following:
```lua
---Create a usable item
---@param item string
---@param data function
function QBCore.Functions.CreateUseableItem(item, data)
    QBCore.UsableItems[item] = data
end
```

## [qb] Resource Folder
Delete qb-inventory, qb-shops, and qb-weapons from the [qb] resource folder

## Server Console
After restarting your server, run the following command in your server's console terminal: ```convertinventory qb```.

This will convert all current and new player inventories, stashes, and etc. to ox inventory's formatting.

## Resource Configuration
Setup of ox_inventory is handled using convars. They are all included in the provided config.cfg file.

## Further Resources
1. Community Ox Documentation: https://coxdocs.dev/ox_inventory
2. YouTube video made by Kamaryn: https://youtu.be/Xkcs50nQ-q0?si=EnErUPhleDNl-YAS

## Credits
I would like to acknowledge and thank the team at Overextended for creating this inventory system for Five M.

I would like to acknowledge and thank the team at Community Ox for continuing on with Overextended's legacy and maintaining this resource.

I would like to acknowledge and thank Kamaryn for her YouTube video showcasing how to install ox_inventory into a QB-Core server.