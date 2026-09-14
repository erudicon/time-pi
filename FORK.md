# About This Fork

This is a fork of [geerlingguy/time-pi](https://github.com/geerlingguy/time-pi),
adapted for hardware that differs from upstream's reference build.

Changes here are **not** intended for upstream. They are hardware-specific and
would not be useful to most users of the original project.

## Branches

| Branch | Purpose |
|---|---|
| `master` | Clean mirror of `upstream/master`. Never commit here. |
| `i226-f10t` | All fork-specific work. This is the working branch. |

Last rebased onto upstream commit `3764cc8`
("Fixes #38: Make Chrony Dashboard a little more flexible for other users").

## Hardware

| Component | Upstream | This fork |
|---|---|---|
| Computer | Raspberry Pi 5 model B | Raspberry Pi 5 model B |
| GNSS | u-blox ZED-F9T on TimeHAT V4 | simpleRTK (u-blox F10T) |
| NIC | Intel i226-LM on TimeHAT V4 | Intel i226-LM on GeeekPi P02 PCIe adapter |
| Antenna | Active GPS antenna | SparkFun L1/L5 multi-band active antenna |

The key difference is that the NIC is a **discrete PCIe card** rather than part
of a combined TimeHAT, so it enumerates differently and needs its own
configuration.

## What's changed

- **Discrete Intel i226 support** — <!-- TODO: what specifically? interface
  naming, driver/module options, IP configuration, link speed -->
- **pps-tools installed** — added to the playbook; needed for PPS verification
  on this build.
- **simpleRTK F10T configuration** — <!-- TODO: baud rate, PROTVER, ubxtool
  settings that differ from upstream's F9T -->
- **README.md** — fork-specific hardware, links, and attribution. Formatting is
  deliberately kept identical to upstream to minimize rebase conflicts; see
  `.markdownlintignore`.
- **YAML lint / config updates** — <!-- TODO: keep or drop? These conflict on
  every sync and provide no fork-specific value -->

## Keeping up with upstream

```bash
git fetch upstream
git switch master
git merge --ff-only upstream/master
git push

git switch i226-f10t
git rebase master
git push --force-with-lease
```

Resolve conflicts by looking at both sides — do not blanket-resolve with
`--ours` or `--theirs`, which has previously dropped upstream documentation
without it being noticed.

`git config --global rerere.enabled true` makes repeated conflicts resolve
themselves after the first time.

After rebasing, update the "last rebased onto" commit above.

## Contributing back

If a change here turns out to be generally useful (a real bug fix, or a
refactor that makes hardware support more flexible), branch it separately off a
fresh `master` and open a PR upstream:

```bash
git fetch upstream
git switch -c fix-thing upstream/master
git cherry-pick <sha>
git push -u origin fix-thing
```

Every change upstream accepts is one less to carry here.