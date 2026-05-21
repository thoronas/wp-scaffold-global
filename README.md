# wp-scaffold-global

Machine-level Claude Code configuration for WordPress development. Clone once per developer machine, run `install.sh` to symlink skills, agents, and rules into `~/.claude/`. Edits to this repo are reflected immediately — no re-install needed.

This is one half of a two-repo scaffold:

- **`wp-scaffold-global`** (this repo) — skills, agents, rules. Install once per machine.
- **`wp-scaffold-project`** — Claude layer template + `bin/init.sh`. Bootstrap once per project.

## Installation

```bash
git clone git@github.com:thoronas/wp-scaffold-global.git ~/Sites/wp-scaffold-global
cd ~/Sites/wp-scaffold-global
chmod +x install.sh
./install.sh
```

The script symlinks each skill directory, agent file, and rule file into `~/.claude/`. Claude Code picks them up automatically in every project.

## Contents

### Skills

Invoked in conversation with `/skill-name`. All PHP-generating skills run `phpcbf → phpcs` before presenting output.

| Skill | When to use |
| --- | --- |
| `/wp-feature` | Add a feature, settings page, post type, or REST endpoint |
| `/wp-block` | Create a Gutenberg block or block component |
| `/wp-debug` | Investigate a bug, error, or unexpected behavior |
| `/wp-migrate` | PHP/WP version upgrades, deprecation replacements, dependency swaps |
| `/wp-review` | Security-focused code review before merge |
| `/wp-theme` | Generate a full theme skeleton — FSE or hybrid |

### Agents

Spawned by Claude as specialized sub-processes.

| Agent | Purpose |
| --- | --- |
| `wpcs-enforcer` | Validate and fix all WPCS violations; text-domain-aware |
| `security-auditor` | Find security vulnerabilities; cross-references phpcs sniff names |
| `performance-profiler` | Find performance issues while maintaining WPCS compliance |

### Rules

Activate automatically by file glob — no manual invocation needed.

| Rule | Applies to |
| --- | --- |
| `php-standards` | `plugins/**/*.php`, `themes/**/inc/**/*.php`, `themes/**/functions.php` |
| `js-standards` | `**/assets/**/*.js`, `**/blocks/**/*.js{x}` |
| `template-standards` | `themes/**/templates/**/*.html`, `**/patterns/**/*.php` |
| `test-standards` | `tests/**/*.php` |

## Theme/Plugin Boundary

Themes own presentation. Plugins own data and logic.

Custom post types, taxonomies, REST endpoints, and business logic belong in plugins. Templates, styles, block patterns, and visual output belong in themes. Breaking this boundary is a data-loss risk if the theme ever changes.

## Updating

Pull new changes from this repo — symlinks mean `~/.claude/` updates automatically, no re-install required.

```bash
cd ~/Sites/wp-scaffold-global
git pull
```

## Related

- [`wp-claude-scaffolding`](https://github.com/thoronas/wp-claude-scaffolding) — per-project Claude layer and WordPress scaffold
