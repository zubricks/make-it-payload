---
name: make-it-payload
description: Review and align a working Payload plugin, custom Admin UI, or application with the installed Payload 3 or 4 native Admin experience and integration conventions. Use for post-build review and remediation of layouts, navigation, components, interactions, styling, feedback states, and application integration.
license: MIT
metadata:
  version: "0.1.0"
---

# Make It Payload

A functional Payload feature should use the platform it extends. Find places where it recreates Payload UI or bypasses existing integration patterns, then make the implementation native while preserving its working behavior.

The guiding question is: **What does Payload already provide for this experience, and are we using it correctly?** Review the whole feature, not just its form controls.

This skill targets Payload integrations and Admin UI. Do not reskin a public-facing website to look like the Admin UI. Specialized interfaces such as a graph canvas can remain custom; their surrounding layout, navigation, actions, overlays, feedback, and forms should reuse Payload where suitable.

## Scope and mode

- Resolve the requested project, plugin, paths, or diff first. The current working directory may be a Payload checkout while the feature lives in a separate application. Read that project's instructions and preserve unrelated edits.
- A request to **check, audit, or review** is assessment-only: inspect, exercise the UI, and report concrete findings without changing implementation.
- A request to **make it Payload, align, fix, or refactor** authorizes local remediation within that scope. Complete the review and routine fixes without inserting another approval gate. Report anything that requires a product decision or incompatible API change.
- Running this skill does not authorize commits, publishing, package upgrades, database changes, or expanding the feature. Installing this skill does not itself authorize changing a project.

## Establish the actual Payload surface

Read [version-compatibility.md](references/version-compatibility.md) and select the Payload 3 or 4 path before proposing replacements. Review the current major in place; never treat this skill as permission to migrate v3 to v4.

1. Identify installed/resolved versions of `payload`, `@payloadcms/ui`, and related packages. Check the lockfile when manifest ranges are ambiguous. Do not use a nearby monorepo's current APIs as proof of what another app supports.
2. Read the project's Payload development skill if provided. In an installed app, look for `node_modules/payload/skills/payload/SKILL.md`; in a Payload checkout, inspect `packages/payload/skills/payload/SKILL.md`. These are optional sources, not installation prerequisites. If absent, continue with the installed packages and official version-appropriate documentation; do not require a separate checkout or another skill.
3. Inspect `@payloadcms/ui` public exports, relevant prop types, implementations, and existing consumers in the same version. Source files are evidence of behavior, not permission to use an unexported internal import. Use official version-appropriate documentation when local evidence is insufficient.
4. Trace the feature through its real Admin layout, navigation, views, overlays, and providers. Compare it with a similar built-in Payload journey. Where forms are involved, establish document-form versus independent local state before choosing components.

## Review the finished implementation

Read [review-checklist.md](references/review-checklist.md). Start with the complete user journey: how users enter the feature, understand its hierarchy, navigate between views, act on content, receive feedback, and leave or recover. Then trace components, styling, providers, and server boundaries.

Give these areas equal consideration where present:

- Page shell, headers, breadcrumbs, navigation, spacing, and responsive layout.
- Toolbars, action hierarchy, icons, menus, tabs, drawers, dialogs, and confirmations.
- Lists, tables, cards, search, filtering, sorting, pagination, selection, and bulk actions.
- Loading, progress, empty, success, error, permission, disabled, and unsaved-change states.
- Forms and editors, including state ownership, validation, and focus behavior.
- Typography, themes, interaction states, accessibility, and Payload application integration.

Record which areas were assessed, legitimately not applicable, or not verified. A completed form-control scan must not be reported as a whole-feature review.

Search the scoped implementation for duplicated page shells, navigation, toolbars, data displays, banners/toasts, loaders, menus/dialogs/tabs, raw form controls, direct third-party UI imports, copied component CSS, and hardcoded tokens. Treat search matches as candidates, not automatic violations. Include nested drawers, empty states, error states, and secondary actions; checking only the main screen misses much of the experience.

