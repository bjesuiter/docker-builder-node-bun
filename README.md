# docker-builder-node-bun

A docker builder imager with node and bun, based on ubuntu

---

# Changelog

## 1.3.0 - 2024-12-19

- update bun to 1.1.40 to be able to use text-based (aka non-binary) lockfile
- update node to 22.12.0

## 1.2.1 - 2024-10-14

- update bun to 1.1.33
- update node to 22.10.0
- update git to 2.43.0

## 1.2.0 - 2024-10-14

- add git to the image

## 1.1.2 - 2024-10-14

- Make sure the bun paths are available without sourcing the bashrc (since gitlab probably won't do that when using the image to run builds)

## 1.1.1 - 2024-10-14

- Add linux/amd64 build to the existing linux/arm64 build

## 1.1.0 - 2024-10-14

- Change workdir for the resulting image to /workdir

## 1.0.0 - 2024-10-14

- Initial Release with Node 22 and bun 1.1.30
