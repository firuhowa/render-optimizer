```markdown
# 🚀 RenderOptimizer

RenderOptimizer is a client-side Minecraft optimization mod designed to improve performance while keeping high Render Distances practical.

The main goal of RenderOptimizer is not simply to lower graphics settings.

Instead, RenderOptimizer tries to reduce unnecessary rendering workload while allowing the player to keep a high Render Distance whenever the hardware can handle it.

## ✨ Features

- 🎯 Automatic Render Distance Optimization
- 🌄 Far Terrain Optimization
- 💤 AFK Chunk Limit
- 🌊 Smooth Render Distance Mode
- 📊 FPS Display
- 💾 Persistent Configuration
- ⚙️ Multiple optimization modes
- 🔧 Client-side optimization through Fabric Mixins

---

## 🎯 Automatic Render Distance Optimizer

The main feature of RenderOptimizer is an automatic Render Distance controller.

Instead of forcing the player to use a fixed Render Distance, RenderOptimizer continuously observes the current FPS and adjusts the Render Distance when necessary.

For example:

```text
Render Distance: 32 chunks
FPS: 25

        ↓

Performance is too low

        ↓

Render Distance is reduced

32 → 27 → 22 → 17 → ...

        ↓

FPS improves

        ↓

Render Distance gradually recovers

When the game has enough performance, RenderOptimizer tries to preserve the player's preferred Render Distance.

🧠 How the Automatic Optimizer Works

RenderOptimizer uses a tick-based optimization system.

The optimizer does not change the Render Distance every single tick.

Instead, it periodically checks the current FPS and evaluates whether the current Render Distance is appropriate.

The general process is:

Minecraft Client Tick
        ↓
Check whether the optimizer is enabled
        ↓
Check whether the player is currently in a world
        ↓
Read current FPS
        ↓
Read Minecraft FPS limit
        ↓
Update performance tracking
        ↓
Optimization check
        ↓
Evaluate FPS
        ↓
Adjust Render Distance if necessary

This prevents the Render Distance from constantly changing and helps keep performance more stable.

📈 FPS-Based Optimization

RenderOptimizer uses an FPS target of approximately 50 FPS.

When FPS is at or above the target, the current performance is considered acceptable.

When FPS falls below the target, RenderOptimizer can begin reducing the Render Distance.

There is also a lower FPS threshold for more aggressive optimization.

Normal FPS problem
        ↓
Small Render Distance reduction

Very low FPS
        ↓
Larger Render Distance reduction

This allows the optimizer to react differently depending on how severe the performance problem is.

🔄 Recovery System

RenderOptimizer does not permanently lower the player's Render Distance.

When optimization begins, the system remembers the Render Distance that should be restored when performance becomes stable again.

For example:

32 chunks
   ↓
FPS drops
   ↓
27 chunks
   ↓
22 chunks
   ↓
17 chunks
   ↓
FPS recovers
   ↓
20 chunks
   ↓
23 chunks
   ↓
26 chunks
   ↓
29 chunks
   ↓
32 chunks

The recovery process can happen gradually rather than immediately jumping back to the maximum value.

This helps prevent another sudden performance spike.

💤 AFK Chunk Limit

AFK Chunk Limit is designed for players who leave Minecraft running while they are away.

When enabled, RenderOptimizer detects when the player has remained stationary for approximately one minute.

After that, the Render Distance is reduced to 8 chunks.

For example:

Player Render Distance: 32 chunks

        ↓
Player becomes AFK

        ↓
1 minute passes

        ↓

Render Distance: 8 chunks

When the player moves again, the previous Render Distance is restored.

The system also works with other Render Distances:

16 → AFK → 8 → movement → 16

32 → AFK → 8 → movement → 32

The original Render Distance is saved dynamically instead of being hardcoded.

🚶 AFK Movement Detection

The AFK system checks the player's actual position.

Simply looking around does not count as movement.

For example:

Player stands still
Player looks left
Player looks right
Player looks around

→ Still AFK

Actual movement ends AFK mode.

🌄 Far Terrain Optimization

RenderOptimizer also includes a Far Terrain Optimization system.

The purpose of this system is to reduce the rendering workload created by distant terrain.

Minecraft can spend a large amount of resources rendering terrain that is very far away from the player.

However, distant terrain usually does not require the same level of detail as terrain directly around the player.

RenderOptimizer therefore provides multiple optimization modes:

OFF
LOW
MEDIUM
ULTRA

The general concept is:

Player
  ↓
Nearby terrain
  ↓
High detail

Farther terrain
  ↓
Reduced rendering workload

Very distant terrain
  ↓
