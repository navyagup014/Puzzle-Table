# Developer guide

## Layout
Everything is in `index.html`: styles, markup and one script. There is no build step.

| Part | What it does |
|---|---|
| Picture library | Six artworks drawn with canvas calls, plus user uploads |
| Geometry | `edge()` and `piecePath()` cut each piece; `scatter()` deals loose pieces onto the apron around the board |
| Snapping | `findSnap()` and `commitSnap()` snap a piece to the board or to a neighbour within a tolerance (`TOL = 18`) |
| Input | Pointer events handle drag, pan and pinch; a 6px threshold separates a click from a drag |
| Saves | `puzzletable.progress.v1` holds up to 8 in-progress puzzles; `puzzletable.gallery.v3` holds finished ones |
| Gallery | Frames are built from CSS (oak, gilded, Art Deco, whimsical, gallery, walnut) |

## Key ideas
- **Union-find groups:** `parent[]` links connected pieces; `find()` and `union()` merge groups as they snap.
- **Symmetric edges:** neighbours trace a shared edge in opposite directions, so each tab profile must be symmetric or gaps appear.
- **Resume from geometry:** saves store the tab grids (`hTab`, `vTab`), piece positions, tray assignments and the group map, so a resumed puzzle is identical.
- **Pieces are canvases:** one `<canvas>` per piece. This is simple and fine up to a few hundred pieces.

## Known limits
- Past roughly 300 to 500 pieces, one canvas per piece gets slow. Larger puzzles need a single-canvas sprite renderer with a spatial grid.
- localStorage is about 5MB across all saved puzzles, which is why saves are capped at 8. IndexedDB would lift that.
- Trays are a fixed width, so at low piece counts a single piece can be wider than a tray.

## Ideas on the list
- 100 / 200 / 500 / 1000 piece counts (needs the renderer rewrite above)
- A mix of piece cuts within one puzzle
- Real photo library once hosted on its own domain
- A walkable gallery

## Changelog
- **Now:** four tabs, autosaved in-progress puzzles, collapsible trays, reference lid, framed gallery with titles and month grouping.
