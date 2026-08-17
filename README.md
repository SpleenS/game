# game — per-host IaC

Unlike helm/edge/micro, `game` has **no `/var/mnt/game` NFS export**, so this
repo is rooted directly at `~/.config/containers/systemd/` — the real, live
quadlet directory systemd reads from. Nothing is copied here, so there is no
source-of-truth drift: what is committed is literally what systemd runs.

Not covered by this repo (deliberate, worth knowing):

- Wings config (`/etc/pterodactyl/config.yml`) — root-owned and contains real
  secrets/tokens.
- Game server data — lives under Wings' own data dir; application state rather
  than config.

`git` on this host is a container wrapper (`/usr/local/bin/git`), not a real
package: game runs MicroOS with a read-only root, so installing git would need
a `transactional-update` plus a reboot. See the `git-ops` skill.