For every actionable finding, record:

- **Where and what:** file/line, affected state, and concrete user impact.
- **Payload replacement:** a verified public component, hook, utility, or pattern available in this project's version; cite its export/type/consumer evidence.
- **Integration requirements:** providers, controlled state, event/value shapes, refs, labels, validation, focus, portals, and styling that must be preserved.
- **Disposition:** replace, adapt, retain with a specific reason, or unresolved because evidence is missing.

Do not report a feature as native merely because it imports a few Payload components or uses Payload color variables. Audit the experience actually rendered to users, including layout, action hierarchy, transitions, and feedback. Conversely, do not replace every HTML element: native structural markup and purpose-built interactions may be appropriate.

## Remediate when authorized

Prefer supported Payload layouts, compositions, and components that fit the journey and state model; use smaller primitives when an existing composition does not fit. Preserve navigation destinations, active states, search/filter state, action availability, selection, and recovery behavior.

For forms, prefer the smallest public Payload primitive that fits the existing state model. A standalone controlled editor usually needs input primitives; a document field should participate in Payload's form APIs. Do not force unrelated local state into document submission or add a form provider merely to satisfy a component's requirements.

Replace duplicated components and their obsolete styling together. Preserve input IDs or update associated labels/help/errors and tests as one change. Preserve option values, clear/multi-select semantics, read-only behavior, action ordering, and focus return. Avoid blanket CSS that restyles Payload descendants or copied class names that imitate its controls.

Keep custom pieces where Payload has no suitable equivalent. State the missing capability or incompatibility and use supported composition before building a replacement. A graph library is not a reason to recreate its surrounding toolbar, drawer, notifications, or inspector controls.

Prioritize findings by user impact across the entire journey, rather than fixing forms first by default. Address misleading actions, inaccessible navigation, broken overlays, and missing feedback alongside control reuse, integration defects, and redundant styles. Do not turn this pass into a redesign or dependency migration.

## Verify in context

- Run scoped type/lint checks and meaningful existing tests. Component substitution can change events, IDs, focus, and form submission even when the screen looks similar.
- Exercise the actual Admin journey: navigate in and out, use toolbar actions and tabs, search/filter/sort lists, select items, open/close nested overlays, and observe loading, empty, success, failure, permission, and unsaved-change states where applicable. Also verify form editing, keyboard navigation, dropdown clipping, scrolling, and relevant responsive layouts.
- Use the `payload-accessibility` skill when available for affected UI. If unavailable, inspect labels, accessible names/states, keyboard/focus behavior, errors, light/dark modes, and contrast settings supported by that version; report the missing specialized evidence. Use the bundled checklist for relevant criteria and verification needs. Do not install another skill solely to proceed, and do not claim accessibility from component choice alone.
- If browser tools or a running app are unavailable, continue with source inspection and explicitly mark visual and interaction behavior unverified. Do not represent source checks as runtime evidence.
- Visually compare with an equivalent built-in Payload view, composition, or control in the same running app/version. Verify theme providers and any enhanced-contrast mode actually supported by that version, rather than manually changing DOM attributes and assuming context updated. A missing v4-only mode or component is not a v3 defect.
- Verify production behavior when imports, RSC/client boundaries, exports, or packaging change. Follow the project's production workflow; a dev-server pass does not establish production bundling correctness. Report unavailable checks.

For handoff, state the resolved Payload version, coverage and highest-impact findings, what was fixed, what remains custom and why, and actual verification results. Include relevant WCAG 2.2 A/AA criteria and manual/AT gaps for UI work, proportionally. Keep assessment-only findings concrete enough to implement. Do not claim the entire project is Payload-native or accessible based on a partial scan.

## Example invocations

- `$make-it-payload check src/plugins/my-plugin` — review only, covering the complete feature experience and verified replacements.
- `$make-it-payload src/plugins/my-plugin` — align the implementation locally and verify retained behavior.
- `$make-it-payload review this plugin before packaging` — assess public exports, providers, styling, integration, and production verification needs; do not publish it.