More aggressive optimization

This allows high Render Distances to remain visually useful while reducing unnecessary rendering work.

🔥 ULTRA Mode

ULTRA is the strongest Far Terrain Optimization mode currently available.

It is designed to significantly reduce the amount of normal detailed terrain rendering required at long distances.

This is especially useful when using very high Render Distances such as 32 chunks.

Instead of rendering every distant area in exactly the same way as nearby terrain, RenderOptimizer attempts to prioritize the area around the player.

The result can be a significant reduction in rendering workload depending on the hardware and Minecraft world.

🌊 Smooth Render Distance Mode

Changing Minecraft's Render Distance can sometimes cause noticeable visual flickering while the renderer updates.

RenderOptimizer includes Smooth Render Distance Mode to reduce this effect.

This is particularly useful when the automatic Render Distance optimizer is actively changing the Render Distance.

Instead of allowing every Render Distance change to cause a full visual disruption, the mod modifies the relevant renderer update behavior.

The goal is to make automatic Render Distance changes feel much smoother.

📊 Display FPS

RenderOptimizer can display the current FPS directly on the Minecraft HUD.

When enabled, it displays something similar to:

FPS: 120

This makes it easier to monitor performance while testing different Render Distances and optimization modes.

💾 Configuration Saving

RenderOptimizer automatically saves its settings.

The configuration file is stored in:

.minecraft/config/renderoptimizer.properties

The configuration contains the state of the main RenderOptimizer features and the selected Far Terrain mode.

For example:

renderDistanceOptimizer=true
smoothRenderDistance=true
displayFPS=false
afkChunkLimit=true
farTerrainOptimization=ULTRA

When Minecraft starts again, the saved settings are automatically loaded.

🧩 Fabric Mixins

RenderOptimizer is built as a Fabric client-side mod and uses Fabric Mixins to modify specific parts of Minecraft's client behavior.

Mixins are used for several systems, including:

Video settings integration
Render Distance behavior
FPS display
Far Terrain rendering
Entity rendering
Block entity rendering
Particle optimization
Renderer updates
Distant terrain culling

Using Mixins allows RenderOptimizer to modify specific parts of Minecraft without replacing the entire rendering engine.

🏗️ Modular Architecture

RenderOptimizer is divided into multiple independent systems.

The main client tick flow is conceptually:

Client Tick
    │
    ├── RenderDistanceOptimizer
    │
    ├── AFKChunkLimitOptimizer
    │
    └── FarTerrainOptimizer

Each system has its own responsibility.

RenderDistanceOptimizer

Responsible for:

FPS monitoring
FPS limit handling
Render Distance reduction
Render Distance recovery
optimization state
AFKChunkLimitOptimizer

Responsible for:

AFK detection
movement detection
saving the previous Render Distance
reducing Render Distance during AFK
restoring Render Distance after movement
FarTerrainOptimizer

Responsible for:

Far Terrain modes
distant terrain optimization
controlling the selected optimization level

This separation makes the project easier to maintain and expand.

⚙️ Optimization States

The different features have independent states.

Conceptually:

RenderOptimizerState
        ↓
ON / OFF

SmoothRenderDistanceState
        ↓
ON / OFF

DisplayFPSState
        ↓
ON / OFF

AFKChunkLimitState
        ↓
ON / OFF

FarTerrainOptimizerState
        ↓
OFF / LOW / MEDIUM / ULTRA

This allows users to combine features however they want.

For example:

Render Distance Optimizer: ON
Smooth Mode: ON
Display FPS: OFF
AFK Chunk Limit: ON
Far Terrain: ULTRA
🎮 Example: 32 Chunk Render Distance

One of the main use cases for RenderOptimizer is playing Minecraft with a high Render Distance.

Without optimization:

Render Distance: 32 chunks

Large amount of terrain
        ↓
Large rendering workload
        ↓
Lower FPS

With Far Terrain Optimization:

Render Distance: 32 chunks

Nearby terrain
        ↓
Normal detailed rendering

Distant terrain
        ↓
Optimized rendering

Unnecessary workload
        ↓
Reduced

This can allow players to use much higher Render Distances than they would normally be comfortable using.

Actual performance improvements depend on the player's hardware, world, graphics settings, shaders, resource packs, entities, and other mods.

🖥️ Client-Side

RenderOptimizer is designed as a client-side optimization mod.

Its main systems modify the local Minecraft client's rendering and performance behavior.

The mod does not need to modify server-side gameplay logic for its primary optimization features.

