<p align="center">
  <a href="https://github.com/lupaxa-docker-toolbox">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/docker-toolbox/readme-logo.png" alt="The Lupaxa Project: Docker Toolbox" />
  </a>
</p>

<h1 align="center">Container Manager</h1>

Interactive menu for Docker images, containers, and volumes on this machine. It lists what is local, including images you pulled and images you built, and it removes or prunes them after confirmation.

Destructive actions ask for confirmation in menu mode. Non-interactive flags mirror the menu. Flag mode never prompts.

## Requirements

- Docker on `PATH`
- Bash (macOS `/bin/bash` 3.2 or newer is fine)
- A terminal (the menu clears the screen and pauses after each command)

## Quick Start

With Homebrew:

```bash
brew tap the-lupaxa-project/tap
brew trust the-lupaxa-project/tap
brew install container-manager
```

Or clone the repository and run the script:

```bash
git clone git@github.com:lupaxa-docker-toolbox/container-manager.git
cd container-manager
./src/container-manager
```

To run a checkout from anywhere on `PATH`, copy or symlink into your personal `bin`:

```bash
cp /path/to/container-manager/src/container-manager ~/bin/container-manager
chmod +x ~/bin/container-manager
```

With no arguments the interactive menu opens. Press `Q` to quit.

```bash
./src/container-manager --help
```

`--help` prints the flags and exits 0. It does not require Docker. Any other invocation does: if `docker` is missing, the script prints `Error: docker was not found in PATH.` and exits 1 before showing the menu.

## Modes

**Menu mode** is the default. The screen clears, you pick a number, and after most commands you press Enter to return. Destructive options ask `[y/N]` first. The default answer is no.

**Flag mode** is one action flag per run. There is no menu, no pause, and no confirmation prompt. Destructive flags exit 2 unless you also pass `-y` or `--yes`.

## Menu

```text
Status
  1) disk usage

Lists
  2) list images
  3) list all containers
  4) list running containers
  5) list stopped containers
  6) list dangling images
  7) list volumes

Containers
  8) stop a container
  9) remove a container
 10) remove all containers

Images
 11) remove an image
 12) remove all images

Volumes
 13) remove a volume
 14) remove all volumes

Prune
 15) prune stopped containers
 16) prune dangling images
 17) prune unused images
 18) prune unused data

  Q) quit
```

## What Each Option Does

| Option | What it Does                                                                                                                             |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Shows how much disk images, containers, volumes, and the build cache use, and how much can be reclaimed.                                 |
| 2      | Lists local images, including images that were pulled and images that were built on this machine.                                        |
| 3      | Lists every container, including stopped ones.                                                                                           |
| 4      | Lists containers that are running.                                                                                                       |
| 5      | Lists stopped containers. A stopped-container prune removes these.                                                                       |
| 6      | Lists dangling images. These are untagged layers a dangling prune removes.                                                               |
| 7      | Lists every local volume.                                                                                                                |
| 8      | Stops one running container. You type the name or ID.                                                                                    |
| 9      | Removes one container. A running container is left alone.                                                                                |
| 10     | Lists every container, then removes them all, including ones that are running.                                                           |
| 11     | Removes one image. An image a container still uses is left in place.                                                                     |
| 12     | Lists every image, then removes them all, including ones a container is using.                                                           |
| 13     | Removes one volume. A volume a container still uses is left in place.                                                                    |
| 14     | Lists every volume, then removes them. A volume a container is still using is left in place.                                             |
| 15     | Lists stopped containers, then removes them.                                                                                             |
| 16     | Lists dangling images, then removes them.                                                                                                |
| 17     | Removes unused images, including tagged images no container uses.                                                                        |
| 18     | Removes stopped containers, unused networks, dangling images, and the build cache. Volumes stay.                                         |
| Q      | Leaves the menu.                                                                                                                         |

## Flags

```text
--df
--list-images
--list-containers
--list-running
--list-stopped
--list-dangling
--stop NAME
--remove-container NAME    requires --yes
--remove-all-containers    requires --yes
--remove-image NAME        requires --yes
--remove-all-images        requires --yes
--list-volumes
--remove-volume NAME       requires --yes
--remove-all-volumes       requires --yes
--prune-containers         requires --yes
--prune-images             requires --yes
--prune-unused-images      requires --yes
--prune                    requires --yes
-y, --yes
```

```bash
./src/container-manager --list-images
./src/container-manager --prune-images --yes
```

## Complete Purge

Remove every container, including ones that are running, then every image, then every volume,
then unused networks, dangling images, and the build cache. Each flag is one run. `--yes` is
required. A volume a container is still using is left in place, so the container step comes
first.

> **Note:** This cannot be undone. Volumes go too. Predefined networks (`bridge`, `host`, `none`) stay.

```bash
./src/container-manager --remove-all-containers --yes
./src/container-manager --remove-all-images --yes
./src/container-manager --remove-all-volumes --yes
./src/container-manager --prune --yes
```

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
