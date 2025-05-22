PuzzlePins Specification - Version 1.0  
Date: May 21, 2025

**License**

PuzzlePins Specification v1.0 is licensed under the Creative Commons
Attribution 4.0 International License (CC BY 4.0). You are free to use,
adapt, and distribute this specification; even commercially; without
attribution in your project files. Inclusion of reference tags such as:

// puzzlepins\_version: 1.0

// puzzlepins\_specification\_url:
https://github.com/robokeys/puzzle-pins/SPEC.md

is not required but encouraged for discoverability and clarity, but some
tools may use this information.

**1. Abstract**

PuzzlePins is a comment-based tagging system for source code and text
documents, designed with a tooling-first philosophy. It enhances
clarity, interaction, and collaboration between human developers, AI
tools, code generators, and other automated systems.

By standardizing metadata, structural delineation, and annotation,
PuzzlePins:

-   Enables robust automation without sacrificing human oversight.

-   Clearly separates tool-managed and human-authored areas.

-   Improves comprehension, maintainability, and safety in code
    modification.

**Tag Types:**

-   **🧩 (Puzzle Tags)**: Define tool-managed structure and metadata.

-   **📌 (Pin Tags)**: Reserved for human annotations, readable but not
    modifiable by tools.

-   **🤖 (Robot Tags)**: AI-generated metadata and analysis; used for
    annotations, diagnostics, and automated suggestions.

**2. Core Principles**

**Emoji-to-Purpose Table**

Note: Emoji appearance may vary by OS, font, or tool (e.g., macOS,
LibreOffice, VS Code). While rendering may differ, each tag uses a
consistent Unicode character and can be reliably copied, pasted, and
interpreted. You can also look them up by their official Unicode names
for clarity in tooling.

<table>
<colgroup>
<col style="width: 7%" />
<col style="width: 11%" />
<col style="width: 36%" />
<col style="width: 13%" />
<col style="width: 17%" />
<col style="width: 13%" />
<col style="width: 0%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Emoji</strong></th>
<th><strong>Name</strong></th>
<th><strong>Purpose</strong></th>
<th><strong>ASCII Fallback</strong></th>
<th><strong>Unicode Codepoint</strong></th>
<th><strong>Unicode Name</strong></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>🧩</td>
<td>Puzzle (Tool)</td>
<td>Tool-generated structure and metadata</td>
<td>*-+</td>
<td>U+1F9E9</td>
<td>PUZZLE PIECE</td>
<td></td>
</tr>
<tr class="even">
<td>📌</td>
<td>Pin (Human)</td>
<td>Human-authored annotations, explanations, reminders</td>
<td>*-&gt;</td>
<td>U+1F4CC</td>
<td>PUSH PIN</td>
<td></td>
</tr>
<tr class="odd">
<td>🤖</td>
<td>Robot (AI)</td>
<td>AI-generated metadata, diagnostics, or suggestions</td>
<td>*-&lt;</td>
<td>U+1F916</td>
<td>ROBOT FACE</td>
<td></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
</tbody>
</table>

**2.1. Comment-Based**

