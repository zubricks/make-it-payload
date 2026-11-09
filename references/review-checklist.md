# Payload integration review checklist

Use the project's installed version and conventions as the authority. Apply the version branch in [version-compatibility.md](version-compatibility.md) first; a v3 review must not enforce v4 styling or component availability. Names below are discovery leads, not a guaranteed cross-version API catalog. Confirm public exports, types, provider requirements, and a working consumer before changing code.

## 1. Page shell, layout, and navigation

- Compare with an equivalent built-in view in the same version. Check Admin shell integration, gutters/content width, page title and description, header placement, breadcrumbs, navigation groups, and selected navigation state. Reuse public layouts/compositions where appropriate instead of building a second application shell.
- Verify hierarchy and density: heading levels, panel divisions, spacing, sticky headers/footers, and scroll ownership should serve the same kinds of tasks as their Payload counterparts. Using tokens alone does not make a composition consistent.
- Inspect navigation destinations, route construction, back behavior, deep links, and retained view state. Preserve existing product choices; identify a concrete mismatch before changing navigation or layout.
- Check narrow and wide layouts, long/localized labels, RTL where supported, overflow, and focus visibility under fixed controls. Keep custom workspaces when their task requires a different composition.

## 2. Actions, navigation widgets, and overlays

- **Toolbars and actions:** use supported Button/link variants and Payload icons. Compare primary/secondary/destructive hierarchy, placement, grouping, compact actions, and overflow menus with a similar built-in workflow. Do not make every action primary or hide essential actions only on hover. Preserve appropriate names for icon-only actions, non-submit button types, busy state, and double-submission protection.
- **Action width:** keep ordinary and destructive actions content-width unless the host pattern or explicit layout calls for a full-width action. Check flex/grid stretching, `width: 100%`, and inherited alignment: choosing a native Button does not prevent a standalone Remove action from stretching across a drawer. Prefer appropriate container alignment over overriding the native button design.
- **Adjacent control sizing:** inputs/selects and buttons placed beside each other should normally have matching visible control heights and aligned edges. Compare the actual control boxes, excluding labels/help text; identical size prop names do not guarantee identical rendered heights across components or Payload versions. Prefer supported size variants and host tokens. Allow deliberate differences for icon actions, toggles, or established host patterns, with a clear reason.
- **Menus and tabs:** use supported Popup/PopupList and the version's available tab patterns where they fit. V4 standalone tabs must not be assumed available in v3. Check active states, dismissal, keyboard interaction, focus management, and menu/tab/panel relationships. Preserve the distinction between links that navigate and buttons that perform actions.
- **Drawers, dialogs, and confirmations:** choose the appropriate public Drawer, document drawer, modal, or confirmation API. Check size/layout, headers and close actions, nested layering, portal ownership, background inertness, scroll locking, Escape, initial focus, and focus restoration. Do not replace a specialized document workflow with a generic overlay without examining its behavior.
- **Tooltips and disclosure:** reuse available components with accessible names and consistent delays/dismissal. Essential instructions or actions must remain discoverable without hover. Native buttons within graph nodes or composite widgets are not automatically violations just because a general-purpose Button exists.

## 3. Data display and discovery

- Inspect lists, tables, cards, badges, relationship summaries, row actions, and pagination for suitable public Payload components/compositions. Preserve column/row semantics, text truncation and access to full content, links, selection state, and empty cells.
- Check search, filters, sorting, result counts, pagination, and bulk actions as one workflow: state changes, clearing/resetting, loading, no-results behavior, selection across pages, and disabled/destructive actions. Reuse available host behavior instead of adding an independent conflicting implementation.
- Match interaction and information hierarchy to the task. A diagram, diff, or specialized visualization may remain custom; inspect its surrounding controls, labels, feedback, and accessibility separately.

## 4. Feedback and lifecycle states

