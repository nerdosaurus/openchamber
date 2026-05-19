---
name: drawio
description: Create and edit draw.io (.drawio) diagram files using native mxGraphModel XML. Use for flowcharts, architecture diagrams, and process maps.
---

# draw.io Diagram Authoring

## File format

`.drawio` files are XML with `<mxfile>` root containing one or more `<diagram>` elements. Each diagram holds an `<mxGraphModel>`.

Minimal blank document:

```xml
<mxfile>
  <diagram name="Page-1">
    <mxGraphModel dx="0" dy="0" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="827" pageHeight="1169" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

## Cell types

### Vertex (shape)

Use `vertex="1"`. The `style` attribute selects the shape.

| Shape | `style` |
|---|---|
| Rounded rectangle | `rounded=1;whiteSpace=wrap;html=1;` |
| Rectangle | `rounded=0;whiteSpace=wrap;html=1;` |
| Decision/diamond | `rhombus;whiteSpace=wrap;html=1;` |
| Terminator (pill) | `rounded=1;whiteSpace=wrap;html=1;arcSize=50;` |
| Process | `rounded=1;whiteSpace=wrap;html=1;arcSize=20;` |
| Document | `shape=document;whiteSpace=wrap;html=1;` |
| Cylinder (database) | `shape=cylinder;whiteSpace=wrap;html=1;` |
| Hexagon | `shape=hexagon;perimeter=hexagonPerimeter2;whiteSpace=wrap;html=1;` |
| Parallelogram | `shape=parallelogram;perimeter=parallelogramPerimeter;whiteSpace=wrap;html=1;` |
| Trapezoid | `shape=trapezoid;perimeter=trapezoidPerimeter;whiteSpace=wrap;html=1;` |
| Ellipse / circle | `ellipse;whiteSpace=wrap;html=1;` |
| Actor (stick figure) | `shape=actor;whiteSpace=wrap;html=1;` |
| Cloud | `shape=cloud;whiteSpace=wrap;html=1;` |
| Card | `shape=card;whiteSpace=wrap;html=1;` |
| Step (angled) | `shape=step;perimeter=stepPerimeter;whiteSpace=wrap;html=1;` |

Vertex geometry:

```xml
<mxCell id="n1" vertex="1" parent="1" style="rounded=1;whiteSpace=wrap;html=1;">
  <mxGeometry x="160" y="80" width="120" height="40" as="geometry"/>
</mxCell>
```

### Edge (connector)

Use `edge="1"` with `source` and `target`:

```xml
<mxCell id="e1" edge="1" parent="1" source="n1" target="n2" style="edgeStyle=orthogonalEdgeStyle;">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Edge labels in `value` attribute:

```xml
<mxCell id="e1" edge="1" parent="1" source="n1" target="n2" style="..." value="Yes"/>
```

Waypoints:

```xml
<mxCell id="e1" edge="1" parent="1" source="n1" target="n2" style="edgeStyle=orthogonalEdgeStyle;">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="300" y="150"/>
    </Array>
  </mxGeometry>
</mxCell>
```

### Containers (swimlanes, groups)

Set `parent` to the container cell ID:

```xml
<mxCell id="c1" vertex="1" parent="1" style="swimlane;whiteSpace=wrap;html=1;startSize=30;">
  <mxGeometry x="40" y="40" width="500" height="300" as="geometry"/>
</mxCell>
<mxCell id="n2" vertex="1" parent="c1" style="rounded=1;whiteSpace=wrap;html=1;">
  <mxGeometry x="60" y="80" width="120" height="40" as="geometry"/>
</mxCell>
```

`startSize=30` sets the height of the swimlane title bar. For invisible groups: `group;pointerEvents=0;`

## Style customization

Add these to the `style` string:

### Colors
- `fillColor=#...` background fill
- `strokeColor=#...` border color
- `textColor=#...` text color
- `gradientColor=#...` gradient end color
- `fontColor=#...` font color (alternative to textColor)

### Stroke
- `strokeWidth=2` border thickness (default 1)
- `dashed=1` dashed border
- `rounded=1` rounded corners
- `arcSize=20` corner radius

### Text
- `fontSize=12` font size
- `fontStyle=1` bold (2=italic, 4=underline, additive)
- `fontFamily=Helvetica` font family
- `align=center` horizontal (left, center, right)
- `verticalAlign=middle` vertical (top, middle, bottom)
- `spacing=6` padding inside shape
- `spacingTop=-4` adjust text vertical position (useful for diamonds)
- `whiteSpace=wrap` word wrap
- `html=1` HTML labels
- `labelBackgroundColor=none` background behind text

