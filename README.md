# 🚀 Asteroid Blaster

A fully featured arcade space shooter built in Python with Pygame. Pilot a spaceship through
endless waves of incoming asteroids, destroy them before they destroy you, and chase a high
score as difficulty scales over time.

Playable in the browser — no installation required.

**[▶ Play Live](https://danjocorona.github.io/asteroid-blaster)**

---

## Gameplay

- **Rotate** your ship left and right
- **Thrust** forward with momentum-based physics
- **Shoot** to destroy asteroids — large ones split into smaller, faster fragments
- **Survive** as long as possible while asteroids spawn continuously
- **Score** points based on asteroid size — bigger kills are worth more

| Asteroid Size | Points |
|---------------|--------|
| Large         | 100    |
| Medium        | 50     |
| Small         | 25     |

---

## Features

- **Physics-based movement** — thrust builds velocity, friction bleeds it off over time using `pygame.Vector2`
- **Asteroid splitting** — large asteroids break into two medium fragments, medium into two small ones
- **Circle collision detection** — per-object collision radius for accurate hit registration
- **Sprite-based rendering** — all game objects use rotatable image sprites
- **Sound design** — distinct sound effects per asteroid size, laser fire, death, and looping background music
- **Start menu & game over screen** — full game loop with restart support
- **Browser focus handling** — music pauses automatically when the tab loses focus (web build)
- **Web build via Pygbag** — compiled to WebAssembly and playable directly in the browser

---

## Project Structure

```
asteroid-blaster/
├── main.py               # Game loop, event handling, collision logic, rendering
├── game/
│   ├── player.py         # Player ship — rotation, thrust, friction, wrapping
│   ├── asteroid.py       # Asteroid — random velocity, screen wrapping, splitting
│   └── bullet.py         # Bullet — directional firing, off-screen culling
├── assets/
│   ├── images/           # Sprites: player, laser, asteroid, background
│   └── sounds/           # SFX: laser, explosions (3 sizes), lose, menu, space music
├── index.html            # Pygbag web build entry point
├── favicon.png
└── README.md
```

---

## How It Works

### Game Loop (`main.py`)
The main loop runs at 60fps via `pygame.clock.tick(60)`. Each frame it processes input events,
updates all game objects, checks collisions, and draws everything to the screen. A
`pygame.USEREVENT` timer fires every 3 seconds to spawn a new asteroid from a random screen edge.

### Player (`player.py`)
The ship rotates around its center using an `angle` value updated by left/right key input.
Thrust converts the current angle into a directional vector added to `(dx, dy)` velocity.
Friction (`0.98` multiplier per frame) creates natural deceleration. The ship wraps around
all four screen edges.

### Asteroids (`asteroid.py`)
Each asteroid spawns with a random direction and speed between 1.0–3.0. On destruction,
the `split()` method spawns two new asteroids at the next size down with fresh random
velocities. The smallest size (32px) does not split further.

### Bullets (`bullet.py`)
Bullets inherit the player's current velocity at the moment of firing, so shooting while
moving feels natural. They travel in the direction the ship is pointing and are culled
once they leave the screen bounds.

### Collision Detection
Both the player and asteroids expose a `get_collision_circle()` method returning a center
point and radius. The `math.dist()` function compares these each frame — if the distance
between centers is less than the sum of the radii, it's a hit.

---

## Tech Stack

| | |
|---|---|
| Language | Python 3 |
| Game Library | Pygame |
| Web Build | Pygbag (Python → WebAssembly) |
| Physics | pygame.Vector2 |
| Architecture | Object-oriented (Player, Asteroid, Bullet classes) |

---

### Controls

| Key | Action |
|-----|--------|
| `←` `→` | Rotate ship |
| `↑` | Thrust forward |
| `Space` | Fire |
| `Enter` | Start game |
| `R` | Restart after game over |

---

## Web Build

The browser version was compiled using [Pygbag](https://pygame-web.github.io/), which
packages Python and Pygame into a WebAssembly bundle that runs natively in modern browsers
with no plugins or installation needed.

---

## License

This project is open source and available under the [MIT License](LICENSE).