- Look for suitable Payload notifications, banners, status messages, loading indicators/skeletons, and empty-state patterns before recreating them. Verify public APIs rather than assuming every pattern has an exported component.
- Check initial loading, refresh, mutation progress, success, partial failure, failure/retry, permissions, no content, and no matching results where reachable. Avoid a screen that appears usable while actions silently fail or block without explanation.
- Compare feedback prominence, wording, dismissal, persistence, and focus/announcement behavior with the host. Avoid stacked duplicate messages, success banners that survive an unrelated operation, or focusable/outlined message containers without a reason.
- Preserve unsaved-change protection and appropriate confirmation/recovery for consequential actions. Clarify what changed and how to recover without introducing extra confirmation steps for routine actions.

## 5. Forms and editors

- **Text and descriptions:** investigate `TextInput`, `TextareaInput`, `FieldLabel`, `FieldDescription`, and `FieldError` instead of hand-styled controls. Check IDs, required indicators, descriptions/errors, refs, input attributes, and supported read-only/disabled props. A textarea fallback or file picker can be legitimate when the package has no suitable public primitive.
- **Selections:** investigate `ReactSelect`, `SelectInput`, or form-bound `SelectField` instead of raw `<select>` or a separate direct `react-select` implementation. Verify single/multi values, option objects, clear behavior, async/search needs, empty choices, loading, and disabled state. Raw selects are review candidates; retain one only for a concrete requirement the available component cannot meet appropriately.
- **Checkboxes:** investigate `CheckboxInput` versus form-bound `CheckboxField`. Verify the actual callback contract (for example `onToggle` rather than an assumed DOM change event), checked/indeterminate state, label activation, and Space-key behavior. Do not present a read-only control as editable or confuse schema `required` metadata with this UI's HTML validation.
- **Editors:** investigate available code/rich-text editor components and their keyboard modes before adding a separate implementation. Preserve input isolation, syntax handling, accessible fallback, and the surrounding host layout.

### Choose the correct integration layer

Payload's `*Field` components commonly integrate with `useField`, document forms, validation, localization, and related providers. Input primitives commonly support explicitly controlled values. Confirm this in source; naming is not a contract.

For an independent schema editor, swapping `<input>` for a form-bound `TextField` can register schema-editor state against the document being edited. Choose a compatible input primitive or adapter instead. Check for accidental parent-form submissions and hidden form-state mutation after conversion.

## 6. Styling and host consistency

- Remove local replicas of replaced Payload layouts, widgets, feedback, and controls. Check broad selectors such as `.plugin input` and `.plugin button`, which may override nested native components.
- Use supported tokens for the installed UI generation: semantic colors, spacing, typography, radii, strokes, and elevations. In v3, valid `--theme-*`/`--style-*` variables and SCSS are not violations merely because v4 replaced them. In v4, use that release's semantic tokens and CSS conventions. Do not impose one major's styling system on the other or use raw palette values as theme-aware colors.
- Use theme/contrast providers and semantic component variants. Verify hover, focus, selected, invalid, busy, read-only, and disabled states in light/dark and supported enhanced contrast.
- Inspect custom focus rules (`:focus`, `:focus-visible`, outline, box-shadow, and wrapper `:focus-within`) for doubled rings, oversized outlines, or styling around the wrong element. Compare with the intended native treatment in the same app/theme; a screenshot alone does not establish the source of a ring. Preserve visible keyboard focus rather than removing it to improve appearance. Avoid automatic input selection on existing-item editing unless the workflow warrants it. Keep focused elements visible in scrollable inspectors and overlays.
- Follow the target project's CSS/SCSS conventions for logical properties, breakpoints, RTL, and component scoping. Retain layout styles for genuinely custom surfaces; native reuse does not eliminate feature layout.
- Check that portals inherit the intended theme, stack above the correct layer, and do not trap scroll or disappear behind drawers. Avoid arbitrary z-index patches before inspecting Payload's layering utilities.

## 7. Payload application integration

