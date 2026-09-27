# AGENTS.md — Working Rules for AI Authoring

This repository is managed by **Pelcrow**, the reference desk and fact-checker for AI authoring agents.

<!-- pelcrow:managed:start -->
<!-- pelcrow:schema-version:sha256:1671d563b9542243 -->
<!-- Generated deterministically by Pelcrow from the live content index. -->
<!-- Do not edit inside this block: it is overwritten on every regeneration. -->

## Core Operating Principles by Repository Format

### DITA (pelcrow-test-dita)



### MDITA (azure-docs, docs, lwdita-code-samples, pelcrow-testcases)



## Repository and Write Boundary by Repository Format

### DITA (pelcrow-test-dita)

- **Destination repository**: the repository containing this `AGENTS.md` is the destination repository for authored content. Resolve every relative output path from this repository root, not from the location of an email, ticket export, specification, attachment, or other source document.
- **Write boundary**: create or update documentation only inside this destination repository unless the user explicitly names a different destination repository. Before writing, resolve the proposed path and verify that it remains inside this repository; if it does not, stop and correct the path.
- **External sources are read-only**: source material may live in Downloads, Documents, Jira, Confluence, another repository, or any other readable location. Reading a source never authorizes writing beside it, and its directory structure must never determine the output directory.
- **Follow the destination structure**: inspect this repository's maps and existing content hierarchy before choosing a path. In a DITA repository that uses a `topics/` hierarchy, place new authored topics under the appropriate `topics/<subject>/` subfolder in this repository and register them in the matching map.
- **Generated output is not source**: never read from, edit, or create authored content under the configured build-output directory (organization default: `out/`). Pelcrow excludes these directories from its content index and workspace.
- **Keep provenance separate from placement**: cite external inputs in `sources:` metadata, but keep the authored document in the destination repository's content hierarchy.
- **Local writes only by default**: write completed documents with native filesystem tools inside this repository’s local working tree. A documentation request does not authorize any remote repository mutation.
- **Complete the local change**: if a topic also requires a map, navigation, manifest, or other supporting-file update, make every required local edit before claiming the content is ready for review.
- **Leave Git to the user**: do not commit, push, open a pull request, or call a hosted write API unless the user separately and explicitly requests that exact action. Report the repository-relative paths changed so the user can review and check them in.

### MDITA (azure-docs, docs, lwdita-code-samples, pelcrow-testcases)

- **Destination repository**: the repository containing this `AGENTS.md` is the destination repository for authored content. Resolve every relative output path from this repository root, not from the location of an email, ticket export, specification, attachment, or other source document.
- **Write boundary**: create or update documentation only inside this destination repository unless the user explicitly names a different destination repository. Before writing, resolve the proposed path and verify that it remains inside this repository; if it does not, stop and correct the path.
- **External sources are read-only**: source material may live in Downloads, Documents, Jira, Confluence, another repository, or any other readable location. Reading a source never authorizes writing beside it, and its directory structure must never determine the output directory.
- **Follow the destination structure**: inspect this repository's maps and existing content hierarchy before choosing a path. In a DITA/MDITA repository that already uses a `topics/` hierarchy, place new authored topics under the appropriate `topics/<subject>/` subfolder in this repository.
- **Generated output is not source**: never read from, edit, or create authored content under the configured build-output directory (organization default: `out/`). Pelcrow excludes these directories from its content index and workspace.
- **Keep provenance separate from placement**: cite external inputs in `sources:` metadata, but keep the authored document in the destination repository's content hierarchy.
- **Local writes only by default**: write completed documents with native filesystem tools inside this repository’s local working tree. A documentation request does not authorize any remote repository mutation.
- **Complete the local change**: if a topic also requires a map, navigation, manifest, or other supporting-file update, make every required local edit before claiming the content is ready for review.
- **Leave Git to the user**: do not commit, push, open a pull request, or call a hosted write API unless the user separately and explicitly requests that exact action. Report the repository-relative paths changed so the user can review and check them in.

