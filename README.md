# NoxBehavior
A nifty Roblox game framework inspired by Unity's MonoBehavior, designed to make life just a tad bit easier. Instead of wasting your time organizing your scripts and assets. Use NoxBehavior right now with just a few clicks!

## How can I install it?
NoxBehavior can be installed from the Roblox Creator Marketplace [here](https://pages.github.com/). 

The steps to properly integrate it into your project is as follows:

1. After getting the model, head to Roblox Studio and open your game. Go to **Toolbox > Inventory > My Models**, and click on "NoxBehavior" which should be at the top.
2. Drag the "Server" folder into **game > ServerStorage** and the "Client" folder into **game > ReplicatedStorage**.
3. Drag the "NoxBehavior" server script into **game > ServerScriptService** and its local script counterpart into **game > StarterPlayer > StarterPlayerScripts**.
4. Delete the folder the items were in (alongside the "README" script) and you should be done!

## Why should I use it?
NoxBehavior favors project consistency and organization. It is fairly optimized, spanning less than 400 lines of code while making development a piece of cake. In addition, NoxBehavior allows effortless network communication without the need of **RemoteEvents** and **RemoteFunctions**. You can also swiftly communicate between other scripts (Server-to-Server or Client-to-Client) without the need of **BindableEvents** or **BindableFunctions**. NoxBehavior also guarantees that one simple error in your script will not break your entire game. 

## How do I use it?
NoxBehavior attempts to simplify and organize development, but using NoxBehavior without understanding it can make these attempts futile. In this tutorial, we will cover how to use NoxBehavior effectively. In addition, "NoxBehavior" will be shortned to "Nox" to save time. Enjoy!

### Classifications:
Nox splits all scripts into three classes:
1. **Handlers** - ModuleScripts ran by Nox to act as traditional Roblox scripts. They are the backbone to making your project run.
2. **Components -** Object-Oriented classes that can be constructed in Handlers via `self:Create()`.
3. **Libraries -** Ordinary ModuleScripts (typically used for data storage and utility).
