# Stalkie Reanimation Module

A self-contained Roblox module for networkless (netless) reanimation, utilizing a vulnerability found in most ragdoll systems.

---

## ⚠️ WARNING

This module allows the use of custom keyframe animations that are **VISIBLE TO ALL PLAYERS** in the game.

**DO NOT use this for:**
- Sexual, obscene, or adult content of any kind
- Harassment, bullying, or targeting other players
- Content that depicts violence against real individuals or groups

**You are solely responsible for any animations you use.** 
- The developers of this module are **NOT responsible** for your actions or any consequences thereof
- You may face **legal consequences** for using this module inappropriately 
- By using this module, you agree to use it **responsibly and legally**

---

## Supported Games

The module automatically detects and configures ragdoll remotes for the following games:

- **[Mic Up](https://www.roblox.com/games/6884319169)** (PlaceId: 6884319169)
- **[Mic Up 18+](https://www.roblox.com/games/15546218972)** (PlaceId: 15546218972)
- **[Spray Paint](https://www.roblox.com/games/5991163185)** (PlaceId: 5991163185)
- **[Ragdoll Engine](https://www.roblox.com/games/5683833663)** (PlaceId: 5683833663)


## Table of Contents

-   [API Reference](#api-reference)
    -   [`API.reanimate(enable, remote, args)`](#apireanimateenable-remote-args)
    -   [`API.play_animation(url, speed)`](#apiplay_animationurl-speed)
    -   [`API.stop_animation()`](#apistop_animation)
    -   [`API.set_animation_speed(speed)`](#apiset_animation_speedspeed)
    -   [`API.on_animation_play(callback)`](#apion_animation_playcallback)
    -   [`API.on_animation_stop(callback)`](#apion_animation_stopcallback)
    -   [`API.is_animation_playing()`](#apiis_animation_playing)
    -   [`API.is_reanimated()`](#apiis_reanimated)
    -   [`API.get_clone()`](#apiget_clone)
    -   [`API.get_real_character()`](#apiget_real_character)
-   [Full Example](#full-example)

## API Reference

### `API.reanimate(enable, remote, args)`

Toggles the reanimation state for the local player.

-   **`enable`** (boolean):
    -   `true`: Clones the character and enables the reanimation.
    -   `false`: Destroys the clone, restores the original character, and disables the reanimation.
-   **`remote`** (Instance) `[optional]`: A `RemoteEvent` or `RemoteFunction` to be fired/invoked immediately after the state changes. If not provided, the module will automatically detect and use the correct remote for supported games.
-   **`args`** (table) `[optional]`: A table of arguments to be passed to the remote when it is fired.

```lua
-- For supported games (Mic Up, Spray Paint, Ragdoll Engine), simply call:
api.reanimate(true)  -- Enable
api.reanimate(false) -- Disable

-- For other games, manually specify the remote:
local ragdoll = game:GetService("ReplicatedStorage").RagdollEvent
api.reanimate(true, ragdoll, {"SomeArgument"})

local unragdoll = game:GetService("ReplicatedStorage").UnragdollEvent
api.reanimate(false, unragdoll)
```

---

### `API.play_animation(url, speed)`

Plays a custom animation on the real (visible) character model. Only works while reanimation is active.

-   **`url`** (string): The raw URL to a script that returns a keyframe table (e.g., from `ichfickdeinemutta.pages.dev`).
-   **`speed`** (number) `[optional]`: The playback speed multiplier. Defaults to `1.0`.

**Note:** Calling this function with the same URL of a currently playing animation will stop it.

```lua
-- Play an animation at normal speed
api.play_animation("https://example.com")

-- Play another animation at double speed
task.wait(5)
api.play_animation("https://example.com", 2)
```

---

### `API.stop_animation()`

Stops any custom animation that is currently playing. The character will return to its default pose.

```lua
api.stop_animation()
```

---

### `API.set_animation_speed(speed)`

Sets the playback speed for any currently playing animation.

-   **`speed`** (number): The new playback speed multiplier.

```lua
-- Speed up the current animation to 2x speed
api.set_animation_speed(2.0)

-- Slow down to half speed
api.set_animation_speed(0.5)
```

---

### `API.on_animation_play(callback)`

Registers a callback function to be called when an animation starts playing.

-   **`callback`** (function): The function to call. It receives the animation URL as an argument.

```lua
api.on_animation_play(function(url)
    print("Started playing animation:", url)
end)
```

---

### `API.on_animation_stop(callback)`

Registers a callback function to be called when an animation stops.

-   **`callback`** (function): The function to call. It receives the animation URL that was stopped.

```lua
api.on_animation_stop(function(url)
    print("Stopped animation:", url)
end)
```

---

### `API.is_animation_playing()`

Returns the current animation playback state.

-   **Returns**: (boolean, string | nil) - `is_playing`, `current_url`

```lua
local is_playing, current_url = api.is_animation_playing()
if is_playing then
    print("Currently playing:", current_url)
end
```

---

### `API.is_reanimated()`

Returns whether the reanimation module is currently active.

-   **Returns**: (boolean) - `true` if reanimated, `false` otherwise.

```lua
if api.is_reanimated() then
    print("The player is currently reanimated.")
end
```

---

### `API.get_clone()`

Retrieves the active clone `Model` that is being controlled. The clone is invisible and handles physics/controls.

-   **Returns**: (Model | nil) - The clone character model, or `nil` if not currently reanimated.

---

### `API.get_real_character()`

Retrieves the original, visible character `Model` that is being synchronized to follow the clone.

-   **Returns**: (Model | nil) - The real character model, or `nil` if not currently reanimated.

```lua
local real_character = api.get_real_character()
if real_character then
    -- You could, for example, change the color of the real character's parts
    real_character.Head.BrickColor = BrickColor.Red()
end
```

## Full Example

### For Supported Games (Mic Up, Spray Paint, Ragdoll Engine)

```lua
-- Load the API
local api = loadstring(game:HttpGet("https://raw.githubusercontent.com/Rootleak/Reanimation/refs/heads/main/module.lua"))()

-- Enable reanimation (automatically detects game and uses correct remote)
api.reanimate(true)

if api.is_reanimated() then
    -- Play Custom Animation (In this case its Billy Bounce from Fortnite)
    local animation_url = "https://raw.githubusercontent.com/Rootleak/Animations/main/Billy%20Bounce%2Elua"
    api.play_animation(animation_url, 1.5) -- Play at 1.5x speed
end

-- After 10 seconds, stop the animation and disable reanimation
task.wait(10)
api.stop_animation()
api.reanimate(false)
```

### For Other Games

```lua
-- Load the API
local api = loadstring(game:HttpGet("https://raw.githubusercontent.com/Rootleak/Reanimation/refs/heads/main/module.lua"))()

-- Find the ragdoll remote for your specific game (use a remote spy/explorer)
-- Example paths you might find:
-- game:GetService("ReplicatedStorage").RagdollEvent
-- game:GetService("ReplicatedStorage").Remotes.Ragdoll
local replicated_storage = game:GetService("ReplicatedStorage")
local ragdoll_remote = replicated_storage:WaitForChild("YourRagdollRemoteName")

-- Enable reanimation and fire the ragdoll remote
api.reanimate(true, ragdoll_remote) -- Add 3rd argument {args} if the remote needs them

if api.is_reanimated() then
    -- Play Custom Animation (In this case its Billy Bounce from Fortnite)
    local animation_url = "https://raw.githubusercontent.com/Rootleak/Animations/main/Billy%20Bounce%2Elua"
    api.play_animation(animation_url, 1.5) -- Play at 1.5x speed
end

-- After 10 seconds, stop the animation and disable reanimation
task.wait(10)
api.stop_animation()
api.reanimate(false, ragdoll_remote) -- Some games use same remote for disable
```