## Authoring Syntax by Repository Format

### DITA (pelcrow-test-dita)

## DITA XML Authoring Syntax

- **Variable**: `<ph keyref="repo:key">fallback text</ph>`.
- **Block transclusion**: `<p conkeyref="repo:target-key/element-id"/>` or `<step conkeyref="..."/>`.
- **Conditional content**: use standard filtering attributes: `audience="admin"`, `platform="cloud"`, `product="..."`, or generic `props="..."`.
- **Key definition**: define keys in the root publication map (`.ditamap`) using `<keydef keys="key-name" href="path/to/topic.dita"/>`.
- **Valid XML Structure**: every topic file must have a single root element (`<concept>`, `<task>`, `<reference>`, or `<troubleshooting>`) adhering to DITA specifications. Call the `get_xml_schema` MCP tool to retrieve required child element hierarchies instead of guessing.

### MDITA (azure-docs, docs, lwdita-code-samples, pelcrow-testcases)

## MDITA Authoring Syntax

### Maps and publication structure

For a new LwDITA publication, prefer an `.mditamap` file. Use Markdown list links to define the topic order and nest list items to define the TOC hierarchy.

Example:

```markdown
# Product documentation

- [Introduction](introduction.md)
- [Installation](installation.md)
  - [System requirements](system-requirements.md)
  - [Install the product](install.md)
- [Configuration](configuration.md)
```

An existing XML `.ditamap` is also supported and can reference MDITA `.md` topics. If the project already uses a `.ditamap`, preserve that format and update its `<topicref>` structure. Do not convert between `.mditamap` and `.ditamap` unless the user explicitly requests it.

Example:

```xml
<map>
  <title>Product documentation</title>
  <topicref href="introduction.md" format="mdita"/>
  <topicref href="installation.md" format="mdita">
    <topicref href="system-requirements.md" format="mdita"/>
    <topicref href="install.md" format="mdita"/>
  </topicref>
  <topicref href="configuration.md" format="mdita"/>
</map>
```

Use these rules:

- New LwDITA map: prefer `.mditamap`.
- Existing `.mditamap`: continue using `.mditamap`.
- Existing `.ditamap`: continue using `.ditamap`.
- MDITA topic referenced by XML: use `format="mdita"`.
- Preserve the existing topic order, hierarchy, attributes, keys, metadata, and map references unless the task requires changing them.
- Do not introduce YAML map declarations.
- Do not place XML `<topicref>` markup inside an `.mditamap`.
- Do not place Markdown list syntax inside a `.ditamap`.
- Do not convert an XML `.bookmap` into an `.mditamap`. Bookmap is a full-DITA structure.

### Variables in MDITA

Use `[variable]` to insert a variable.

Example:

```markdown
Welcome to [product-name].
```

Use variables already defined for the publication. Do not define variables inside an ordinary topic.

