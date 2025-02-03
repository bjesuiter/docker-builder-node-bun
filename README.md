# docker-builder-node-bun

A docker builder imager with node and bun, based on ubuntu

## Current Settings

- deployed at: https://hub.docker.com/repository/docker/bjesuiter/docker-builder-node-bun/tags

## Deployment of new Image

1. Check Versions of tool in the image: run bun dev-rebuild
   1. bun -v
   2. node -v
   3. git --version
2. Update version number in package.jsons 'config' key
3. If not already done, login to docker hub via `docker login` (to the bjesuiter account)
4. Run `bun prod-buildpush` => builds and pushes directly for linux amd64,arm64 and mac

## Instructions - Read image from private registry

```bash
# The use of printf (as opposed to echo) prevents encoding a newline in the password.
# If your username includes special characters like @, you must escape them with a backslash (\) to prevent authentication problems.
# bjesuiter: is the username, my_password is the bjesuiter personal access token
echo -n 'bjesuiter:my_password' | base64

# Example output to copy
bXlfdXNlcm5hbWU6bXlfcGFzc3dvcmQ
```

Step 2 - Create docker config

```
  {
      "auths": {
          "https://index.docker.io/v1/": {
              "auth": "(Base64 content from above)"
          }
      }
  }
```

Step 3: Add this to DOCKER_AUTH_CONFIG
Step 4: simply add your private image to your job definition in your gilab-ci.yml file. If its not working, check the following:

- which method do you use for base64 encoding? => might be URL standard or something, we need pure base64 here
- is the registry url correct? => should be https://index.docker.io/v1/ for docker hub (as of 2024-10-15)
- is your auth token correct? (which you used instead of a password)

---

# Changelog

## 1.4.0 - 2025-02-03

- update bun to 1.2.2
- update node to v22.13.1

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

---

# Repo Log (reverse-chronological)

## 2024-10-14: Research - best base image for running node?

## Option 1: oven/bun:1.1-debian

- Problem: installing node on it is complicated

## Option 2: Alpine

- only good when installed from apk package manager
- alpine3.20.3 installs nodejs version 20.15.1 (which is the older LTS version at the time of writing, current LTS is 22.x)
- trying to install nodejs via fnm fails, probably because alpine uses musl instead of glibc, which prevents the node binaries installed by fnm from executing

## Option 3: Ubuntu (Works)

- use 24.04 as a base image
- install nodejs via deb.nodesource.com: https://github.com/nodesource/distributions/blob/master/README.md#installation-instructions-deb
- install bun via https://bun.sh/install
