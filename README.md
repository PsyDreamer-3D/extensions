# PsyDreamer-3D Blender Extensions

One Blender extension repository for PsyDreamer-3D's add-ons: add it once and Blender installs and
updates all of them.

## Add it to Blender

1. **Edit → Preferences → Get Extensions → Repositories → + → Add Remote Repository**
2. URL: `https://psydreamer-3d.github.io/extensions/index.json`
3. Turn on **Check for Updates on Startup**.

The add-ons then appear under **Get Extensions**. A page listing them, with links you can drag into
Blender, is at <https://psydreamer-3d.github.io/extensions/>.

## What's in it

The add-ons named in [`extensions.txt`](extensions.txt), each at its latest GitHub Release:

- [Poser Tools](https://github.com/PsyDreamer-3D/poser_tools)
- [Weight Paint Armature Tools](https://github.com/PsyDreamer-3D/wp_armature_tools)
- [Archimesh (PD3D)](https://github.com/PsyDreamer-3D/archimesh_pd3d)

Each add-on's own minimum Blender version comes from its manifest, so Blender only offers the
add-ons your version can run.

## How it's published

[`publish.yml`](.github/workflows/publish.yml) downloads the extension `.zip` from each listed
add-on's latest release, runs Blender's `extension server-generate --html` over them, and publishes
the zips, `index.json` and `index.html` to GitHub Pages. It runs daily, when `extensions.txt` or the
workflow changes, and on demand (**Actions → Publish extensions index → Run workflow**).

**Adding an add-on:** its repo must be public and its releases must carry the extension `.zip` (the
release workflow from `blender-addon-scaffold` does both the build and the upload). Add its
`owner/name` to `extensions.txt`.

**Publishing right away:** a new release otherwise appears within a day. To publish at once, an
add-on's release workflow can end with a step that triggers this one. It needs a fine-grained
token with **Contents: read and write** on this repository, stored in the add-on repo as the secret
`EXTENSIONS_DISPATCH_TOKEN`:

```yaml
      - name: Republish the PsyDreamer-3D extensions index
        env:
          GH_TOKEN: ${{ secrets.EXTENSIONS_DISPATCH_TOKEN }}
        run: gh api repos/PsyDreamer-3D/extensions/dispatches -f event_type=extension-released
```