Tags use host-language comment syntax (e.g., //, \#, &lt;!--), ensuring
language agnosticism.

**2.2. Primary Markers (Tool-Centric)**

-   🧩: Emoji marker for tool-driven metadata.

-   \*-+  : ASCII fallback, treated identically to 🧩.

-   **Lifecycle**: Generated and maintained by tools. Manual edits
    should be limited to tool authors (see Section 7.3).

**2.3. Human Annotation Marker**

-   📌: Human-generated and maintained.

-   \*-&gt; : ASCII fallback.

-   **Lifecycle**: Not intended for automated modification.

**2.4. AI Annotation Markers**

-   🤖: Reserved for machine-generated metadata and analysis.

-   \*-&lt; : ASCII fallback.

-   **Usage**: Tags prefixed with 🤖 are typically added by AI tools to
    record confidence levels, detected patterns, or refactoring
    suggestions.

    -   Example: // \[🤖confidence-low: auth/logic\] Complex business
        logic, needs more context

    -   Example: // \[🤖refactor-suggestion::\] Consider extracting a
        strategy pattern

-   These may include additional qualifiers such as from=human to
    explicitly mark human-intended metadata for AI tools.

-   Tools MAY process, ignore, or act upon these tags based on their
    function.

**2.5. Ignored/User-Defined Markers**

Other emoji or prefixes (e.g., ❓) are outside this spec and ignored by
conforming parsers.

**2.5. Hierarchy Separator**

-   / defines tag hierarchy (e.g., form/login/username).

**2.6. Global Identifier Separator**

-   :: combines file path and tag path for unique project-wide
    identifiers (e.g., src/components/login.ts::form/login/username).
    This can be used for providing a reference to a section in a project
    to an AI, for example.

**2.7. Tag Syntax**

-   Block: \[🧩 Section: Name\] ... \[/🧩 Section: Name\]

-   Point: \[🧩 Point: Name\]

-   Keywords are case-sensitive; parsers MAY be flexible with whitespace
    for readability.

**3. \[File Info\] Block**

**3.1 Purpose**

File-level metadata block used by tools for project-wide indexing,
automation, versioning, licensing, authorship, and state tracking.

**3.2 Syntax**

\[🧩 File Info\] ... \[/🧩 File Info\]

-   Lines inside the block use standard comment syntax and key: value
    pairs.

-   Optional \[📌 Note: ...\] lines provide developer context but are
    not processed by tools.

**3.3 Example**

// \[🧩 File Info\]

// path: src/lib/utils.ts

// author: Your Name

// created: 2025-05-21

// updated: 2025-05-21T12:00:00.000Z

// description: Core module for data processing and formatting logic.

// license: MIT

// generator: plop

// editable: no

// puzzlepins\_version: 1.0

// puzzlepins\_specification\_url:
https://github.com/robokeys/puzzle-pins/SPEC.md

// \[📌 Note: For detailed information on the normalization algorithm
see wiki/normalize.md\]

// \[📌 Note: This file's metadata conforms to PuzzlePins v1.0.\]

// \[/🧩 File Info\]

**3.4 Key Information**

-   All fields are optional, but some (e.g., path, editable, generator)
    are commonly used by tools.

-   Values should be concise and appear on a single line.

-   Dates should be ISO 8601 format (date or datetime).

-   Notes (📌) are free-form and for human context only.

-   Fields represent the last time PuzzlePins-aware tooling was run, not
    necessarily the last time the file was edited.

**3.5 Management**

-   The \[🧩 File Info\] block is usually generated and updated by
    scaffolding tools or formatters.

Example of a minimal tool-generated block:

// \[🧩 File Info\]

// path: src/lib/utils.ts

// generator: plop

// editable: no

// license: MIT

// \[/🧩 File Info\]

-   Manual editing is permitted for adding custom metadata, but should
    be done cautiously and consistently, especially in collaborative or
    tool-integrated environments.

-   **Management**: Tool-generated; manual editing should be cautious
    and rare.

**4. Tool-Driven Structure Tags (🧩)**

**4.1. Section Tags**

-   Syntax: \[🧩 Section: Path/Name\] ... \[/🧩 Section: Path/Name\]

-   Purpose: Logical grouping for navigation, generation, and
    automation.

-   **Example**:

> // \[🧩 Section: auth/login/formFields\]
>
> const username = '';
>
> const password = '';
>
> // \[/🧩 Section: auth/login/formFields\]

**4.2. Point Tags**

-   Syntax: \[🧩 Point: Path/Name\]

-   Purpose: Precise anchors for tool operations (e.g., code insertion).

-   **Example**:

-   // \[🧩 Point: hooks/preLoginValidation\]

**4.3. Region Tags**

-   Syntax: \[🧩 Region: Type: Path/Description\] ... \[/🧩 Region:
    Type: Path/Description\]

-   Types: Editable, NonEditable, EditableWithCaution

**Type Behaviors:**

-   **Editable**: Tool-generated block with space for safe manual
    editing. Tools should preserve and not overwrite manual changes.

-   **NonEditable**: Tool-managed block. Tools may overwrite contents.
    Manual changes are discouraged or invalid.

