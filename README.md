# Golang Tetris

A Tetris clone written in Go using the [Pixel](https://github.com/faiface/pixel) 2D OpenGL game library. Features smooth gameplay, a parallax mountain background, ghost-piece preview, and a next-piece display.

![A sample example of the program](docs/media/example1.png?raw=true "An example of the program running")
![A sample example of the program](docs/media/example2.png?raw=true "An example of the program running")

## Features

- 7 classic Tetris pieces (I, J, L, O, S, T, Z)
- Ghost piece showing where the active piece will land
- Next piece preview
- Progressive difficulty — speed increases every 60 seconds
- Score tracking
- Parallax mountain background

## Requirements

- Go 1.16+
- A C compiler (CGO required for OpenGL bindings)
  - **Windows:** Install [WinLibs GCC](https://winlibs.com/) via `winget install BrechtSanders.WinLibs.POSIX.UCRT`
  - **macOS:** Xcode Command Line Tools (`xcode-select --install`)
  - **Linux:** `sudo apt install gcc` (or equivalent)

## Running the Game

```bash
# Download dependencies
go mod tidy

# Run (CGO must be enabled)
CGO_ENABLED=1 go run .
```

**Windows (PowerShell):**
```powershell
$env:CGO_ENABLED = "1"
go run .
```

## Controls

| Key | Action |
|-----|--------|
| `←` / `→` | Move piece left / right |
| `↑` | Rotate piece |
| `↓` | Soft drop (fast fall) |
| `Space` | Hard drop (instant drop) |

## Project Structure

| File | Description |
|------|-------------|
| `main.go` | Window setup, game loop, input handling, rendering |
| `board.go` | Board logic: gravity, collision, rotation, row clearing, ghost piece |
| `shape.go` | Shape definitions and transformations for all 7 pieces |
| `spritesheet/spritesheet.go` | Sprite sheet loader and background image generator |
| `resources/` | Game assets (sprite sheet, parallax background images) |

## Dependencies

- [`github.com/faiface/pixel`](https://github.com/faiface/pixel) — 2D OpenGL game library
- [`golang.org/x/image`](https://pkg.go.dev/golang.org/x/image) — image utilities

## Todo

- [ ] Menus (opening, game-over, pause)
- [ ] Animation for row clearing
- [ ] Music and sound effects
