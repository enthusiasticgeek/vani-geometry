# vani-geometry — TODO

> Compiler builtins that already exist and must NOT be reimplemented:
> `abs` `sqrt` `sin` `cos` `tan` `atan2` `acos` `asin` `f64_hypot` `f64_pi()`
> `push` `pop` `len` `set`
>
> No Kosh dependencies -- v0.1.0 is a standalone package.

---

## v0.1.0 — Implemented ✓

### 2D point/vector construction and arithmetic (6 functions)
- [x] `point2d_new`, `point2d_add`, `point2d_sub`, `point2d_scale`
- [x] `point2d_distance`, `point2d_midpoint`

### 2D vector operations (5 functions)
- [x] `vec2d_dot`, `vec2d_cross` (scalar z-component)
- [x] `vec2d_norm`, `vec2d_normalize`
- [x] `vec2d_angle_between` (via atan2(|cross|, dot), numerically safer near 0/pi than acos)

### 3D point/vector construction and arithmetic (5 functions)
- [x] `point3d_new`, `point3d_add`, `point3d_sub`, `point3d_scale`, `point3d_distance`

### 3D vector operations (4 functions)
- [x] `vec3d_dot`, `vec3d_cross`, `vec3d_norm`, `vec3d_normalize`

### 2D lines and segments (4 functions)
- [x] `line2d_point_distance` -- perpendicular distance to an infinite line
- [x] `segment_point_distance` -- clamped-projection distance to a segment
- [x] `segments_intersect` -- orientation-test intersection check
- [x] `segment_intersection_point` -- infinite-line intersection (undefined for parallel lines)

### Triangles and polygons (5 functions)
- [x] `triangle_area` (via cross product)
- [x] `polygon_area`, `polygon_perimeter` (shoelace formula)
- [x] `polygon_centroid` (area-weighted, not mean-of-vertices)
- [x] `point_in_polygon` (ray casting)

### Computational geometry (2 functions)
- [x] `convex_hull` -- Andrew's monotone chain, validated against a square +
      3 interior points (hull correctly excludes all interior points, hull
      area matches the square exactly)
- [x] `closest_pair_distance` -- brute force O(n²)

### Circles (2 functions)
- [x] `circumcenter`, `circumradius` -- validated against a right triangle
      (circumcenter = hypotenuse midpoint, circumradius = hypotenuse/2) plus
      an equidistance check against all three vertices

### Conic sections (1 function)
- [x] `conic_classify` -- discriminant B²-4AC test (ellipse/parabola/hyperbola),
      validated against a circle, an ellipse, a parabola, and two hyperbolas

### 3D planes and lines (3 functions)
- [x] `plane_from_points`, `point_plane_distance`
- [x] `line3d_distance` -- skew-line shortest distance, with a parallel-lines
      fallback (point-to-line distance) rather than dividing by ~0

### Triangle angles (1 function)
- [x] `triangle_angle` -- law of cosines, validated against a 3-4-5 right
      triangle (90°) and an equilateral triangle (60°)

### Tests and examples
- [x] `tests/test_vectors.vani` -- 2D/3D point and vector construction/arithmetic
- [x] `tests/test_lines_polygons.vani` -- lines, segments, triangles, polygons,
      convex hull, closest pair
- [x] `tests/test_circles_conics_3d.vani` -- circumcircle, conic classification,
      3D planes/lines, triangle angle
- [x] `examples/convex_hull_demo.vani` -- builds a hull from a scattered point
      set and reports its area/perimeter
- [x] `examples/plane_and_conics_demo.vani` -- 3D plane distance, skew-line
      distance, conic classification

### Safety annotations
- [x] `#[bounded_stack(bytes=N)]` on all functions, budgets set to `vanic
      check`'s exact reported worst-case (largest: `convex_hull` at 648 bytes,
      since it composes several point-arithmetic calls per loop iteration)

### Manual sort workaround
- [x] `_sort_points_lex` -- private in-place insertion-sort helper for
      `Vec<Point2D>` by (x, then y), needed because the builtin `sort`/`sort_by`
      only support `Vec<i64>`/`Vec<f64>` as of vani-compiler MATH-2 (unfixed).
      Revisit once MATH-2 lands.

---

## Future

No v0.2.0 is currently planned. Candidates if a concrete need shows up:
O(n log n) divide-and-conquer `convex_hull`/`closest_pair_distance` for large
point sets, 3D convex hull, polygon-polygon intersection/union (clipping),
Delaunay triangulation, and degenerate-input handling (collinear points for
`circumcenter`, fewer-than-3-point `convex_hull`) that currently returns a
meaningless result rather than erroring.
