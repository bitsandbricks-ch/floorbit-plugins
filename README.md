# Floorbit plugins

[![Publish the Marketplace](https://github.com/bitsandbricks-ch/floorbit-plugins/actions/workflows/publish.yml/badge.svg?branch=main)](https://github.com/bitsandbricks-ch/floorbit-plugins/actions/workflows/publish.yml?query=branch%3Amain) [![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-bitsandbricks-ffdd00?logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/bitsandbricks)

The official Marketplace of [Floorbit](https://github.com/bitsandbricks-ch/floorbit): a signed
`index.json` at **https://bitsandbricks-ch.github.io/floorbit-plugins/index.json**, which every
Floorbit trusts out of the box. The plugins are signed with the Floorbit plugin key
(`floorbit/keys/plugins/`), the index with the Floorbit index key (`floorbit/keys/index/`). See
Floorbit's [docs/marketplace.md](https://github.com/bitsandbricks-ch/floorbit/blob/master/docs/marketplace.md).

## How it works

- Every plugin lives in its own repository, `bitsandbricks-ch/floorbit-plugin-<id>`. Pushing a tag
  `v<version>` there tests the plugin, signs it and attaches `<id>-<version>.tar.gz` to the GitHub
  release. The same signed file is what the local agents install.
- [`plugins.yaml`](plugins.yaml) finds those repositories: every public `floorbit-plugin-*`
  repository of bitsandbricks-ch is included by itself. The
  [publish workflow](.github/workflows/publish.yml) reads their releases every hour (and when this
  repository changes or a plugin is released), checks each archive's signature, publishes the
  archives and the signed index with GitHub Pages, and then downloads them again and checks them
  the way a Floorbit installing them does.
- Every release is a version admins can install, update to or go back to, with its release notes.
  A version is withdrawn by adding `withdrawn: {"1.2.0": "why"}` to its repository's entry in
  `plugins.yaml`, never by deleting the release: Floorbit warns everyone who runs it, and never
  installs it again.

| Plugin | Repository |
| --- | --- |
| LAN cache | [floorbit-plugin-lancache](https://github.com/bitsandbricks-ch/floorbit-plugin-lancache) |
| Shelly power meters | [floorbit-plugin-shelly](https://github.com/bitsandbricks-ch/floorbit-plugin-shelly) |
| Home Assistant power | [floorbit-plugin-home-assistant](https://github.com/bitsandbricks-ch/floorbit-plugin-home-assistant) |

## Floorbit's own rules list

The same workflow builds **https://bitsandbricks-ch.github.io/floorbit-plugins/applications.json**,
the games, launchers and site groups every Floorbit knows, from [`games.yaml`](games.yaml). It is
signed with the index key, and Floorbit lists it as its last rule source, on by default. Floorbit's
[docs/applications.md](https://github.com/bitsandbricks-ch/floorbit/blob/master/docs/applications.md)
says how it is used.

To add or change a game, open a pull request on `games.yaml`:

- Add only what you have checked against real traffic or the publisher's own documentation, and say
  in the pull request where it comes from. Nothing made up, nothing guessed.
- Use the `id` of a built-in entry to replace it; a new `id` adds one.
- The build checks every entry the way a Floorbit checks a rules file. One bad entry stops it, and
  the last published list stays.

## Setting it up (once)

- **Pages:** Settings > Pages > Source: *GitHub Actions*.
- **Environment `plugin-signing`:** the secret `FLOORBIT_INDEX_SIGNING_KEY` (the private key of
  Floorbit's `floorbit/keys/index/index.pub`). Limit the environment to the branch `main`.

The tools come from Floorbit itself at the commit in [`FLOORBIT_REF`](FLOORBIT_REF), so the index is
built and checked by the code that installs from it.

## Licence

TODO: Mike: which licence for the Marketplace and the plugins?
