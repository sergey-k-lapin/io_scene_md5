# io_scene_md5

Blender add-on for importing and exporting the id Tech 4 MD5 model format
(`.md5mesh` and `.md5anim`), as used by Doom 3, Quake 4, Prey (2006),
Wolfenstein (2009), and The Dark Mod.

Originally written by nemyax for Blender 2.7x; ported to **Blender 4.0+**.

## Installation

1. Download `io_scene_md5.py`.
2. In Blender, go to **Edit > Preferences > Add-ons > Install...** and select
   the file.
3. Enable the add-on ("id tech 4 MD5 format").

## Usage

All entries are under **File > Import** / **File > Export**.

### Import

- **MD5 Mesh (.md5mesh)** — imports a mesh with its skeleton. The skeleton is
  created as a new armature named `MD5`, and all imported bones are
  automatically marked for MD5 export. Options: *Reorient* (rotate the model
  to face Blender's -Y forward convention) and *Scale*.
- **MD5 Animation (.md5anim)** — imports an animation onto the **active
  armature**, which must match the skeleton in the file (same bones and
  hierarchy). The animation is keyed starting at the current frame, and
  `*_start` / `*_end` timeline markers are created for it. These markers are
  used by the batch exporter.

### Export

First select the armature-deformed mesh(es) you want to export.

- **MD5 Mesh (.md5mesh)** — exports the selection as a mesh file.
- **MD5 Animation (.md5anim)** — exports the current scene frame range as an
  animation file.
- **MD5 (batch export)** — exports the mesh plus one `.md5anim` per
  `<name>_start` / `<name>_end` timeline marker pair. The optional *Marker
  filter* limits export to markers whose names start with the given text.

Export options: *Reorient* (treat +X as the forward direction) and *Scale*.

### Marking bones for export

Only bones marked for MD5 export are included in exported files. To manage
the marked set, use the **MD5 Export Setup** panel in
**Properties > Scene**, with bones selected in **Edit** or **Pose** mode:

- **Add Selected** / **Remove Selected** — change the set.
- **Replace with Selected** — make the selection the entire set.
- **Clear All** — empty the set.

Internally the flag is stored as a custom property (`md5`) on each bone.
(Blender versions before 2.80 used a reserved armature bone layer instead;
that system was removed from Blender.)

### Export requirements and checks

The exporter validates the selection and reports errors for:

- no armature-deformed meshes selected,
- meshes bound to different armatures,
- no bones marked for MD5 export,
- multiple root bones, or bones whose parents are not marked,
- vertices with no (or zero) deformation weights,
- meshes without UV coordinates.

## Format notes

- Files are written as `MD5Version 10`.
- Only one armature and one root bone per export; multi-mesh exports share
  the same skeleton.
- Vertex weights are normalized on export.

## License / credits

- Original add-on: nemyax (Blender 2.7x era).
- Blender 4.x port: see repository history.