- **Plugin shape:** inspect existing plugin conventions. Compose with the host config; preserve existing components, hooks, collections, endpoints, and options instead of replacing them accidentally. Keep application-specific paths and assumptions configurable when packaging is in scope.
- **Admin registration:** use supported custom-view/navigation slots, metadata/breadcrumb/layout components, import maps, and providers. Do not bootstrap a duplicate Admin tree or hardcode a root-level route assumption.
- **URLs:** use `formatAdminURL` and the configured admin/API routes. Use the project's query serializer (for example `qs-esm`) for structured queries.
- **Labels/translations:** use Payload's translation utilities such as `getTranslation` for config-defined labels. Use the project's UI translation workflow for new interface strings; avoid stringifying objects/functions or assuming English labels.
- **Client/server separation:** verify supported public package entry points, `'use client'` boundaries, server component registration, and serializable client props. Keep secrets, filesystem access, and server dependencies out of client bundles. Inside the monorepo, follow its client-export and component-folder rules.
- **Data and access:** when acting on behalf of a user, pass the authenticated user and use `overrideAccess: false` explicitly for Local API operations; do not assume Local API access enforcement. Check endpoint authentication/authorization and relevant request protections. Separate legitimate trusted setup/maintenance operations from user-facing actions.
- **Hooks and operations:** preserve `req`/context/transaction behavior where needed and existing validation/access logic. Do not replace built-in operations with direct database access solely for convenience.
- **Dependencies:** reuse host APIs before adding UI libraries. Check compatible Payload package versions and peer-dependency conventions when publishing is actually in scope. A specialized canvas/editor dependency is not inherently a defect.
- **Source changes:** for tools that write configs, keep source-of-truth, external-change checks, custom-code preservation, and error/rollback behavior working. Replacing UI primitives must not weaken these protections.

## 8. Verification and evidence

Use existing project tooling and relevant fixtures. Cover the relevant journey areas, including behavior that replacements can break:

- Entry/navigation, breadcrumbs/back links, toolbar hierarchy, action availability, and tabs.
- Rendered action widths and side-by-side control heights/alignment at relevant viewports, including wrapped rows, busy/disabled states, and long labels. Verify screenshots or bounding boxes rather than assuming component adoption or matching size props produce a coherent layout.
- Search/filter/sort/pagination, list selection, bulk actions, and retained view state.
- Loading/progress, empty/no-results, success/error/retry, permissions, and unsaved changes.
- Controlled values, default/empty values, dirty tracking, validation, and persistence.
- Select value identity and clearing, checkbox callbacks, input refs, and labels/error associations.
- Keyboard access, focus visibility/order/return, no keyboard traps, meaningful names/states, and relevant status/error announcements.
- Nested overlays, scroll containers, responsive layout, theming, and contrast-provider integration.
- Appropriate pointer interactions and alternatives for custom dragging, without losing click-to-edit behavior.
- Admin navigation and API requests under configured non-default routes when route construction changes.
- Production imports/bundling when boundary or dependency changes warrant it.

Apply the accessibility workflow for plausibly affected WCAG 2.2 A/AA criteria; common ones include 1.3.1, 1.4.3, 1.4.11, 2.1.1, 2.1.2, 2.4.3, 2.4.7, 2.4.11, 2.5.3, 2.5.7, 2.5.8, 3.3.1, 3.3.2, 4.1.2, and 4.1.3. Select criteria from actual behavior. Axe and DOM assertions do not prove screen-reader speech or whole-product conformance.

## 9. Calibration examples

**Custom list/dashboard:** inspect its shell, header, toolbar, search/filter controls, table/cards, row and bulk actions, pagination, loading, no-results state, and notifications. A page with no editable fields still requires a complete review.

**Overlay workflow:** a feature may use Payload inputs yet have a homemade modal, incorrect layer stacking, misplaced primary actions, and no error/retry feedback. Native form controls do not make that workflow native.

**Independent visual schema inspector:** raw Name/Label/Description inputs, native Type/Relationship selects, checkboxes, and custom action buttons should trigger a search for native input primitives and actions. Existing native drawers or editors do not excuse the remaining handmade form controls. Keep the graph library, connection handles, and deliberate node interactions unless a suitable host abstraction exists.

**Complex existing collection:** imported access functions, field factories, and hook logic are not reasons to rewrite working source during a UI alignment pass. Preserve them. Find a specific integration defect before proposing a change.

**Public marketing site:** a custom branded frontend using Payload for content does not need Admin controls or Admin styling. Review only its Payload integration and any custom Admin extensions in the requested scope.
