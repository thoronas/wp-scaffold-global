---
name: wp-feature
description: Build a new WordPress feature with full security, hook registration, and WordPress Coding Standards compliance. Use when the user asks to add a feature, create a settings page, register a post type, build a REST endpoint, or implement any new functionality.
---

## Step 1 — Triage

Before planning or writing any code, assess the feature against the checklist below.
For each signal that applies, name the relevant skill and ask whether to invoke it
as part of this build. If the feature is clearly simple and self-contained (e.g. a
single settings field, a one-hook filter) and no signals fire, skip straight to Step 2.

| Signal | Relevant skill |
| ------ | -------------- |
| Interactive UI — `data-wp-*` directives, client-side state, reactive blocks | `/wp-interactivity-api` |
| WP-CLI command, db operation, batch script, or ops automation | `/wp-wpcli-and-ops` |
| Performance concern — heavy queries, N+1 risk, caching strategy, HTTP calls | `/wp-performance` |
| Static analysis setup or PHPStan baseline needed | `/wp-phpstan` |
| New Gutenberg block (edit/save components, `block.json`, render callback) | `/wp-block` |
| Needs a WordPress Playground demo or blueprint for testing/review | `/wp-playground` or `/blueprint` |
| Plugin will be submitted to WordPress.org | `/wp-plugin-directory-guidelines` |
| UI uses or should use the WordPress Design System | `/wpds` |
| Feature touches the Abilities API (permissions, capability registration) | `/wp-abilities-api` |
| Unfamiliar repo — need to map structure before building | `/wp-project-triage` |

If one or more signals fire, present them and ask: **"Before I build, should I invoke
[skill] to [specific reason]?"** Wait for approval. If the user says yes, invoke the
skill now. If no, note it and continue.

---

## Step 2 — Plan

Determine:

1. **Which package owns this feature** — theme (`themes/[name]/`) or plugin (`plugins/[name]/`)? Follow the boundary rules: themes own presentation, plugins own data and logic. Check PROJECT-SPEC.md if a theme/plugin boundary table exists.
2. **What** the feature does (one sentence)
3. **Where** it lives — admin, frontend, REST, CLI, or block
4. **Who** uses it — which WordPress capability/role
5. **Which hooks** it attaches to
6. **What data** it reads or writes — options, meta, custom tables, transients

If reference material exists in `.claude/reference/` that is relevant to this feature,
read it first and apply its patterns where appropriate.

Then produce, in order:

1. **Package and file placement** — which theme or plugin directory this goes into, following the conventions in CLAUDE.md
2. **Hook registration map** — every `add_action` / `add_filter` with priority and justification
3. **Data flow** — user action → hook → service → repository → database → response
4. **Security checklist** — nonce verification, capability check, sanitization functions, escaping functions, `$wpdb->prepare()` usage
5. **Complete implementation** — WordPress Coding Standards compliant, no stubs
6. **Enqueue calls** — `wp_enqueue_script` / `wp_enqueue_style` with correct dependencies, conditional loading, and correct text domain for the owning package
7. **Edge cases** — what happens on multisite, when options don't exist, when the user lacks permissions, when expected data is missing
8. **Uninstall cleanup** — what gets removed via `register_deactivation_hook` or `uninstall.php`

---

## Step 3 — Implement

**WPCS enforcement (mandatory):**
Run `composer phpcbf` on every PHP file you create to auto-fix standards violations,
then run `composer phpcs` to verify zero remaining violations. If any manual fixes are
needed, apply them and re-run until clean. For large features with multiple PHP files,
delegate final validation to the `wpcs-enforcer` subagent.

After delivering, tell the user to check:
- All input sanitized, all output escaped
- Nonces on every form and AJAX/REST handler
- Capability checks use the correct capability for the role
- Text strings use the correct text domain for the package (theme vs plugin)
- Feature is in the correct package (presentation in theme, logic in plugin)
