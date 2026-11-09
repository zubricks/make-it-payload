<img width="1200" height="375" alt="make-it-payload-header (1)" src="https://github.com/user-attachments/assets/de9176aa-3c80-485c-9b6e-247c9697bdce" />

# Make It Payload

An Agent Skill for **Payload 3 and 4**, reviewing and improving finished Payload plugins and custom Admin UI so they use the components, patterns, and behavior Payload already provides. If you're building with/inside of Payload this skill will help ensure that things are done the right way to save you time in the long run.

## Install

From the project where you want to use the skill:

```sh
npx skills add zubricks/make-it-payload
```

The [Skills CLI](https://github.com/vercel-labs/skills) lets you select a supported agent. To install specifically for Codex:

```sh
npx skills add zubricks/make-it-payload --agent codex
```

Add `--global` to install for your user rather than the current project. No npm package, build step, or Payload config change is needed for this skill.

## Use

In Codex, ask for a review without implementation changes:

```text
$make-it-payload check src/plugins/my-plugin
```

Or ask the agent to review, fix, and verify the integration:

```text
$make-it-payload src/plugins/my-plugin
```

You can scope it to another project, a feature, or a diff. With another skill-capable agent, use its skill invocation syntax or ask it to use make-it-payload to review or align the feature.

**Review mode** reports findings with source locations, verified Payload alternatives, and integration requirements. **Remediation mode** makes local changes while preserving existing behavior and reports the verification performed. Neither mode authorizes commits, publishing, upgrades, or unrelated changes.

## What it reviews

- **Layout and navigation:** Admin shell integration, headers, breadcrumbs, spacing, responsive behavior, and navigation state.
- **Actions and overlays:** toolbars, action hierarchy, icons, menus, tabs, drawers, dialogs, and confirmations
- **Data displays:** lists, tables, cards, search, filtering, sorting, pagination, selection, and bulk actions
- **Feedback:** loading, progress, empty, success, error, retry, permission, and unsaved-change states
- **Forms and editors:** native primitives, form-state ownership, validation, disabled/read-only states, and focus behavior
- **Styling and accessibility:** tokens, typography, themes, focus treatment, keyboard interaction, and supported contrast modes
- **Payload integration:** public exports, providers, translations, routes, access control, client/server boundaries, and production verification needs

## Compatibility and requirements

- Supports reviewing **Payload 3.x and 4.x** projects using their installed APIs and design systems
- Includes [version-specific guidance](references/version-compatibility.md) for styling, themes, buttons, tabs, search, and input primitives. Valid v3 theme variables and SCSS are preserved; unavailable v4 components are not treated as v3 defects
- Covers `@payloadcms/ui` **3.88.0**, **3.90.1**, and **4.0.0-beta.7**, with additional **3.92.0** tagged-source checks. Recommendations still require checking the project's exact release. Browser/assistive-technology verification and every historical minor version are not covered by those source checks


## License

[MIT](LICENSE). Maintained independently; this project is not an official Payload package.
