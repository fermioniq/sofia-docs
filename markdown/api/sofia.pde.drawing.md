# sofia.pde.drawing

### *class* sofia.pde.drawing.Style(SPACING_X: float = 200, CELL_CENTRE_Y: float = 150, CELL_HEIGHT: float = 200, CELL_CENTRE_DOT_RADIUS: float = 8, FIELD_DOT_RADIUS: float = 4, BOUNDARY_EXTENT: float = 60, UPPER_TEXT_Y: float = 10, LINEWIDTH: float = 3, EPS: float = 1, AXIS_LENGTH: float = 66.66666666666667, AXIS_XY: tuple[float, float] = (0, 250.0), AXES_ARROW_HEAD_WIDTH: float = 12, AXES_ARROW_TAIL_WIDTH: float = 3, COLOR_MODE: str = 'light', COLOR_INTERIOR: str = '#418b8b', COLOR_GHOST: str = '#728c8c', COLOR_BOUNDARY: str = '#b81370', COLOR_FIELD_DOT: str = '#000000', UPPER_TEXT_FONT_SIZE: int = 16, BOUNDARY_TEXT_FONT_SIZE: int = 16, FIELD_LABEL_FONT_SIZE: int = 16, THETA_LABEL_FONT_SIZE: int = 14, AXES_TEXT_FONT_SIZE: int = 16, DEFAULT_N_GHOST: int = 2, DEFAULT_N_INTERIOR: int = 3, MIDDLE_SPACE: float = 300.0, PADDING_H: float = 12)

Bases: `object`

### sofia.pde.drawing.set_theme(name: str)

Set the current theme, overriding all other settings.

Currently supported: “light” and “dark”.

* **Parameters:**
  **name** – Name of the theme.

### sofia.pde.drawing.get_style() → [Style](#sofia.pde.drawing.Style)

Get the current style.

### sofia.pde.drawing.update_style(\*\*kwargs)

Update drawing style using kwargs.

* **Parameters:**
  **\*\*kwargs** – Keyword arguments.

### sofia.pde.drawing.draw_grid(G: [Grid](sofia.domain.grid.md#sofia.domain.grid.Grid), \*fields, coords: tuple[sp.Symbol, sp.Symbol] | None = None, edge: Literal['start', 'end'] | None = None, \*\*style_kwargs) → Figure

Draw a schematic of a grid plus (optional) fields.

* **Parameters:**
  * **G** – Grid to draw.
  * **fields** – One or more fields to draw onto the grid.
  * **coords** – Which (two) coordinates to display along the horizontal and vertical axes,
    respectively.
  * **edge** – If set, decides which end of the grid to show (start / end, which for the
    x-direction correspond to left / right). If None (default), then both ends will
    be drawn.
  * **style_kwargs** – Optional style keyword arguments.
* **Returns:**
  Matplotlib Figure object.
* **Return type:**
  figure
