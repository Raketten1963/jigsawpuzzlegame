# JigsawPuzzle

A lightweight, reusable JavaScript class for creating interactive jigsaw puzzle games in web applications. Features customizable piece shapes, rotation support, save/load functionality, and touch/mouse controls.

## Demo

Try it out live: [https://jigsawpuzzleclass.raketten.net/game.html](https://jigsawpuzzleclass.raketten.net/game.html)

## Installation

```bash
npm install jigsaw-puzzle
```

## Quick Start

```javascript
import { JigsawPuzzle } from 'jigsaw-puzzle';

const puzzle = new JigsawPuzzle('puzzle-container', {
    image: 'https://example.com/image.jpg',
    numPieces: 20,
    shapeType: 0,
    allowRotation: false,
    onReady: () => {
        puzzle.start();
    },
    onWin: () => {
        console.log('Puzzle solved!');
    }
});
```

## HTML Setup

```html
<div id="puzzle-container"></div>
<script type="module" src="your-script.js"></script>
```

Make sure your container has a defined size:

```css
#puzzle-container {
    width: 100vw;
    height: 100vh;
    position: relative;
}
```

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `image` | string | `null` | URL or data URL of the image to use for the puzzle |
| `numPieces` | number | `20` | Number of puzzle pieces (approximate - actual count depends on optimal grid layout) |
| `shapeType` | number | `0` | Shape type for puzzle pieces (0-3):<br>• `0` - Classic jigsaw shape (curved tabs)<br>• `1` - Alternative shape 1<br>• `2` - Alternative shape 2<br>• `3` - Straight edges (rectangular pieces) |
| `allowRotation` | boolean | `false` | Whether pieces can be rotated by clicking/tapping (90° increments) |
| `onReady` | function | `null` | Callback function called when the puzzle is ready (image loaded and displayed) |
| `onWin` | function | `null` | Callback function called when the puzzle is completed |
| `onStart` | function | `null` | Callback function called when a game starts |
| `onStop` | function | `null` | Callback function called when a game is stopped |

## API Methods

### `start()`

Starts a new game with the current settings. Creates the puzzle pieces and distributes them.

```javascript
puzzle.start();
```

**Important:** Call this only after the puzzle is ready (use the `onReady` callback).

### `stop()`

Stops the current game and returns to the image preview state.

```javascript
puzzle.stop();
```

### `reset()`

Resets the puzzle to initial state. Reloads the current image and prepares for a new game.

```javascript
puzzle.reset();
// Then wait for onReady callback and call puzzle.start()
```

### `setImage(imageUrl)`

Sets a new image for the puzzle.

**Parameters:**
- `imageUrl` (string) - URL or data URL of the image

```javascript
puzzle.setImage('https://example.com/new-image.jpg');
// Image will reload, wait for onReady callback
```

### `setOptions(newOptions)`

Updates puzzle options without creating a new instance.

**Parameters:**
- `newOptions` (object) - Object with options to update (partial updates allowed)

```javascript
puzzle.setOptions({
    numPieces: 50,
    allowRotation: true,
    shapeType: 1
});
```

### `save([callback])`

Saves the current game state. If no callback is provided, saves to localStorage.

**Parameters:**
- `callback` (function, optional) - Function that receives the saved data as JSON string

```javascript
// Save to localStorage (default)
puzzle.save();

// Save with custom callback
puzzle.save((savedData) => {
    localStorage.setItem('myPuzzleSave', savedData);
    // Or send to server, download as file, etc.
});
```

### `load([savedData])`

Loads a previously saved game state.

**Parameters:**
- `savedData` (string, optional) - JSON string of saved data. If not provided, loads from localStorage

```javascript
// Load from localStorage
puzzle.load();

// Load from custom data
const savedData = localStorage.getItem('myPuzzleSave');
puzzle.load(savedData);
```

### `destroy()`

Completely destroys the puzzle instance, cleaning up all resources. Use this when you want to remove the puzzle and create a new one in the same container.

```javascript
puzzle.destroy();
puzzle = new JigsawPuzzle('puzzle-container', {
    image: 'new-image.jpg',
    numPieces: 30
});
```

## Complete Example

```javascript
import { JigsawPuzzle } from 'jigsaw-puzzle';

const puzzle = new JigsawPuzzle('puzzle-container', {
    image: 'https://example.com/image.jpg',
    numPieces: 20,
    shapeType: 0,
    allowRotation: false,
    onReady: () => {
        // Puzzle is ready, start the game
        puzzle.start();
    },
    onWin: () => {
        alert('Congratulations! You solved the puzzle!');
    },
    onStart: () => {
        console.log('Game started');
    },
    onStop: () => {
        console.log('Game stopped');
    }
});

// Save game
function saveGame() {
    puzzle.save((data) => {
        localStorage.setItem('puzzleSave', data);
        console.log('Game saved!');
    });
}

// Load game
function loadGame() {
    const saved = localStorage.getItem('puzzleSave');
    if (saved) {
        puzzle.load(saved);
    }
}
```

## User Interactions

- **Mouse/Touch:** Click and drag pieces to move them
- **Rotation:** If `allowRotation` is enabled, quick click/tap rotates pieces 90°
- **Piece Merging:** When pieces are close and correctly aligned, they automatically merge
- **Pan:** Click and drag on empty space to pan all pieces
- **Zoom:** Mouse wheel to zoom in/out, or pinch gesture on touch devices

## Browser Compatibility

Requires modern browser features:
- ES6 Modules support
- Canvas API
- Path2D API
- Touch events (for mobile support)

Works in all modern browsers (Chrome, Firefox, Safari, Edge).

## Styling

The puzzle adds these CSS classes you can style:

- `.polypiece` - Individual puzzle pieces
- `.polypiece.moving` - Pieces during animation
- `.gameCanvas` - Reference image canvas (hidden during play)

## Troubleshooting

### Puzzle doesn't start
- Make sure you're calling `start()` in the `onReady` callback
- Check that the image URL is valid and accessible (CORS issues if loading from different domain)
- Verify the container element exists and has a size

### Image doesn't load
- Check browser console for CORS errors
- Use data URLs or images from the same domain
- Ensure the image URL is correct

### Pieces don't merge
- Make sure pieces are close enough (the puzzle calculates optimal distance)
- Check that pieces are in the same rotation if rotation is enabled
- Verify pieces are correctly aligned

## License

Copyright (c) 2026 by Dillon (https://codepen.io/Dillo/pen/QWKLYab) & Henrik Rasmussen (https://www.raketten.net)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Authors

- Dillon - https://codepen.io/Dillo/pen/QWKLYab
- Henrik Rasmussen - https://www.raketten.net

