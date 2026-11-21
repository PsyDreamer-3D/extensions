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

**Publishing right away:** each add-on's release workflow ends with a step that triggers this one
(`repository_dispatch`, event `extension-released`), so a release is listed as soon as its zip is up.
That step uses the organization secret `EXTENSIONS_DISPATCH_TOKEN`; until the secret exists it prints
a notice and does nothing, and the daily run lists the release. The token is a fine-grained personal
access token owned by PsyDreamer-3D, with access to only this repository and **Contents: Read and
write**, stored as an organization secret the add-on repos can use:

```bash
gh secret set EXTENSIONS_DISPATCH_TOKEN --org PsyDreamer-3D --repos poser_tools,wp_armature_tools,archimesh_pd3d
```

(`gh` asks for the token's value.) A new add-on's repo has to be added to that secret's list too.