🔧 Installation
Requirements
Minecraft 1.21.11
Fabric Loader
Fabric API
Compatible Java version
Installation
Install Fabric for Minecraft 1.21.11.
Install Fabric API.
Download renderoptimizer-1.0.0.jar.
Put the JAR file into your .minecraft/mods folder.
Launch Minecraft using Fabric.
⚠️ Compatibility

Because RenderOptimizer modifies parts of Minecraft's client rendering system, compatibility may vary with other rendering and optimization mods.

Potential conflicts may occur with mods that heavily modify:

Terrain rendering
LevelRenderer
Chunk rendering
Entity rendering
Particle rendering
Video settings
Minecraft's rendering pipeline

If you experience crashes or visual problems, test RenderOptimizer without other rendering modifications first.

🛠️ Troubleshooting
Render Distance is not changing

Make sure:

Render Distance Optimizer = ON

Also make sure you are currently inside a Minecraft world.

AFK Chunk Limit is changing my Render Distance

Make sure:

AFK Chunk Limit = ON

If enabled, the Render Distance will be reduced after approximately one minute without movement.

Moving again restores the previous Render Distance.

I want a fixed Render Distance

Turn:

Render Distance Optimizer = OFF

This prevents the automatic FPS-based Render Distance system from changing the player's Render Distance.

🧪 Performance Testing

For meaningful performance comparisons, use the same conditions.

For example:

Same Minecraft version
Same world
Same location
Same Render Distance
Same Simulation Distance
Same resource pack
Same shader settings
Same entity conditions

Then compare:

RenderOptimizer OFF

against:

RenderOptimizer ON

This provides a much more useful performance comparison than testing completely different worlds or locations.

🎯 Who Is RenderOptimizer For?

RenderOptimizer is especially useful for players who:

Want to use high Render Distances
Experience FPS drops at high Render Distances
Want automatic Render Distance management
Want distant terrain optimization
Leave Minecraft AFK
Want to monitor FPS
Want their settings saved automatically
Want more control over the balance between visual distance and performance
📦 Example Configurations
Maximum Performance
Render Distance Optimizer: ON
Smooth Render Distance: ON
Far Terrain Optimization: ULTRA
AFK Chunk Limit: ON
Display FPS: ON
Balanced
Render Distance Optimizer: ON
Smooth Render Distance: ON
Far Terrain Optimization: MEDIUM
AFK Chunk Limit: ON
Display FPS: OFF
Visual Quality Priority
Render Distance Optimizer: OFF
Smooth Render Distance: ON
Far Terrain Optimization: OFF
AFK Chunk Limit: OFF
Display FPS: OFF
🚀 Future Development

RenderOptimizer is still under active development.

Possible future improvements include:

Improved LOW mode
Improved MEDIUM mode
Further ULTRA optimization
More advanced Level of Detail rendering
Better distant terrain rendering
More intelligent FPS detection
Better FPS stabilization
More configuration options
Improved compatibility
Additional rendering optimizations
More efficient distant-object handling

The long-term goal is to continue improving performance without making the world look unnecessarily low-detail.

🧠 Development Philosophy

The core philosophy of RenderOptimizer is:

Do not reduce everything. Reduce what does not need to be rendered at full cost.

A traditional optimization approach might simply reduce every graphical setting when FPS drops.

RenderOptimizer instead attempts to determine where rendering resources are actually useful.

The basic idea is:

Nearby terrain
    ↓
Important
    ↓
Keep detail

Distant terrain
    ↓
Less important
    ↓
Optimize

Player becomes AFK
    ↓
Rendering becomes less important
    ↓
Reduce Render Distance

Player returns
    ↓
Restore previous settings

This allows the mod to dynamically adapt to the current situation.

📜 License

See the repository license for the current licensing terms.

👤 Author

Created by firuhowa.

RenderOptimizer is an independent Minecraft Fabric optimization project.

❤️ Final Goal

The goal of RenderOptimizer is simple:

Make high Render Distance more practical without forcing the player to sacrifice everything else.

Minecraft worlds are extremely large, but rendering everything at maximum detail at all times is not always necessary.

RenderOptimizer attempts to make better use of the available performance by dynamically reducing unnecessary rendering work.

Instead of simply lowering the player's settings, the mod attempts to adapt to the current situation.

High FPS
   ↓
Keep high Render Distance

Low FPS
   ↓
Reduce unnecessary rendering

AFK
   ↓
Reduce Render Distance

Player returns
   ↓
Restore Render Distance

Distant terrain
   ↓
Optimize

Nearby terrain
   ↓
Keep detail

Higher Render Distance.
Less unnecessary rendering.
More performance where it matters.
Please join Discord to get the latest information quickly.　https://discord.gg/xpw5fgMEcW