-   **EditableWithCaution**: Semi-safe block. Tools may generate or
    reprocess contents under certain conditions. Manual changes are
    allowed but may be fragile or require re-verification. Tooling may
    emit warnings when changes are detected.

-   The first segment after Region: (and ending at the first colon) must
    be a recognized type (Editable, NonEditable, or
    EditableWithCaution). The remainder of the string after the second
    colon is treated as the Path/Description. Colons within the
    Path/Description are permitted but may reduce readability. Parsers
    SHOULD treat the first colon as the type delimiter and the second as
    the start of the path.

-   **Example**:

> // \[🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // Developer can add extra navigation items here
>
> // \[/🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // \[🧩 Region: NonEditable: system/core/boot\]
>
> // DO NOT MODIFY: Generated startup sequence
>
> // \[/🧩 Region: NonEditable: system/core/boot\]
>
> // \[🧩 Region: EditableWithCaution: data/schema/inference\]
>
> // ⚠️ Changes here may be overwritten by schema sync tools
>
> // \[/🧩 Region: EditableWithCaution: data/schema/inference\]

-   The first segment after Region: (and ending at the first colon) must
    be a recognized type (Editable, NonEditable, or
    EditableWithCaution). The remainder of the string after the second
    colon is treated as the Path/Description. Colons within the
    Path/Description are permitted but may reduce readability. Parsers
    SHOULD treat the first colon as the type delimiter and the second as
    the start of the path.

-   **Example**:

> // \[🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // Developer can add extra navigation items here
>
> // \[/🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // \[🧩 Region: NonEditable: system/core/boot\]
>
> // DO NOT MODIFY: Generated startup sequence
>
> // \[/🧩 Region: NonEditable: system/core/boot\]

-   The first segment after Region: must be a recognized type (Editable,
    NonEditable, EditableWithCaution). The second segment is treated as
    the path or name. Colons within the path or name are discouraged and
    should be escaped or avoided to prevent parsing ambiguity. Parsers
    SHOULD treat the first colon as the type delimiter and the second as
    the path delimiter.

-   **Example**:

> // \[🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // Developer can add extra navigation items here
>
> // \[/🧩 Region: Editable: layout/navbar/extraLinks\]
>
> // \[🧩 Region: NonEditable: system/core/boot\]
>
> // DO NOT MODIFY: Generated startup sequence
>
> // \[/🧩 Region: NonEditable: system/core/boot\]

**5. Human-Driven Annotation Tags (📌)**

**5.1. Note Tags**

-   Syntax: \[📌 Note: Text\]

-   **Example**:

> // \[📌 Note: Consider refactoring this loop for clarity in v2.1\]

**5.2. Section Tags**

-   Syntax: \[📌 Section: Path/Name\] ... \[/📌 Section: Path/Name\]

-   **Example**:

> // \[📌 Section: PaymentLogic/SecurityAuditTrail: Audited by J.Doe on
> 2025-05-15\]
>
> // ... implementation ...
>
> // \[/📌 Section: PaymentLogic/SecurityAuditTrail\]

**5.3. Point Tags**

-   Syntax: \[📌 Point: Path/Name\]

-   **Example**:

> // \[📌 Point: ComplexAlgorithmStart: See design\_doc\_v3.pdf for
> detailed rationale\]

**5.4. Region Tags**

-   Syntax: \[📌 Region: OptionalType: Path/Description\] ... \[/📌
    Region: OptionalType: Path/Description\]

-   **Example**:

> // \[📌 Region: Example: UserFlowVisualization: This mock-up
> illustrates the checkout sequence.\]
>
> // ... diagram code or description ...
>
> // \[/📌 Region: Example: UserFlowVisualization\]

**6. Global Project Identifiers**

Global Project Identifiers provide a unique way to reference any
PuzzlePin tag across an entire project. Ideally, all tag paths (e.g.,
form/login/username) within a single file should be unique to avoid
ambiguity. The following format and rules apply:

-   Format: &lt;RelativePath&gt;::&lt;TagPath&gt;

-   If multiple tags exist with the same path, tools MAY reference them
    using indexed disambiguation (e.g., \[2\] for the second
    occurrence).

-   Tools SHOULD usually default to resolving the **first** instance of
    a tag path unless a disambiguation index is specified.

