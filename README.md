# Cub3D 🧱🐺

A 3D game engine developed in C using raycasting techniques, inspired by the classic game Wolfenstein 3D. This 42 School project involves parsing a map file, rendering a 2.5D environment, handling player movement, collisions, and displaying textured walls, sprites, and sky/floor colors.

## 📋 Table of Contents

- [About The Project](#about-the-project)
- [Core Concepts](#core-concepts)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [Map File (`.cub`) Configuration](#map-file-cub-configuration)
- [Project Structure](#project-structure)
- [Makefile Targets](#makefile-targets)
- [Author](#author)
- [Notes](#notes)

## 🎯 About The Project

Cub3D is a graphics project that requires students to create a dynamic 3D view of a maze from a first-person perspective. The challenge lies in implementing a raycasting engine from scratch, managing textures, handling sprites (bonus), and creating a playable experience using the MiniLibX graphics library.

### Project Goals:
-   Understand and implement raycasting algorithms for 2.5D rendering.
-   Parse and validate complex map configuration files.
-   Manage textures for walls (North, South, East, West).
-   Implement player movement (forward, backward, left, right, rotation).
-   Handle basic collision detection with walls.
-   Display floor and ceiling colors.
-   (Bonus) Implement sprite rendering.
-   (Bonus) Save the first rendered image as a BMP file.
-   (Bonus) Implement distance-based shading/fog.
-   (Bonus) Add background music or sound effects.

## 🧠 Core Concepts

-   **Raycasting:** A rendering technique to create a 3D perspective in a 2D map. Rays are cast from the player's viewpoint for each vertical stripe of the screen to determine what is visible.
-   **MiniLibX (MLX):** A simple graphics library provided by 42 to handle window creation, image manipulation, and event handling (keyboard/mouse input).
-   **Map Parsing:** Reading and validating a `.cub` file that defines the game map, textures, colors, and player starting position.
-   **Texture Mapping:** Applying textures to walls based on their orientation (North, South, East, West).
-   **Player Representation:** Managing the player's position (x, y), direction vector, and camera plane.
-   **Digital Differential Analysis (DDA) Algorithm:** Often used in raycasting to determine how far a ray travels before hitting a wall.
-   **BMP Image Saving:** (Bonus) Generating a BMP image file from the first rendered frame.

## ✨ Features

-   First-person 3D view rendered using raycasting.
-   Textured walls with distinct textures for North, South, East, and West faces.
-   Solid colors for floor and ceiling.
-   Player movement: forward/backward, strafe left/right, rotate left/right.
-   Basic wall collision.
-   Parsing of `.cub` scene description files.
-   **Bonus Features Implemented (You can list yours here):**
    -   Sprite rendering (e.g., collectible items, enemies).
    -   Saving the first rendered frame to a BMP file when using the `--save` argument.
    -   [Add other bonuses like health bar, fog, skybox, etc., if you did them]

## 🛠️ Technologies Used

-   **C Language**
-   **MiniLibX (MLX):** Graphics library (usually provided or to be set up for macOS/Linux).
-   **Makefile:** For project compilation.
-   **XPM:** Texture file format commonly used with MiniLibX.
-   **BMP:** Image format for the `--save` option.

## 🚀 Installation

Prerequisites:
-   A C compiler (e.g., `gcc` or `clang`).
-   `make` utility.
-   **MiniLibX:** This library needs to be available on your system.
    -   For macOS, this is often provided.
    -   For Linux, you might need to install dependencies (`xorg`, `libxext-dev`, `libbsd-dev`, etc.) and compile the provided MiniLibX source.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Marouane0107/Cub3D.git
    cd Cub3D
    ```

2.  **Set up MiniLibX (if necessary):**
    Ensure the `Makefile` is correctly linked to your MiniLibX library (`libmlx.a` and header files). This might involve updating paths in the `Makefile` or placing the MiniLibX library in an expected location.

3.  **Compile the project:**
    ```bash
    make
    ```
    This will create the `cub3D` executable.

## 🎮 Usage

To run the game, provide a map file (`.cub`) as an argument:

```bash
./cub3D path/to/your_map.cub
```

**Optional `--save` argument:**
To save the first rendered image as a BMP file (e.g., `cub3D.bmp`) and then exit:
```bash
./cub3D path/to/your_map.cub --save
```

### In-Game Controls (Typical):
-   **W, A, S, D:** Move forward, strafe left, backward, strafe right.
-   **Left Arrow / Right Arrow:** Rotate the camera.
-   **ESC:** Exit the game.
*(Verify and update these if your controls are different)*

## 🗺️ Map File (`.cub`) Configuration

The `.cub` file describes the scene. It typically includes:

-   **Resolution (R):** `R <width> <height>` (e.g., `R 800 600`)
-   **Texture Paths (NO, SO, WE, EA):** Paths to XPM texture files for North, South, West, and East walls.
    -   `NO ./path/to/north_texture.xpm`
    -   `SO ./path/to/south_texture.xpm`
    -   `WE ./path/to/west_texture.xpm`
    -   `EA ./path/to/east_texture.xpm`
-   **Sprite Texture Path (S):** `S ./path/to/sprite_texture.xpm` (for bonus)
-   **Floor Color (F):** RGB color `F <R,G,B>` (e.g., `F 220,100,0`)
-   **Ceiling Color (C):** RGB color `C <R,G,B>` (e.g., `C 0,150,220`)
-   **The Map:** A grid of characters representing walls (`1`), empty spaces (`0`), player start position (`N`, `S`, `E`, `W`), and sprites (`2` - for bonus). The map must be enclosed by walls.

**Example `.cub` snippet:**
```cub
R 1920 1080
NO ./textures/north.xpm
SO ./textures/south.xpm
WE ./textures/west.xpm
EA ./textures/east.xpm
S  ./textures/sprite.xpm

F 220,100,0
C 0,150,220

1111111111
1001000N01
1011000001
1000020001
1111111111
```

The parser must strictly validate the format and content of this file.

## 📁 Project Structure

A common project structure for Cub3D:

```
Cub3D/
├── cub3D                # Executable (after compilation)
├── Makefile
├── includes/            # Header files
│   └── cub3d.h
├── srcs/                # Source files (.c)
│   ├── main.c
│   ├── parser/          # Map parsing logic
│   │   ├── parse_map.c
│   │   └── ...
│   ├── engine/          # Raycasting engine, rendering
│   │   ├── raycaster.c
│   │   └── ...
│   ├── player/          # Player movement, events
│   │   ├── movement.c
│   │   └── ...
│   ├── utils/           # Utility functions
│   │   └── ...
│   └── bonus/           # Bonus feature implementations
│       ├── sprites.c
│       └── bmp_save.c
├── maps/                # Example .cub map files
│   └── example.cub
└── textures/            # Example .xpm texture files
    └── north.xpm
    └── ...
```

## COMMANDS Makefile Targets

Common `Makefile` targets:

-   `all`: Compiles the `cub3D` executable.
-   `clean`: Removes object files (`.o`).
-   `fclean`: Removes object files and the `cub3D` executable.
-   `re`: Performs `fclean` then `all`.
-   `bonus`: (If applicable) Compiles the project with bonus features. If bonuses are part of the main `all` target, this might not be separate.

## 👨‍💻 Author

**Marouane Aouzal** (Marouane0107)
- GitHub: [@Marouane0107](https://github.com/Marouane0107)
- UM6P-1337 Coding School Student

## 📝 Notes

-   This project is a significant step in graphics programming within the 42 curriculum.
-   Careful error handling, especially for map parsing and MiniLibX interactions, is crucial.
-   Memory management (freeing allocated resources like textures and map data) is essential to prevent leaks.
-   The MiniLibX library has different versions and setup procedures for macOS and Linux; ensure compatibility.
-   Adherence to the 42 School coding standards (Norminette) is typically required.

---

*Repository for the 42 School "Cub3D" project.*

*Last Updated: 2024*
