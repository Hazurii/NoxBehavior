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

### Classification:
Nox splits all scripts into three classes:
1. **Handlers** - ModuleScripts ran by Nox to act as traditional Roblox scripts. They are the backbone to making your project run.
2. **Components -** Object-Oriented classes that can be constructed in Handlers via `self:Create()`.
3. **Libraries -** Ordinary ModuleScripts (typically used for data storage and utility).

### Getting Services:
All handlers must return a table like so:
```lua
local NoxBehavior = {
	Using = {},
	Signals = {
		OneWay = {},
		TwoWay = {}
	},
}
```
The `Using` table will allow you to access Roblox services without the need of `game:GetService(serviceName)`. To do this, you need to create an array of strings (names of the services you want to get).
```lua
local NoxBehavior = {
	Using = {'RunService', 'TweenService'},
	Signals = {
		OneWay = {},
		TwoWay = {}
	},
}
```
In this example, we can now use both `RunService` and `TweenService`.
### Core Functions:
In order for your handler to run properly, you need to make use of NoxBehavior's core functions. NoxBehavior's core functions are `:Init()`, `:Start()`, `:Close()`, `:Update()`, and `:FixedUpdate()`.

`:Init()` is ran before all else, thus some components, libraries, and other handlers may not be properly loaded yet. There is no guarantee that you will be able to use another handler's function so keep that in mind while you are integrating NoxBehavior into your game.

```lua
function NoxBehavior:Init()
  print(self:GetHandler('OtherHandler')) -- returns nil because OtherHandler has not loaded
end
```

`:Start()` runs right after `:Init()`, firing when everything has finished loading. You will be able to access all assets just fine in this function.

```lua
function NoxBehavior:Start()
  print(self:GetHandler('OtherHandler')) -- returns OtherHandler
end
```
`:Close()` as you might have guessed, runs when the server has closed. It is the equivalent to `game:BindToClose()`.
```lua
function NoxBehavior:Close()
  task.wait(3) -- server waits 3 seconds when closing
end
```
`:Update()` runs every single frame (typically 60 times a second although factors such as lag may change this). It is the equivalent to `game:GetService('RunService').Heartbeat`.
```lua
function NoxBehavior:Update()
  print('Hazurii is so cool') -- this string will be printed in output every single frame
end
```
`:FixedUpdate()` functions similarly to `:Update()` with the only deviation being that it runs before physics. On the client, it can altneratively be ran every frame on a `FixedPriority`. This function is equivalent to `game:GetService('RunService').Stepped` on the server or `game:GetService('RunService'):BindToRenderStepped()` on the client.
```lua
local NoxBehavior = {
	Using = {},
	Signals = {
		OneWay = {},
		TwoWay = {}
	},
  FixedPriority = Enum.RenderPriority.First
}

function NoxBehavior:FixedUpdate()
  print('Before physics!') -- since the function is prioritized to run first, this string will run at the start of every frame
end
```
### Network Communication:
NoxBehavior uses `Signals` to achieve cross-network communication. `Signals` can be created within the NoxBehavior table by creating an array of strings (each string will be the name of a newly created `Signal`). One way `Signals` do not return any data when called upon while two way `Signals` do. Here is an example of creating signals:
```lua 
local NoxBehavior = {
	Using = {},
	Signals = {
		OneWay = {'Greet', 'Wave'},
		TwoWay = {'Converse'}
	},
}
```
NoxBehavior would create one way `Signals` called "Greet" and "Wave" alongside a two way signal called "Converse". These `Signals` can now be called in a core function like `:Start()` using `:Call()` within said functions. To handle a signal, you can attach a function to them via `:Connnect()`. Assuming that you want to communicate from the server to the client, you can set up your handlers like so:

**Server:**
```lua 
local NoxBehavior = {
	Using = {'Players'},
	Signals = {
		OneWay = {'Greet', 'Wave'},
		TwoWay = {'Converse'}
	},
}

function NoxBehavior:Start()
  local bob = self.Players:WaitForChild('Bob')
  
  self:Call('Greet', bob, 'hello') 
  
  local reply = self:Call('Converse', bob, 'how's your day?') 
  print(reply) 
end

return NoxBehavior
```
**Client:**
```lua 
local NoxBehavior = {
	Using = {},
	Signals = {
		OneWay = {},
		TwoWay = {}
	},
}

function NoxBehavior:Start()
  self:Connect('Greet', function(greeting))
    print('The server said ' ..greeting.. ' to me')
  end)
  
  self:Connect('Converse', function(question))
    print('The server asked me ' ..question)
    return 'I don't want to talk to you!'
  end)
end

return NoxBehavior
```
**Output:**
```lua 
-- [CLIENT] The server said hello to me
-- [CLIENT] The server asked me how's your day
-- [SERVER] I don't want to talk to you!
```
## API:
NoxBehavior boasts numerous useful functions to help you on your development journey!

