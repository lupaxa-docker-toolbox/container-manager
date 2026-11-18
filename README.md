<p align="center">
  <a href="https://github.com/lupaxa-docker-toolbox">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/docker-toolbox/readme-logo.png" alt="The Lupaxa Project: Docker Toolbox" />
  </a>
</p>

<h1 align="center">Container Manager</h1>

Interactive menu for Docker images and containers on this machine. It lists what is local, including images you pulled and images you built, and it removes or prunes them after confirmation.

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
 1  disk usage
 2  list images
 3  list all containers
 4  list running containers
 5  list stopped containers
 6  list dangling images
 7  stop a container
 8  remove a container
 9  remove all containers
10  remove an image
11  remove all images
12  prune stopped containers
13  prune dangling images
14  prune unused images
15  prune unused data
 Q  quit
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
| 7      | Stops one running container. You type the name or ID.                                                                                    |
| 8      | Removes one container. A running container is left alone.                                                                                |
| 9      | Lists every container, then removes them all, including ones that are running.                                                           |
| 10     | Removes one image. An image a container still uses is left in place.                                                                     |
| 11     | Lists every image, then removes them all, including ones a container is using.                                                           |
| 12     | Lists stopped containers, then removes them.                                                                                             |
| 13     | Lists dangling images, then removes them.                                                                                                |
| 14     | Removes unused images, including tagged images no container uses.                                                                        |
| 15     | Removes stopped containers, unused networks, dangling images, and the build cache. Volumes stay.                                         |
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

Stop every container, then delete the containers and everything linked to them: images, volumes, unused networks, and the build cache. Predefined networks (`bridge`, `host`, `none`) stay.

> **Note:** This cannot be undone. Named volumes go too.

```bash
docker ps -q | while read -r id; do docker stop "${id}"; done
docker ps -aq | while read -r id; do docker rm -f "${id}"; done
docker images -aq | while read -r id; do docker rmi -f "${id}"; done
docker volume ls -q | while read -r id; do docker volume rm "${id}"; done
docker network prune --force
docker builder prune --all --force
```

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
