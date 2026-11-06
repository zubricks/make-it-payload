# Payload 3 and 4 compatibility

Use this reference before making version-specific recommendations. This skill supports reviewing Payload 3.x and 4.x projects in place; it is not a v3-to-v4 migration. Installed package exports, prop types, styles, and consumers take precedence over the examples here, especially across minor releases and betas.

## Select the project's version

- Resolve `payload`, `@payloadcms/ui`, and relevant adapters/plugins from the target application. Check the resolved package or lockfile rather than assuming a manifest range identifies the installed release. A multi-app workspace can contain both majors.
- Record the exact version and evidence in the review. Package export maps can block `package.json` subpath imports; read package metadata from disk or use the project's package-manager tooling instead of importing an unexported subpath.
- Use matching tagged source or documentation. Do not mistake the current documentation, a neighboring v4 checkout, or a beta migration guide for v3 implementation requirements.
- If the package version or API cannot be established, report the uncertainty and avoid speculative rewrites. Do not upgrade dependencies to make a proposed replacement available.

## Payload 3 path

### Styling and themes

`--theme-bg`, `--theme-text`, `--theme-elevation-*`, `--style-radius-*`, and SCSS/mixins are part of the checked v3 UI generation. Their presence is not a legacy-style violation in a v3 project. Reuse that version's variables and component styles; do not replace them with undefined v4 semantic tokens or rename working SCSS files solely to match v4 conventions. Follow the target project's style rules.

Inspect `useTheme` for the installed version. The checked v3 releases expose `theme`, `autoMode`, and `setTheme`, without v4's `highContrastMode`/`setHighContrastMode` APIs. Test supported light/dark themes and ordinary keyboard/focus/contrast behavior. Mark a host enhanced-contrast mode as unavailable if it is absent; do not fabricate its setting or require the v4 account screen to complete a v3 review. Lack of that host feature does not remove accessibility requirements.

### Components and interactions

The checked v3 exports include `Gutter`, `SetStepNav`, `Button`, `Popup`/`PopupList`, `Drawer`, `ConfirmationModal`, `Banner`, `toast`, `Table`, `Pagination`, `SearchFilter`, `TextInput`, `TextareaInput`, `CheckboxInput`, `SelectInput`, and `ReactSelect`. Find their consumers and verify props in the installed minor version; they are discovery leads rather than a guaranteed minimum-version list.

- V3 Banner supports types such as `info` and `error` in the checked releases; the checked v4 beta uses `brand` and `danger` instead. Preserve the message semantics using that version's types rather than copying a variant string between majors.
- V3 Button supports variants such as `error`, `subtle`, and `transparent`, plus small sizes in the checked releases. Do not recommend v4-only `destructive`, `ghost`, `primary-ghost`, or `loading` props without checking availability.
- The checked v3 exports have form-bound `TabsField`/`TabsProvider`/`TabComponent`, not v4's standalone `Tabs`/`TabsList`/`TabButton` composition. Do not register independent view-tab state as document fields to force reuse. Inspect an appropriate v3 consumer and use supported composition, or retain an accessible custom tab implementation with an explicit reason.
- Investigate v3 `SearchFilter` rather than assuming v4 `SearchInput`/`ListSearchFilter` replacements exist. Verify the public props, data/provider context, and search semantics before substituting it.
- For independent controls, inspect input primitives separately from form-bound fields. For example, the checked `TextInput` requires `path`, `SelectInput` uses `name`/`path` and option callbacks, and `CheckboxInput` uses `onToggle` with a change event. Do not invent a universal DOM-style adapter or assume input IDs/refs are identical across versions.

## Payload 4 path

Inspect the exact 4.x release or beta. In the checked beta, Admin styles use plain CSS and semantic `--color-*`, spacing, radius, typography, stroke, and elevation tokens. Avoid using raw palette values as theme-aware component colors. The v3 `@payloadcms/ui/scss` entry point and legacy theme-variable system are not the basis for v4 custom styling.

The checked v4 beta exports standalone tabs and `SearchInput`/`ListSearchFilter`, revises Button/Banner variants and sizes, and adds an enhanced-contrast theme context. Use only APIs exported by the installed beta. Do not assume every beta has the same exports, visual design, or behavior, and do not apply v3 snippets blindly to v4 either.

## Shared integration rules

Both paths should prefer suitable public components over replicas, use the configured Admin/API routes, preserve translations and user access, respect form ownership and client/server boundaries, and verify real navigation, overlays, data discovery, feedback, and keyboard behavior. The installed host determines which concrete APIs implement those principles.

A feature's custom graph, diff, or frontend branding does not require a framework migration. If a suitable component is unavailable in that version, document the gap and keep or adapt the custom implementation. Do not report absence of a v4 feature in v3 as a defect.

## Evidence and maintenance

Shared primitive examples and major-specific Button, Banner, search, tabs, and theme API checks were type-checked against local installations of `@payloadcms/ui` **3.88.0**, **3.90.1**, and **4.0.0-beta.7**. V3 checks also confirmed that v4-only tab/search exports, destructive Button variants, and enhanced-contrast properties are rejected. Additional tagged **3.92.0** source was checked for public exports, v3 theme context, and styling. This is evidence for the version branches above, not a claim that all historical 3.x/4.x APIs or browser behavior have been tested.

Primary references:

- [V3.92.0 public client exports](https://github.com/payloadcms/payload/blob/v3.92.0/packages/ui/src/exports/client/index.ts)
- [V3.92.0 Button props](https://github.com/payloadcms/payload/blob/v3.92.0/packages/ui/src/elements/Button/types.ts)
- [V3.92.0 styling variables and mixins](https://github.com/payloadcms/payload/blob/v3.92.0/packages/ui/src/scss/vars.scss)
- [V3.92.0 theme provider](https://github.com/payloadcms/payload/blob/v3.92.0/packages/ui/src/providers/Theme/index.tsx)
- [V4.0.0-beta.7 public client exports](https://github.com/payloadcms/payload/blob/v4.0.0-beta.7/packages/ui/src/exports/client/index.ts)

When guidance changes, check a representative v3 project as well as a v4 project. Include a no-form workflow and cases where a native replacement exists in only one major. Keep source/type verification separate from browser or assistive-technology evidence.
