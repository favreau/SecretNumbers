# Happy Taxi — Game Recreation Prompt

Use this document to rebuild **Happy Taxi** in any stack (Unity, Godot, Flutter, native mobile, Three.js, etc.).

---

## One-line pitch

**Happy Taxi** is a top-down driving game where you learn real-world cities by driving their actual streets. OpenStreetMap provides the map; optional police patrols add light pressure without turning the game into a crime simulator.

---

## Core fantasy

You are a driver exploring a real place. The goal is **spatial learning**: internalize the layout of a town by moving through it. Police exist as ambient pressure — they patrol, notice you if you enter their view, and chase — but the product is not “be a criminal.” It is “get to know the map.”

---

## Primary loop

1. Choose a real city (or area).
2. Download road (and optional building) data for that area.
3. Spawn on a real road, correctly oriented along the street.
4. Drive freely; learn junctions, districts, landmarks.
5. Sometimes attract police attention and escape or get caught.
6. Restart / pick another city and repeat.

---

## Features to implement

### Map & world

- **City selection** via geocoding (e.g. Nominatim).
- **Road network** from OpenStreetMap (Overpass or preprocessed extracts):
  - highway types: motorway, trunk, primary, secondary, tertiary, residential, unclassified, etc.
  - store road **name** when available for HUD (“current street”).
- **Buildings** (optional but desirable): footprints + height from `building:levels` / `height`, rendered as simple 2.5D extrusions (walls + roof) in top-down view.
- **Satellite basemap** (optional): tile imagery under vector roads (e.g. Esri World Imagery or any satellite tile provider).
- **Area size**: prefer full city bbox for small/medium towns; clamp very large cities to a playable window (~10–15 km) for performance.
- **Coordinate system**: local metric projection from a city center (equirectangular or Web Mercator meters).

### Driving

- Top-down car with accelerate / brake / reverse / steer.
- **Modest acceleration**; readable speed (not arcade-hyper).
- **Strong steering** even at low speed.
- **Steering assist**: gently align to road direction and pull toward lane center; weaker when player is actively steering.
- **Stay on roads**: constrain vehicle to road corridors (half-width per highway class). If off-road, push back toward nearest centerline and damp speed.
- **Camera**:
  - follows car smoothly;
  - **auto-zoom by speed** (faster → higher altitude / zoom out);
  - optional pinch/manual zoom multiplier;
  - **vehicle sprite scale follows zoom** (“altitude”).

### Police (secondary system)

- Spawn **2** (configurable) police cars on roads at **distinct** positions, not stacked, reasonably far from the player.
- **Patrol mode**: wander between random road waypoints; follow roads tightly; no siren.
- **Detection**: if player is in view range (~100 m) and inside a forward sight cone (~90°), or very close, switch to **chase**.
- **Chase mode**: pursue player with light prediction; siren / visual alert on; slightly higher speed than patrol.
- **Lose player**: if distance exceeds lose range (~170 m), return to patrol with a new waypoint.
- **Vehicle collisions**: solid separation (no overlapping). Multiple resolution passes per frame; damp relative speed on contact. Police–police collisions too.
- **Catch rule**: if a **chasing** police car stays too close (~24 m) for **10 continuous seconds** → **Game Over**.
- Show a countdown/alert in HUD while proximity timer is active.
- On Game Over, freeze controls; allow restart (e.g. key **R**) which respawns player and police.

### UI / UX

- City text field + **Load** button.
- Loading **modal** with progress stages:
  - geocode city
  - download OSM
  - parse roads / buildings
  - bake / build collision network
  - spawn entities
- HUD:
  - speed
  - current street name
  - optional counts (roads, buildings)
  - police alert timer when relevant
- **Minimap** (corner): roads only + player + police dots; local radius around player.
- Mobile: on-screen steer + accel/brake pads; safe areas; prevent browser scroll/zoom stealing touches.
- Desktop: keyboard (arrows / WASD-style).

### Performance notes

- Prefer **baking** roads (and buildings) to an offscreen texture / mesh once after load; draw as a single image/mesh each frame.
- Simplify polylines.
- Limit tile DPR on mobile.
- Spatial sampling when searching nearest segments if the network is huge.

---

## Feel targets

- Calm exploration first; tension only when seen by police.
- Roads feel sticky and readable; player rarely “falls off” the graph.
- Police feel present in the city, not psychic: **line-of-sight / proximity** starts the chase.
- Learning emerges from repetition: same real junctions, names, and shapes.

---

## Non-goals

- Full GTA feature set (missions, economy, gunplay, multiplayer).
- Perfect driving physics or lane-level traffic rules.
- Guaranteed global coverage of huge metros in one download (must chunk or clamp).

---

## Suggested tech mappings

| Concern | Browser prototype | Engine rebuild |
|--------|-------------------|----------------|
| Map data | Overpass + Nominatim | Same, or offline PBF/GeoJSON |
| Render | Canvas 2D / WebGL | Sprites, tilemaps, or 3D top-down |
| Roads | Polylines + segment list | Nav graph + ribbon meshes |
| Buildings | Extruded polygons | Boxes / simple meshes |
| AI | State machine patrol/chase | Same on nav graph (pathfollow) |
| Collisions | Circle/capsule separation | Physics engine triggers |

---

## Acceptance checklist

- [ ] Load a real city by name and drive on its OSM roads  
- [ ] Street name updates on HUD when available  
- [ ] Car stays on road network with assist  
- [ ] Zoom reacts to speed; car size scales with zoom  
- [ ] Police patrol until they see the player, then chase  
- [ ] No vehicle overlapping; push-apart on contact  
- [ ] 10 s close chase → Game Over; restart works  
- [ ] Works on mobile with touch controls  

---

## Product name

**Happy Taxi**

Tagline options:

- *Learn any city by driving it.*
- *Apprends la ville en la conduisant.*

---

## Reference prototype behavior (historical)

An early HTML5 Canvas prototype used:

- Nominatim geocoding  
- Overpass for highways + buildings  
- Esri satellite tiles  
- Local metric projection  
- Pre-baked road/building canvases  
- Segment-based nearest-road constraint  
- Two police cars with patrol/chase states  

Rebuilds should preserve the **design intent** above even if implementation details change.
