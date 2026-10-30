# Floater marking

Select GaussianSplats in the Inspector. Under Floater Marking (Persistent):

- Force Opacity to 1 overrides each Gaussian opacity; Gaussian edge falloff remains.
- Filter by Opacity shows only Gaussians within Minimum Opacity and Maximum Opacity (0 to 1, inclusive).
- For example, 0 to 0.2 isolates low-opacity splats; 0.5 to 1 shows high-opacity splats.
- Filtering uses decoded original asset opacity, before Opacity Scale or Force Opacity to 1. Both options can be combined. Compression can affect values near a boundary.

In Play or Edit Mode, open Scene view and enable the renderer Inspector's Edit toggle.
Drag a rectangle to select Gaussian centers. Shift adds, Ctrl subtracts. Selection
includes overlapping centers at all depths. A plain click clears selection.
Choose Mark Color and press Save Color to Selected. Selection clears to reveal the saved color.
Use Restore Original Color of Selected to remove marks from selected splats.
Deselect (Show Saved Colors) only removes the temporary magenta selection highlight.

Settings and color marks are saved immediately in the shared GaussianSplatAsset,
so they persist after Play Mode and affect all renderers using that asset.
Original binary data and PLY exports are unaffected. Marks override SH color and
are bound to the data hash; recreated/changed GS data may invalidate them.
Marking resized geometry is disabled.

Overrides affect normal Splats rendering. Filtering also excludes hidden splats
from new rectangle, select-all, and invert selections. Changing filter settings
clears the current renderer selection. Color marking supports compressed assets;
the existing Very High quality warning concerns geometry manipulation tools.
GPU marking data costs 4 bytes per Gaussian. Saving performs synchronous selection
readback and can briefly pause large scenes.

Verified base marking workflow in Unity 2022.3.62f3 / Direct3D 12 with 408,338 splats:
Play Mode selection, 569 red marks, and persistence after leaving Play Mode.
Temporary test marks were removed.

## Gaussian outlines

Enable **Show Gaussian Outlines** in Floater Marking (Persistent).
**Outline Color** includes alpha; **Outline Width (px)** controls the inward
stroke (0.5 to 8 render-target pixels). **Outline Radius** controls the ellipse
size in Gaussian coordinates (default 1.8; the quad extends to 2).
Gaussians have no hard boundary, so this is a reference ellipse, not a mesh edge.
Very small splats can appear filled when the line is wider than their footprint.
Outlines follow splat scale and the opacity filter, work with forced opacity
and saved colors, and use the existing depth-sorted transparency compositing.
They do not show through fully opaque foreground splats. Color alpha is
independent of original splat opacity so low-opacity floaters remain inspectable.
Settings are stored in the shared asset and persist after Play Mode.
Only the normal Splats render mode is affected; PLY data is unchanged.
