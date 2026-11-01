# THUG2 THPS4 Level Converter

> **Not affiliated with or endorsed by Activision, Neversoft or any other rights holder.** A non-commercial fan project
> for preservation, interoperability and education. No game files are included: the tools work only on copies of the
> games you own. Game names are trademarks of their owners. See LICENSE (MIT for the project's own work; no licence for material
> from the games); third-party parts (downloaded tools) are listed in THIRD_PARTY_NOTICES.md. Rights holders: open an
> issue and it will be dealt with promptly.

Converts every THPS4 level with a scene, with its scripts, people and cars from your own copy of Tony Hawk's Pro Skater 4 (PC) to the formats of Tony Hawk's Underground 2 (PC), for its level
select. Nothing of either game is in this repository: the files are made from your copies each time. How it works,
what is converted and what isn't: [thps4_levels/README.md](thps4_levels/README.md).

- `thps4_levels/` - the converter (`thps4_levels.ts`: `convert`, `selfcheck`, `list`), its readers of THPS4's files and scripts and the THUGPRO comparisons.

## Dependencies

Each pinned to a tagged commit in package.json (`git+https://git@github.com/...#<commit>`; one copy of each: every repository that uses one pins the same commit):

- [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core): THUG2's formats (scenes, collision, textures, archives, scripts), the level port's common steps. THPS4's own formats are read here.
- [thug2-thugpro-compat](https://github.com/CanuckAussieKev/thug2-thugpro-compat): the THUGPRO compatibility profile (`--compat`): the register's THPS4 data - level names, network nodes, node edits, THUGPRO's own content by reference - and its port changes; `thps4_levels/thugpro_compare.ts --extract-thugpro` rewrites it (in a clone of that repository).

## Public API

None: nothing here is imported by another repository (package.json exports no module). The converter is used through its commands.

Commands (package.json's `commands`: the tools with a command line - importing a module never runs one): `thps4_levels/migrate_layouts.ts`, `thps4_levels/thps4_levels.ts`, `thps4_levels/thugpro_compare.ts`, `thps4_levels/thugpro_inventory.ts`.

## Usage

Needs Node 24 and NeverScript's `ns.exe` (QB scripts compiled): the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) builds it (`tools\build_neverscript.ps1` ->
`out\neverscript\ns.exe`, found there when this converter is used inside it) - or pass `--ns <ns.exe>`.

    git clone https://github.com/CanuckAussieKev/thug2-thps4-level-converter
    cd thug2-thps4-level-converter
    npm ci
    node thps4_levels/thps4_levels.ts convert Alc --thps4 "C:\Games\THPS4" --thug2 C:\Games\THUG2 --ns <ns.exe>
    node thps4_levels/thps4_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thps4\` (not read by the game); without level names it converts the default
set (thps4_levels/README.md). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thps4_levels/thps4_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it;
`package.test.ts` checks package.json's exports, commands and pins). GitHub Actions runs them on every push
(`.github/workflows/ci.yml`).

## Versions

Releases are tagged `v<major>.<minor>.<patch>`; this is `v1.1.0` (THUGPRO's profile from thug2-thugpro-compat; core v2.0.0). The repositories that use it pin the tagged commit.
