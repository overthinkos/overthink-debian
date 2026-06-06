# overthink-debian — native DEB packaging for `ov` (Debian + Ubuntu)

`debian/` builds the `overthink` `.deb` (the `ov` CLI at `/usr/bin/ov`) for
Debian and Ubuntu. ONE source serves both (Ubuntu inherits Debian's `deb`
format).

Consumed two ways, both through the SAME
`build.yml distro.debian.format.deb.local_pkg.build_template`:

- **localpkg deploy** — `ov deploy` / `ov update` / `ov eval run` to a `target:
  vm` (or `target: local`) Debian/Ubuntu target builds this `.deb` on the host
  (in a debian container) and `apt-get install`s it onto the target,
  auto-resolving the mandatory deps.
- **release artifacts** — `task pkg:debian` builds a downloadable `.deb` into
  `dist/`.

The build_template copies a prebuilt `ov` binary into the source tree as `./ov`
(installed via `debian/overthink.install`) and rewrites `debian/changelog`'s
version to the binary's CalVer, so `dpkg -s overthink` matches `ov version` and
every build is upgradeable.

**Tailscale is intentionally NOT a `Depends:`** — it is not in Debian main. The
`ov` layer (`candy/ov/candy.yml`) supplies tailscale for Debian/Ubuntu via the
tailscale apt repo. All other mandatory deps are in Debian/Ubuntu main and
auto-resolve.

History lives in the superproject's `CHANGELOG.md`.
