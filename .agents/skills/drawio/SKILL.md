---
name: drawio
description: Create and edit draw.io (.drawio) diagram files using native mxGraphModel XML. Use for flowcharts, architecture diagrams, and process maps.
---

# draw.io Diagram Authoring

## File format

`.drawio` files are XML with an `<mxfile>` root containing one or more `<diagram>` elements. Each diagram holds an `<mxGraphModel>`.

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

Use `vertex="1"`. The `style` attribute selects the shape. Common styles:

| Shape | `style` |
|---|---|
| Rounded rectangle | `rounded=1;whiteSpace=wrap;html=1;` |
| Process/action | `rounded=1;whiteSpace=wrap;html=1;arcSize=20;` |
| Decision/diamond | `rhombus;whiteSpace=wrap;html=1;` |
| Terminator/start-end | `rounded=1;whiteSpace=wrap;html=1;arcSize=50;` |
| Document | `shape=document;whiteSpace=wrap;html=1;` |
| Cylinder (database) | `shape=cylinder;whiteSpace=wrap;html=1;` |
| Hexagon | `shape=hexagon;perimeter=hexagonPerimeter2;whiteSpace=wrap;html=1;` |
| Parallelogram | `shape=parallelogram;perimeter=parallelogramPerimeter;whiteSpace=wrap;html=1;` |
| Trapezoid | `shape=trapezoid;perimeter=trapezoidPerimeter;whiteSpace=wrap;html=1;` |
| Ellipse | `ellipse;whiteSpace=wrap;html=1;` |

Vertex cells need geometry with position and size:

```xml
<mxCell id="n2" vertex="1" parent="1" style="rounded=1;whiteSpace=wrap;html=1;">
  <mxGeometry x="160" y="80" width="120" height="40" as="geometry"/>
</mxCell>
```

### Edge (connector)

Use `edge="1"` with `source` and `target` attributes referencing vertex IDs:

```xml
<mxCell id="e1" edge="1" parent="1" source="n2" target="n3" style="edgeStyle=orthogonalEdgeStyle;">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Edge labels go in the `value` attribute of the edge cell:

```xml
<mxCell id="e1" edge="1" parent="1" source="n2" target="n3" style="..." value="Yes">
```

Waypoints go in `<mxGeometry>`:

```xml
<mxCell id="e1" edge="1" parent="1" source="n2" target="n3" style="edgeStyle=orthogonalEdgeStyle;">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="300" y="150"/>
    </Array>
  </mxGeometry>
</mxCell>
```

Common edge style additions: `endArrow=block;endFill=0;endSize=8;strokeWidth=1;rounded=0;jettySize=auto;orthogonalLoop=1;fontSize=11;labelBackgroundColor=none;`

### Containers

Cells can nest inside other cells by setting `parent` to the container cell ID:

```xml
<mxCell id="container" vertex="1" parent="1" style="swimlane;whiteSpace=wrap;html=1;">
  <mxGeometry x="40" y="40" width="500" height="300" as="geometry"/>
</mxCell>
<mxCell id="child" vertex="1" parent="container" style="rounded=1;whiteSpace=wrap;html=1;">
  <mxGeometry x="60" y="80" width="120" height="40" as="geometry"/>
</mxCell>
```

## ID generation

Use unique IDs for each cell. Pattern: `prefix-N` where N increments. Prefix your IDs (e.g., `cell-1`, `cell-2`) to avoid collisions.

## Spacing guidelines

- Horizontal gap between nodes: ~120-200px
- Vertical gap between nodes: ~80-120px
- Container padding: at least 30px from inner nodes to container border
- Arrowhead straight segment: at least 20px before the target

## XML writing rules

1. Always include `<root>` with cells `id="0"` and `id="1"` (parent="0")
2. All normal diagram cells have `parent="1"` unless inside a container
3. Every edge cell must have `<mxGeometry relative="1" as="geometry"/>` — never self-closing
4. Vertex positions use `x`, `y`, `width`, `height` in `<mxGeometry>`
5. Use `&lt;br&gt;` for multi-line labels in the `value` attribute
6. Set `html=1` in styles for rich text support