-   Parsers MAY differ in resolution strategy depending on the file
    structure or tool logic.

-   Duplicate tag names are allowed across unrelated parts of a file or
    project, but may introduce ambiguity.

-   **Nested sections using the same name (e.g., auth/login inside
    another auth/login) MUST be avoided.** This can create confusing
    structures that are difficult to resolve reliably and clearly.

-   Flat structures with distinct tag paths (e.g., auth, auth/login) are
    allowed but discouraged if they may be confused as related
    hierarchies.

-   There is no implied parent-child requirement between tags like \[📌
    Section: PaymentLogic\] and \[📌 Section:
    PaymentLogic/SecurityAuditTrail\]. If they appear in different
    contexts or locations, this can be misleading.

-   This is a metadata standard intended primarily for automated tooling
    where tag uniqueness and clarity are typically enforced. However,
    since human authors may edit or misuse tags, tools MUST tolerate and
    handle imperfect or redundant structures. Clean, non-redundant
    structure is strongly encouraged.

**Examples:**

-   src/lib/api.ts::UserEndpoint/TokenSection

-   src/lib/api.ts::UserEndpoint\[2\]/TokenSection — references the
    second matching UserEndpoint block.

-   templates/header.html::layout/navigation

**7.1 Tag Path Disambiguation Rules**

**Clarification:**

PuzzlePins supports indexed disambiguation of duplicate tag paths **only
in global references**, such as
src/lib/api.ts::UserEndpoint\[2\]/TokenSection. This indexing is a
tool-side resolution strategy and is **not valid inside tag
declarations**. Tags must remain clean and literal.

-   ❌ \[📌 Section: PaymentLogic/SecurityAuditTrail\[2\]\] →
    **Invalid**

-   ✅ \[📌 Section: PaymentLogic/SecurityAuditTrail-2\] → **Valid**,
    and preferred

**Why It Exists:**

-   Tools need deterministic ways to handle accidental duplication.

-   Indexing avoids failure or silent misbehavior when structure isn't
    ideal.

**Tool Behavior:**

-   Tools MUST NOT treat \[2\] as part of the tag itself.

-   Parsers SHOULD warn if multiple identical tag paths are found.

-   Default behavior SHOULD resolve to the first matching tag unless an
    index is explicitly provided.

-   Tools and developers SHOULD avoid relying on \[N\] disambiguation
    except when resolving accidental duplicates.

**Recommendations:**

-   Tag paths SHOULD be designed to remain globally unique within a file
    to ensure clear and deterministic references.

-   Avoid duplicates; use descriptive suffixes instead.

-   If -2, -admin, or -alt naming avoids index disambiguation, prefer
    that.

-   Tools SHOULD highlight and help eliminate ambiguous structures.

**7.2 Human-Centric Annotation (📌 Tags)**

-   Encourage inline documentation, rationale sharing, and team
    onboarding.

-   Example: \[📌 Note: This uses a deprecated pattern. See
    wiki/RefactoringRoadmap.\]

**7.3 Manual Modification Warnings**

-   Manual edits to 🧩 tags risk tool malfunction and data loss.

**7.4 Team Usage**

-   Tag changes should be reviewed in PRs.

-   PuzzlePins tags—especially 🧩 tags—are not just comments; they are
    structured metadata with functional impact on tooling and
    automation. As such, they should be treated as first-class citizens
    in the codebase.

-   Review processes should consider tag accuracy, relevance, and
    clarity in the same way functional code is reviewed.

-   Encourage living documentation with both 🧩 and 📌 tags to ensure
    the codebase remains both tool-friendly and human-readable.

**8. Use Cases**

**8.1. Tooling Interaction**

-   🧩 defines code generation boundaries, insertion points, and
    replaceable sections.

**8.2. Developer Communication**

-   📌 improves documentation, code reviews, and onboarding.

**8.3. Metadata in PRs**

-   Tag changes are valid, reviewable code changes.

**8.4. General Clarity**

-   🧩 defines structure for tools.

-   📌 enriches understanding for humans.

**9. Extensibility**

-   This spec is designed for evolution. New markers, keywords, and
    metadata can be added as AI/code collaboration practices mature.