### Shadow
- `shadow=0` no shadow
- `shadow=1` drop shadow

### Images in shapes
- `image;image=URL` image shape with URL
- `shape=image;imageURL=https://...` image from URL
- `shape=image;verticalLabelPosition=bottom;labelBackgroundColor=#FFFFFF;` image with label below

## Edge style options

### Arrowheads
- `endArrow=block` arrow style (none, block, open, classic, diamond, oval, etc.)
- `endFill=1` filled arrowhead
- `endSize=8` arrowhead size
- `startArrow=none` same options for start
- `startFill=0`
- `startSize=8`

### Routing
- `edgeStyle=orthogonalEdgeStyle` orthogonal (right-angle) routing
- `edgeStyle=curved` curved routing
- `edgeStyle=elbowEdgeStyle` elbow routing
- `edgeStyle=isometricEdgeStyle` isometric routing
- `rounded=0` edge corners (0=sharp, 1=rounded)
- `jettySize=auto` connector stub length
- `orthogonalLoop=1` loopback routing

### Edge appearance
- `strokeWidth=1`
- `strokeColor=#...`
- `dashed=1` dashed line
- `dashPattern=8 4` custom dash pattern

### Edge label placement
- `labelBackgroundColor=none`
- `fontSize=11`
- `align=center`
- `verticalLabelPosition=middle`
- `horizontalLabelPosition=middle` (use `y` offset in mxGeometry to move label)

## Common diagram patterns

### Flowchart

Start → Process → Decision → yes/no branches → End

```xml
<mxCell id="s1" vertex="1" parent="1" style="rounded=1;whiteSpace=wrap;html=1;arcSize=50;" value="Start">
  <mxGeometry x="160" y="20" width="120" height="40" as="geometry"/>
</mxCell>
<mxCell id="p1" vertex="1" parent="1" style="rounded=1;whiteSpace=wrap;html=1;" value="Do something">
  <mxGeometry x="160" y="100" width="120" height="40" as="geometry"/>
</mxCell>
<mxCell id="d1" vertex="1" parent="1" style="rhombus;whiteSpace=wrap;html=1;spacing=6;spacingTop=-4;" value="Condition?">
  <mxGeometry x="170" y="180" width="100" height="80" as="geometry"/>
</mxCell>
```

### Decision branches

Connect diamond to two outcomes. Use `value="Yes"` / `value="No"` on edges:

```xml
<mxCell id="e1" edge="1" parent="1" source="d1" target="a1" style="edgeStyle=orthogonalEdgeStyle;endArrow=block;endFill=0;" value="Yes"/>
<mxCell id="e2" edge="1" parent="1" source="d1" target="a2" style="edgeStyle=orthogonalEdgeStyle;endArrow=block;endFill=0;" value="No"/>
```

### Swimlane / container with title

```xml
<mxCell id="lane1" vertex="1" parent="1" style="swimlane;whiteSpace=wrap;html=1;startSize=30;horizontal=1;fillColor=#f0f0f0;" value="Service Layer">
  <mxGeometry x="40" y="40" width="400" height="200" as="geometry"/>
</mxCell>
```

## ID generation

Use unique IDs for each cell. Prefix your IDs (e.g., `cell-1`, `cell-2`, `edge-1`, `edge-2`) to avoid collisions.

## Coordinate system

- Origin (0,0) is top-left
- X increases to the right, Y increases downward
- Use `dx` and `dy` in `<mxGraphModel>` for scroll offset (not position)

## Spacing guidelines

- Horizontal gap between nodes: ~120-200px
- Vertical gap between nodes: ~80-120px
- Container padding: at least 30px from inner nodes to container border
- Arrowhead straight segment: at least 20px before the target
- Diamond width ~100px, height ~80px

## XML writing rules

1. Always include `<root>` with cells `id="0"` and `id="1"` (parent="0")
2. All normal diagram cells have `parent="1"` unless inside a container
3. Every edge cell must have `<mxGeometry relative="1" as="geometry"/>` — never self-closing
4. Vertex positions use `x`, `y`, `width`, `height` in `<mxGeometry>`
5. Use `&lt;br&gt;` for multi-line labels in the `value` attribute
6. Set `html=1` in styles for rich text support
7. When editing an existing file, preserve all existing cells and only add/modify the ones you need to change
8. After writing, tell the user the file path so they can open it in the diagram editor
