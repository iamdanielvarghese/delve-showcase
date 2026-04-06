# Building Infinite Worlds: Procedural Terrain Generation


!!! abstract "Executive Summary"
    *Delve* is a custom Python-based exploration engine. The core objective was to move away from static, hand-drawn levels and build an architecture capable of generating infinite, playable terrain dynamically using **Perlin Noise algorithms**, rendering natively via **Pygame**, and caching via **SQLite**.

---

## System Architecture

To create natural geography rather than chaotic "TV static," the engine relies on procedural mathematical generation. 

=== "The Algorithm (Perlin Noise)"
    Unlike standard pseudo-random number generators, Perlin Noise calculates smooth, continuous gradients. By layering multiple "octaves" of noise and adjusting the frequency and amplitude, the algorithm generates a mathematical heightmap.

=== "Data Quantization"
    This continuous mathematical output is quantized into discrete grid tiles. The engine maps specific height thresholds to distinct biomes (e.g., deep water, sand, grass, rock) on the fly.

=== "Caching (SQLite)"
    To prevent memory overflow, generated chunks are saved to an SQLite database. When a player leaves an area and returns, the engine reads the cached database chunk rather than recalculating the Perlin math.

---

## Engineering Bottleneck: Dynamic Collision

!!! bug "The Obstacle"
    Standard collision detection relies on static hitboxes drawn in the engine. Because *Delve* generates terrain infinitely, the environment is fundamentally dynamic. The core challenge was mapping the player's continuous pixel coordinates to the discrete, procedurally generated grid arrays in real-time. If the math was slightly off, the sprite would phase directly through generated mountains.

!!! success "The Solution"
    Instead of relying on brute-force hitbox overlaps, I implemented a **predictive grid-mapping system**. Before committing to a movement frame, the engine calculates the player's intended next position, translates it into a grid index, and queries the generated `biomes` array.

### Core Implementation

Here is the production logic handling the predictive collision. Notice how the movement vector is blocked *before* the frame ever renders if the target tile is impassable.

```python
# Calculate intended movement vector
# new_x, new_y = current_x + dx, current_y + dy

# Predictive Grid-Mapping & Collision Check
if 0 <= new_x < MAP_W and 0 <= new_y < MAP_H:
    # Query the dynamically generated Perlin noise array
    if biomes[new_y, new_x] == TYPE_FLOOR:
        # Commit movement only if the target tile is passable
        px, py = new_x, new_y
```
## Tech Stack
* **Language:** Python
* **Rendering:** Pygame
* **Data Storage:** SQLite (for chunk caching)
* **Algorithms:** Perlin Noise (Procedural Generation)