**10. AI-Generated Annotations (🤖 Tags)**

PuzzlePins also supports a special 🤖 marker (with an ASCII backup of
\*-&lt; ) to indicate AI-authored annotations that are neither
structural (🧩) nor human notes (📌), but rather machine-inferred
metadata, suggestions, or insights. These tags are intended to be
processed primarily by AI tooling or internal diagnostics but MAY also
provide value to humans in specific contexts. So while these tags are
intended to be read by both humans and tools they are **authored by
AI**.

**10.1 Syntax Examples**

// \[🤖confidence-low: marker1:\] Complex business logic, needs more
context

// \[🤖pattern-detected: marker1:\] Observer pattern implementation

// \[🤖refactor-suggestion::\] Consider extracting strategy pattern

**10.2 Characteristics**

-   The first segment after 🤖 denotes a label or classification (e.g.,
    confidence-low, pattern-detected).

-   A second optional colon-separated key (e.g., marker1) may indicate
    an internal tag or cross-reference.

-   The value after the final colon is free-form text.

**10.3 Use Cases**

🤖 tags are typically used for annotations that AI tooling needs to
track independently of human-authored intent. For example:

-   Tracking confidence or uncertainty for machine-generated sections

-   Suggesting design patterns or possible refactors (optionally
    accompanied by 📌 tags for human review)

-   Embedding machine-specific metadata or guidance that supplements but
    does not replace human-readable comments

Most human-visible notes that AI wishes to communicate should be
expressed via 📌 tags for clarity. 🤖 tags should be reserved for
metadata and situations where the machine origin is especially relevant.

**10.3.1 Human-to-AI 🤖 Notes**

PuzzlePins allows humans to leave structured notes *to AI tools* using
the from=human property inside a 🤖 tag. This provides a clean,
machine-parseable way to:

-   Prevent unwanted tool modifications

-   Offer human intent or rationale for non-obvious code

-   Trigger tool behavior in downstream passes

**Syntax:**

// \[🤖suppress-edit: from=human: legacy/auth/tokenFlow\] This logic is
verified by legal — do not modify

// \[🤖context-hint: from=human: api/geoHandler\] Important: this
override supports GDPR region targeting

These notes are not instructions to humans, but metadata specifically
meant for future AI or automated agents to parse, respect, or respond
to.

Tooling MAY treat these notes differently from AI-authored 🤖 tags, for
example:

-   Highlighting them in IDEs

-   Elevating them as soft constraints

-   Logging or persisting them for future passes

🤖 tags are typically used for annotations that AI tooling needs to
track independently of human-authored intent. For example,:

-   Tracking confidence or uncertainty for machine-generated sections

-   Suggesting design patterns or possible refactors (optionally
    accompanied by 📌 tags for human review)

-   Embedding machine-specific metadata or guidance that supplements but
    does not replace human-readable comments

Most human-visible notes that AI wishes to communicate should be
expressed via 📌 tags for clarity. 🤖 tags should be reserved for
metadata and situations where the machine origin is especially relevant.

-   Highlighting areas for AI-based review or uncertainty

-   Suggesting design patterns or refactor targets

-   Annotating segments for potential improvement, based on code
    structure or patterns

**10.4 Tooling Guidance**

-   Tools MAY emit or consume 🤖 tags during code analysis.

-   🤖 tags SHOULD be treated as advisory, and may coexist with 🧩 or 📌
    tags.

-   🤖 tags MUST NOT change file structure or affect parsing of 🧩
    regions.

-   Human editors MAY choose to remove or upgrade 🤖 annotations to 📌
    tags if validated.

This marker enables AI to build up layered insights in codebases over
time, without conflicting with human or tool-managed sections.

-   This spec is designed for evolution. New markers, keywords, and
    metadata can be added as AI/code collaboration practices mature.

**Appendix A: Linter Rules and Anti-Patterns**

To support consistent and predictable use of PuzzlePins, tools SHOULD
implement the following linting rules and heuristics:

**A.1 Tag Uniqueness**

-   Rule: Within a single file, full tag paths SHOULD be unique.

-   Violation Example:

> // \[🧩 Section: auth/login\]
>
> ...
>
> // \[/🧩 Section: auth/login\]
>
> ...

-   // \[🧩 Section: auth/login\] // ❌ Duplicate

-   Suggested Fix: Rename one section or use a descriptive suffix.

**A.2 Nested Duplicate Tags**

-   Rule: Tags with the same name MUST NOT be nested inside each other.

-   Violation Example:

> // \[🧩 Section: auth/login\]
>
> // \[🧩 Section: auth/login\] // ❌ Invalid nesting
>
> ...
>
> // \[/🧩 Section: auth/login\]
>
> // \[/🧩 Section: auth/login\]

**A.3 Misuse of Disambiguation in Tag Declarations**

-   Rule: \[N\] syntax is ONLY allowed in global references—not in tag
    declarations.

-   Violation Example:

> // \[📌 Section: config/routes\[2\]\] // ❌ Invalid

-   Suggested Fix:

> // \[📌 Section: config/routes-2\] // ✅ Acceptable

**A.4 Missing End Tags**

-   Rule: Block tags (Section, Region) MUST have a corresponding end
    tag.

-   Violation Example:

> // \[🧩 Section: setup/env\]
>
> const env = "dev"; // ❌ Missing \[/🧩 Section: setup/env\]

**A.5 Overlapping Regions with Conflicting Types**

-   Rule: Regions with incompatible editability types SHOULD NOT overlap
    unless deliberately structured.

-   ✅ **Editable inside NonEditable**: Allowed. Developers may wish to
    indicate editable comments or documentation within otherwise
    tool-managed content.

-   ⚠️ **NonEditable inside Editable**: Allowed but discouraged. It
    suggests part of a human-editable region is off-limits to editing,
    which can be confusing unless well-documented.

-   ✅ **NonEditable inside EditableWithCaution**: Acceptable. Often
    used to carve out tool-owned areas inside otherwise semi-editable
    sections.

-   ⚠️ **Editable inside EditableWithCaution**: Allowed but discouraged.
    In general, avoid this as it reduces clarity on risk boundaries.

-   Violation Example (unclear intent):

> // \[🧩 Region: Editable: api/data\]
>
> // \[🧩 Region: NonEditable: api/data/cache\] // ⚠️ Ambiguous intent
>
> // \[/🧩 Region: NonEditable: api/data/cache\]
>
> // \[/🧩 Region: Editable: api/data\]

-   Valid Example:

> // \[🧩 Region: EditableWithCaution: api/data\]
>
> // ⚠️ You may edit this section, but changes might be regenerated.
>
> // \[🧩 Region: NonEditable: api/data/derived\]
>
> // Do not touch this derived section.
>
> // \[/🧩 Region: NonEditable: api/data/derived\]
>
> // \[/🧩 Region: EditableWithCaution: api/data\]
>
> // \[🧩 Region: Editable: api/data\]
>
> // \[🧩 Region: NonEditable: api/data/cache\] // ⚠️ Unclear intent
> unless well-documented
>
> // \[/🧩 Region: NonEditable: api/data/cache\]
>
> // \[/🧩 Region: Editable: api/data\]

-   Valid Example:

> // \[🧩 Region: EditableWithCaution: api/data\]
>
> // ⚠️ You may edit this section, but changes might be regenerated.
>
> // \[🧩 Region: NonEditable: api/data/derived\]
>
> // Do not touch this derived section.
>
> // \[/🧩 Region: NonEditable: api/data/derived\]
>
> // \[/🧩 Region: EditableWithCaution: api/data\]

-   Rule: Avoid overlapping regions where he outer one is marked
    Editable and the inner is NonEditable.

-   Violation Example:

> // \[🧩 Region: Editable: api/data\]
>
> // \[🧩 Region: NonEditable: api/data/cache\] // ⚠️ Conflict

**A.6 Tool Guidance on Violations**

-   Tools SHOULD:

    -   Warn on duplicates

    -   Error or Warn on invalid nesting or tag syntax

    -   Offer auto-fix suggestions when practical

    -   Highlight ambiguous paths that trigger fallback behavior

These rules help ensure that PuzzlePins tags are both human-legible and
reliably machine-processed.
