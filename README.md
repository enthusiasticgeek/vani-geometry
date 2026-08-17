# vani-geometry

Computational and analytic geometry library for the [vāṇी compiler](https://github.com/enthusiasticgeek/vani-compiler).

`Point2D`, `Point3D`, and `Plane` are plain structs with only `f64` fields --
no heap-owning fields, so they're freely copyable, like
[vani-complex](https://github.com/enthusiasticgeek/vani-complex)'s `Complex`.
Functions that take a *collection* of points use `ref Vec<Point2D>`
(read-only) or `mut ref Vec<Point2D>` (in-place sort), matching the
`Vec<f64>` convention established in
[vani-matrix](https://github.com/enthusiasticgeek/vani-matrix)/[vani-optimize](https://github.com/enthusiasticgeek/vani-optimize).

**API reference / tutorial:** <https://enthusiasticgeek.github.io/vani-geometry/>

## Add to your project

```toml
# vani.toml
[deps]
geometry = { registry = "kosh", version = "^0.1" }
```

```sh
vanic add geometry
vanic build
```

## What's included (v0.1.0 — complete; see TODO.md)

| Module | Functions |
|---|---|
| 2D point/vector construction & arithmetic | `point2d_new`, `point2d_add`, `point2d_sub`, `point2d_scale`, `point2d_distance`, `point2d_midpoint` |
| 2D vector operations | `vec2d_dot`, `vec2d_cross`, `vec2d_norm`, `vec2d_normalize`, `vec2d_angle_between` |
| 3D point/vector construction & arithmetic | `point3d_new`, `point3d_add`, `point3d_sub`, `point3d_scale`, `point3d_distance` |
| 3D vector operations | `vec3d_dot`, `vec3d_cross`, `vec3d_norm`, `vec3d_normalize` |
| 2D lines & segments | `line2d_point_distance`, `segment_point_distance`, `segments_intersect`, `segment_intersection_point` |
| Triangles & polygons | `triangle_area`, `polygon_area`, `polygon_perimeter`, `polygon_centroid`, `point_in_polygon` |
| Computational geometry | `convex_hull` (Andrew's monotone chain), `closest_pair_distance` (brute force) |
| Circles | `circumcenter`, `circumradius` |
| Conic sections | `conic_classify` (ellipse/parabola/hyperbola by discriminant) |
| 3D planes & lines | `plane_from_points`, `point_plane_distance`, `line3d_distance` (skew lines) |
| Triangle angles | `triangle_angle` (law of cosines) |

## A note on algorithmic complexity

`convex_hull` and `closest_pair_distance` use straightforward O(n²) sorting/
comparison rather than the asymptotically optimal O(n log n) divide-and-conquer
algorithms -- fine for the modest input sizes this library targets. A future
version could add faster variants if a real need for large n shows up (see
TODO.md).

## Encoding

```
struct Point2D { x: f64, y: f64 }
struct Point3D { x: f64, y: f64, z: f64 }
struct Plane   { nx: f64, ny: f64, nz: f64, d: f64 }   // nx*x + ny*y + nz*z + d = 0
```

`Vec<Point2D>` works directly (structs without heap-owning fields nest into
`Vec<T>` with no special handling needed) -- see `convex_hull`, `polygon_area`,
and friends, which all take `ref Vec<Point2D>`.

## What this library does NOT provide

These are already vāṇी compiler builtins — call them directly, no import needed:

`abs` `sqrt` `sin` `cos` `tan` `atan2` `acos` `asin` `f64_hypot` `f64_pi()`
`push` `pop` `len` `set`

## License

MIT
