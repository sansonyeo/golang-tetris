# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Run the game
go run .

# Build binary
go build .

# Download dependencies
go mod tidy
```

There is no Makefile, test suite, or linter configuration.

## Architecture

A single-package Go Tetris game using the [Pixel](https://github.com/faiface/pixel) 2D OpenGL library for rendering and input.

**Entry point:** `main.go` — initializes the 765×450 pixel window, loads sprites, and runs the per-frame game loop. The loop applies gravity on a timer, reads keyboard input, triggers row completion checks, and calls rendering functions.

**Game state** is held in package-level variables:
- `gameBoard Board` — the 22×10 grid (top 2 rows are invisible, used for game-over detection)
- `activeShape Shape` — 4 `Point` values representing the current falling piece
- `currentPiece / nextPiece Piece` — enum (I/J/L/O/S/T/Z) for the active and preview pieces
- `gravityTimer`, `baseSpeed`, `gravitySpeed` — timing; speed increases every 60s, capped at 0.2s/drop

**File responsibilities:**

| File | Role |
|------|------|
| `main.go` | Window setup, game loop, input handling, rendering, score display |
| `board.go` | Board methods: gravity, collision detection, wall-kick rotation, row clearing, ghost-piece calculation |
| `shape.go` | Shape transformations (move, rotate), piece definitions for each of 7 Tetris types, game-over check |
| `spritesheet/spritesheet.go` | Loads `resources/blocks.png` sprite sheet and parallax background images; generates overlay backgrounds procedurally |

**Rendering pipeline (per frame):**
1. Draw 5-layer parallax mountain background (`main.go`)
2. Draw semi-transparent board overlay
3. `board.displayBoard()` — renders each non-empty `Block` cell and the ghost piece
4. Draw score text and next-piece preview (`main.go`)

**Block/Piece mapping:** `main.go:pieceToBlock()` maps each `Piece` enum to one of 8 `Block` color constants; blocks index into the `blocks.png` sprite sheet.

**Key controls:** Arrow keys for move/rotate/soft-drop; Space for instant drop. Key-repeat for left/right is simulated manually in `main.go` with a delay counter.

## Dependencies

- `github.com/faiface/pixel v0.9.0` — 2D game library (requires OpenGL; needs a display on the running machine)
- `golang.org/x/image` — image loading utilities used by Pixel
