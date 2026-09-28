# AGENTS.md — Working Rules for AI Authoring

This repository is managed by **Pelcrow**, the reference desk and fact-checker for AI authoring agents.

<!-- pelcrow:managed:start -->
<!-- pelcrow:schema-version:sha256:1671d563b9542243 -->
<!-- Generated deterministically by Pelcrow from the live content index. -->
<!-- Do not edit inside this block: it is overwritten on every regeneration. -->

## Core Operating Principles by Repository Format

### DITA (pelcrow-test-dita)



### MDITA (docs, lwdita-code-samples, pelcrow-testcases)



### Microsoft Learn Markdown (azure-docs)

1. **Search before writing**: call `search_documents` for existing topics, then use `search_keys` and `resolve_key` for reusable facts and passages. Never rewrite content that already exists.
2. **Stop before duplicating a topic**: when `search_documents` reports high duplicate risk, stop before drafting and ask the user to choose reuse, update, variant, or an intentionally separate topic. Reuse the existing topic through the publication map whenever it already satisfies the need.
3. **Use native Microsoft Learn reuse**: reuse shared passages with `[!INCLUDE [description](relative/path.md)]`, preserving the repository's existing include organization and relative paths.
4. **Never invent factual values**: product names, version numbers, URLs, and environment flags must come from verified source material or existing shared includes — never guess them.
5. **Use only verified source details**: only state behavior, UI labels, prerequisites, supported formats, and procedure steps supported by a cited source or verified existing documentation. Do not turn plausible assumptions into instructions.
6. **Record provenance**: every new or updated document must cite what produced it in a frontmatter `sources:` list (plural — not `source:`) — one or more opaque IDs such as `commit:abc123`, `ticket:JIRA-42`, or a spec name/URL. This is what staleness tracking keys off; a document with no `sources:` entry, or the wrong field name, can never be flagged stale when its source changes.
7. **Check before saving**: run every draft through `validate_draft` before writing the finished content locally — this already includes terminology checking. Use `check_terminology` on its own to check a smaller piece of text (before it's assembled into a full draft) for banned/avoid terms and their preferred replacements.
8. **Save locally and stop**: after the checks pass, use native filesystem tools to create or update every required topic, map, navigation file, or manifest in the requested local output folder. Do not commit, push, open a pull request, or write through a hosted repository API unless the user separately and explicitly asks for that exact version-control action.
9. **Name files descriptively**: file names must be kebab-case and derived from the topic's actual subject, matching this repository's existing convention (e.g. `configure-retention-policies.dita`, `knox-compatibility-matrix.md`) — never generic names like `overview.md`, `usage.md`, or `index.md` that say nothing about what the topic covers.
10. **Look up ticket references yourself, don't trust a paraphrase**: Pelcrow has no access to any issue-tracker system (Jira, Linear, GitHub Issues, etc.) — if the task mentions a ticket ID that will end up in `sources:` as `ticket:ID`, and a ticket-tracker MCP server is also available to you in this session, search it directly for that ticket's actual current title, description, and status before drafting. A secondhand summary pasted into the conversation can be stale, incomplete, or wrong; the ticket itself is the source of truth you're citing.
11. **Search for a ticket before assuming there isn't one**: if you're asked to draft something without a ticket ID being named, and a ticket-tracker MCP server is available, search it for anything that plausibly matches the topic before you start — don't just proceed source-less because none was handed to you. If you find a real candidate, confirm with the user which one (if any) applies before citing it; never guess an ID. If nothing plausible turns up, or no tracker is available, that's fine — `sources:` accepts `commit:`, `spec:`, or a URL just as well, and a ticket citation specifically is never mandatory.

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

### MDITA (docs, lwdita-code-samples, pelcrow-testcases)

- **Destination repository**: the repository containing this `AGENTS.md` is the destination repository for authored content. Resolve every relative output path from this repository root, not from the location of an email, ticket export, specification, attachment, or other source document.
- **Write boundary**: create or update documentation only inside this destination repository unless the user explicitly names a different destination repository. Before writing, resolve the proposed path and verify that it remains inside this repository; if it does not, stop and correct the path.
- **External sources are read-only**: source material may live in Downloads, Documents, Jira, Confluence, another repository, or any other readable location. Reading a source never authorizes writing beside it, and its directory structure must never determine the output directory.
- **Follow the destination structure**: inspect this repository's maps and existing content hierarchy before choosing a path. In a DITA/MDITA repository that already uses a `topics/` hierarchy, place new authored topics under the appropriate `topics/<subject>/` subfolder in this repository.
- **Generated output is not source**: never read from, edit, or create authored content under the configured build-output directory (organization default: `out/`). Pelcrow excludes these directories from its content index and workspace.
- **Keep provenance separate from placement**: cite external inputs in `sources:` metadata, but keep the authored document in the destination repository's content hierarchy.
- **Local writes only by default**: write completed documents with native filesystem tools inside this repository’s local working tree. A documentation request does not authorize any remote repository mutation.
- **Complete the local change**: if a topic also requires a map, navigation, manifest, or other supporting-file update, make every required local edit before claiming the content is ready for review.
- **Leave Git to the user**: do not commit, push, open a pull request, or call a hosted write API unless the user separately and explicitly requests that exact action. Report the repository-relative paths changed so the user can review and check them in.

### Microsoft Learn Markdown (azure-docs)

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

### MDITA (docs, lwdita-code-samples, pelcrow-testcases)

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

### Microsoft Learn Markdown (azure-docs)


## Microsoft Learn Markdown Authoring Syntax

Follow the Microsoft Learn Markdown reference: https://learn.microsoft.com/en-us/contribute/content/markdown-reference.

- **Base syntax**: author CommonMark as processed by Markdig, plus only the Microsoft Learn extensions already used by the repository.
- **Includes**: [!INCLUDE [description](../includes/file.md)]. Preserve both the description and the relative target path.
- **Alerts**: begin the blockquote with [!NOTE], [!TIP], [!IMPORTANT], [!CAUTION], or [!WARNING] on its own line.
- **Images**: standard ![alt text](media/image.png) or :::image type="content" source="media/image.png" alt-text="Description":::.
- **Code**: put the language token immediately after the opening fence. For referenced samples use :::code language="csharp" source="../samples/example.cs":::.
- **Bold has meaning**: use bold only for interactive UI elements and values the user selects or enters. Do not use bold for general emphasis or merely for table, property, or resource names.
- **Navigation paths**: write menu and UI paths as **A** > **B** > **C**.
- **Italic has meaning**: use italic only when introducing a new term whose definition appears in the same or next sentence, or for a placeholder.
- **Inline code has meaning**: use code formatting for code, commands, parameters, package names, table and column names, file names and paths, and resource names that must not be localized.
- **Placeholders**: put <name> inside code formatting or escape it as \<name\>. Never write a bare angle-bracket placeholder that Markdown can interpret as HTML.
- **Headings and link text**: do not use bold, italic, or inline code in headings, and do not use bold or italic in link text.

Formatting source: https://learn.microsoft.com/en-us/contribute/content/text-formatting-guidelines.

- **Directives**: preserve triple-colon blocks such as :::zone, :::moniker, :::image, :::code, and tab/pivot structures byte-for-byte unless the requested change targets that directive.
- **No MDITA reinterpretation**: ordinary [bracketed text] is not a variable. Do not introduce data-keyref, data-conref, data-props, .mditamap, or DITA key syntax.
- **No entity corruption**: do not serialize Markdown punctuation as HTML entities unless the source intentionally contains an entity.


## House Style (from markdownlint)

The repository's own markdownlint configuration states these rules. Changed lines are checked against them before a commit is accepted.

- **Proper names**: write exactly `JavaScript`, `PowerShell`, `ASP.NET`, `macOS`, `NuGet`, `.NET`, `CLI`, `C#`.
- **Italic**: use the same delimiter the file already uses.
- **Bold**: use the same delimiter the file already uses.

## Mandatory Metadata by Repository Format

### DITA (pelcrow-test-dita)

DITA topics store metadata in `<prolog><metadata>`. Required properties (owner, journeyStage, useCases, etc.) should be stored as `<othermeta name="..." content="..."/>` elements.
- `title`: required topic title in `<title>` element.
- `owner`: `<othermeta name="owner" content="Author Name"/>`.
- `journeyStage`: `<othermeta name="journeyStage" content="..."/>`.
- `useCases`: `<othermeta name="useCases" content="..."/>`.

### MDITA (docs, lwdita-code-samples, pelcrow-testcases)

Every document's YAML frontmatter must set: title, owner, type, journeyStage, useCases, audience, platform.
- `type`: required — one of concept | task | reference | troubleshooting.
- `journeyStage`: required. Where in the customer journey this topic sits.
- `useCases`: required. Which of the organization's defined use cases this topic covers.

### Microsoft Learn Markdown (azure-docs)


Preserve the repository's existing Microsoft Learn YAML frontmatter and field order. The effective Pelcrow metadata schema remains authoritative for organization-required fields. Do not invent Microsoft-internal metadata values; copy or update values only from a verified source. See https://learn.microsoft.com/en-us/contribute/content/metadata.


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