- **XML `.ditamap` key definitions are map-scoped, never in the topic or its frontmatter**: for an existing full-DITA `.ditamap`, declared via `<keydef keys="key-name" href="topics/target.dita"/>` (or any `keys`-bearing `<topicref>`). Frontmatter (`id:`, singular `key:`, etc.) is document metadata, never a key registry on its own; `data-key` (singular) is not the specification's `data-keys` and is not a recognized key-definition mechanism.
- **Key value (generate this form)**: `<topicmeta><keywords><keyword>Effective Value</keyword></keywords></topicmeta>` inside the `<keydef>` — Pelcrow's current generated standard for a pure variable, matching what Pelcrow's own Map Editor "Add Variable" UI writes. `<topicmeta><linktext>Effective Value</linktext></topicmeta>` is also a valid DITA effective-key-content representation (permitted as general fallback effective content, not only a link's display label) and Pelcrow reads it as a fallback, but do not generate it for a new pure variable — a future organization-level policy may select it as the enforced form instead. Both forms normalize to the same internal key/value semantics.
- **Block transclusion**: `<div data-conref="repo:key"></div>`; inline: `<span data-conref="repo:key"></span>`.
- **Conditional content**: use standard `data-props`. Generic values are whitespace-separated, e.g. `<p data-props="cloud internal">…</p>`. When the condition dimension matters, preserve it with parenthesized groups, e.g. `<p data-props="platform(cloud) audience(admin)">…</p>`. Both forms are valid; do not invent plain `platform=`, `audience=`, or `product=` HTML attributes.

**Contiguous HTML constraint**: wrapping block tags (`data-props`, `data-conref`) and their contents must be authored as contiguous raw HTML with **no interior blank lines** after the opening tag or before the closing tag. Interior blank lines cause DITA-OT to split the element into un-paired siblings, letting conditional content silently escape filtering.

## Mandatory Metadata by Repository Format

### DITA (pelcrow-test-dita)

DITA topics store metadata in `<prolog><metadata>`. Required properties (owner, journeyStage, useCases, etc.) should be stored as `<othermeta name="..." content="..."/>` elements.
- `title`: required topic title in `<title>` element.
- `owner`: `<othermeta name="owner" content="Author Name"/>`.
- `journeyStage`: `<othermeta name="journeyStage" content="..."/>`.
- `useCases`: `<othermeta name="useCases" content="..."/>`.

### MDITA (azure-docs, docs, lwdita-code-samples, pelcrow-testcases)

Every document's YAML frontmatter must set: title, owner, type, journeyStage, useCases, audience, platform.
- `type`: required — one of concept | task | reference | troubleshooting.
- `journeyStage`: required. Where in the customer journey this topic sits.
- `useCases`: required. Which of the organization's defined use cases this topic covers.

## Validation Gate (hard failures)

- **Unresolved references**: every `[key-name]`/`data-keyref`/`data-conref` (MDITA) or `@keyref`/`@conkeyref` (DITA) must resolve to an existing key definition.
- **Duplicate definitions**: a key defined more than once in the global namespace is rejected.
- **Transclusion cycles**: reuse loops (A → B → A) are forbidden.
- **Missing metadata**: required fields must be present (see Mandatory Metadata) — as frontmatter (MDITA) or `<prolog><metadata>` (DITA).
- **Invalid metadata value**: metadata values with permitted options must match one of the allowed values.
- **Malformed source**: YAML frontmatter that fails to parse (MDITA/Markdown), or XML that fails to parse (DITA/DocBook), is rejected.
- **Banned terminology**: any term in the organization's termbase is flagged with its preferred replacement.

HTML elements inside fenced code blocks are treated as literal example code and are never indexed as live references.

**This is not optional and it is not this document asking nicely.** Every commit to this repository — whether created by the user or another authorized workflow — is re-validated against these exact rules before it can merge. Calling `validate_draft`/`check_terminology` while drafting only changes when you find out about a problem, not whether it will be caught. Treat a hard failure here as equivalent to a failing test blocking a merge, because that is what it is.

## Pelcrow Repositories

Valid repository IDs for Pelcrow tools and namespaced keys: `azure-docs`, `docs`, `lwdita-code-samples`, `pelcrow-test-dita`, `pelcrow-testcases`.

## DITA-OT Operational Rules

- **Hands off the local DITA-OT installation**: do NOT install, uninstall, reinstall, reintegrate, or edit any file inside a local DITA-OT installation (`DITA_HOME`/`DITA_OT` paths). The toolkit setup is managed by the user.
- **Windows invocation**: always use `dita.bat` when running builds under Windows/git-bash; the Unix `bin/dita` script produces classpath resolution errors on Windows.

## MCP Integration (Crucial)

**You MUST connect to and use the Pelcrow MCP server.** This repository is part of a larger, interconnected content graph managed by Pelcrow. If you are an AI assistant editing this content:

- **Do not guess or hallucinate.** Use `search_documents` to find existing topics and `search_keys`/`resolve_key` to find reusable variables and snippets.
- **Stop on duplicate risk.** If `search_documents` returns `actionRequired: true`, do not draft. Show the candidate to the user and ask whether to reuse it in the map, update it, create a distinct variant, or intentionally create a separate topic. Never choose or fabricate an override yourself.
- **Ground every claim.** Only include behavior, UI labels, prerequisites, supported formats, and procedure steps explicitly supported by cited sources or verified existing documentation. Omit unsupported details or identify them as needing confirmation; plausible inference is not evidence.
- **Validate before you save.** Before writing a finished topic to the local working tree, run the proposed content through the `validate_draft` tool.
- **Respect terminology.** Use the `check_terminology` tool to ensure compliance with the organization's style guide.
- **Save finished content locally.** After validation, use the agent's native filesystem tools to create or update the topic in the requested output folder inside the local working tree.
- **Stop before version-control actions.** Creating or updating documentation does not authorize a commit, push, pull request, or remote repository write. Leave the validated files in the local working tree for the user to review and check in. Only run a version-control action when the user separately and explicitly requests that exact action.

## Organization Writing Guide

**Guide: Technical documentation**

Clear, consistent guidance for product documentation.

### Voice & tone

#### Use a direct, helpful tone (warning)

Address the reader as “you.” Use a neutral, direct, and helpful tone. Avoid “simply,” “obviously,” and “just” when describing a task.

**Use:**

```markdown
Select Save to apply your changes.
```

**Avoid:**

```markdown
Obviously, the user should simply click Save.
```

### Headings

#### Use sentence case for headings (required)

Write headings in sentence case. Capitalize only the first word and proper nouns. Do not end headings with a period.

**Use:**

```markdown
Configure retention policies
```

**Avoid:**

```markdown
Configure Retention Policies.
```

### Procedures

#### Start steps with an action (required)

Start each procedure step with an imperative verb and keep one primary action in each step. Use numbered lists for sequential actions.

**Use:**

```markdown
1. Open the Settings page.
```

**Avoid:**

```markdown
1. The Settings page should be opened.
```

### Notes

#### Use one note format (required)

Format notes as a GitHub-style admonition with an uppercase label. Use only NOTE, TIP, IMPORTANT, CAUTION, or WARNING.

**Use:**

```markdown
> [!NOTE]
> Restart the service for the change to take effect.
```

**Avoid:**

```markdown
**Note:** Restart the service.
```

### Tables

#### Keep tables consistent (required)

Introduce each table with a complete sentence. Include a non-empty header row, use sentence case for headers, and left-align text columns.

**Use:**

```markdown
The following table lists the available settings.

| Setting | Description |
|:--|:--|
```

**Avoid:**

```markdown
| SETTING | DESCRIPTION |
|:-:|:-:|
```

### Lists

#### Use parallel list items (warning)

Begin items in the same list with the same grammatical form. Use bullets for nonsequential information and numbers for sequential actions.

### Accessibility

#### Write descriptive link text (required)

Use link text that identifies the destination without surrounding context. Do not use “click here,” “here,” or a raw URL as link text.

**Use:**

```markdown
See [Configure retention policies](…).
```

**Avoid:**

```markdown
For more information, [click here](…).
```

### Custom

#### Use only verified source details (required)

Only include product behavior, UI labels, prerequisites, supported formats, and procedure steps that are explicitly supported by cited sources or verified existing documentation. Do not infer missing steps or capabilities. Omit unsupported details or identify them as needing confirmation.

Why: A plausible procedure that is not supported by a source is still incorrect documentation.

**Use:**

```markdown
The denylist takes precedence over the allowlist.
```

**Avoid:**

```markdown
Select your project, enter an IPv6 CIDR range, and select Add. (when those details are not present in a cited source)
```

<!-- pelcrow:managed:end -->

## Customer Rules

<!-- pelcrow:customer:start -->
_Add your team's own rules here. Pelcrow never modifies this section._
<!-- pelcrow:customer:end -->
