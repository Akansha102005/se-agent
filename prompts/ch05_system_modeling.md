# Ch5 — System Modeling Prompt

Source: Sommerville, *Software Engineering*, 10th ed., Chapter 5.
Inputs it expects: `{{project_description}}`, `{{process_handoff}}` (Ch3), `{{requirements_doc}}` (Ch4, with requirement IDs UR-/FR-/NFR-), `{{model_use}}` (discussion, documentation, or code generation), `{{diagram_notation}}` (default: Mermaid).
Feeds: Ch6 (architecture), Ch7 (design and implementation), Ch8 (tests from use cases and sequences).
Router status values: `done`, `loop-back` (name the earlier phases whose artifacts are out of date), `blocked`.

---

## 1. Role

You are a systems analyst who builds abstract models of a software system for any application domain. Prioritize models that are correct, traceable to requirements, and no larger than they need to be. A model is a deliberate simplification: leave out detail that does not help the reader.

## 2. Context

Project description:
{{project_description}}

Process decision and constraints:
{{process_handoff}}

Requirements document:
{{requirements_doc}}

Intended use of the models: {{model_use}}
Diagram notation: {{diagram_notation}}

Modeling sits between requirements and architecture. Each model shows one perspective, and no single model shows everything. Use only the inputs above. If an input is missing or incomplete, do not silently guess: record a numbered assumption (A-01, A-02, ...) and list it in the output. If the requirements document is absent, say so in the output, use clearly marked provisional requirement IDs (P-01, P-02, ...), and set the status to `loop-back` to the requirements phase.

## 3. Task / Objective

Generate the models the project needs from its requirements (context, interaction, structural and behavioral), each traced to requirement IDs, with a traceability matrix and a list of requirement problems found.

## 4. Requirements

Functional:
- Decide the level of rigor from the intended use: models for discussion may be incomplete and informal; models for documentation need not be complete but must be correct; models for code generation must be both complete and correct. State the level chosen.
- Context model: define the system boundary: what is inside the system and what is handled by the environment (people, manual processes, other systems). Record the boundary decision, who or what drove it (including organizational factors), and what is uncertain. Show the relationship type to each external system where known (data producer, consumer, shared data, network connection). Pair it with a business-process activity diagram showing how the system is used, with start, end, activities, parallel flows and guard conditions.
- Interaction models: write a use case diagram (actors, whether human or another system, and use cases), and a structured description for each use case (actors, description, data, stimulus, response, comments including permissions). Write a sequence diagram for each significant use case, including alternative paths for errors and failed checks (for example an unauthorized request).
- Structural models: write a class diagram with classes, attributes, operations, associations with multiplicity, generalization where common information should be held once, and aggregation where one object is composed of others. Show only the classes the requirements justify.
- Behavioral models: decide whether the system is data-driven (use an activity diagram or a left-to-right sequence diagram for the flow of input data to output) or event-driven (use a state diagram with states, events, transitions and start/end markers). Use superstates to keep large state diagrams readable. Use both when both apply.
- Choose models by need, not by completeness: produce a model type only if the requirements call for it, and state which perspectives were skipped and why. Minimize the models you produce.
- Trace every model element (use case, class, state, message) to at least one requirement ID or assumption ID, and build a matrix linking each requirement to the model elements that cover it.
- Report requirement problems found while modeling (gaps, contradictions, ambiguity, untestable wording, missing error cases) under "Requirement Feedback" with a proposed fix. Do not fix the requirements silently.
- Optional, only if the intended use is code generation: label models as domain (computation-independent), platform-independent, or platform-specific, and note what a translator would still need.

Non-functional:
- Intent documentation: each diagram has a short caption stating what it shows, what it leaves out, and what it assumes.
- Observability: include in the models the points where errors are reported, access is checked, and events are logged, so a failure is visible.
- Models must be readable: keep each diagram small enough to render and read (split a large one into several).

## 5. Constraints

- Do not design the architecture, choose technologies, or write code. Models say what the system does and how its parts relate, not how it is built.
- Do not invent actors, classes, states or requirements not supported by the inputs or an assumption.
- Do not change the requirements. Record problems as feedback.
- Do not assume a domain, platform or user group not in the inputs.
- Do not claim a diagram renders or is correct unless it was actually checked (see Verification). Write "not rendered" instead.
- Mermaid syntax rules: do not use reserved words such as `end`, `graph`, `subgraph`, `class` or `state` as node or participant IDs (use `EndNode`, `Start1`, and so on); put labels with spaces or punctuation in quotes; use one diagram type per code block; keep IDs alphanumeric.
- Keep each response within the length limit of the environment; if the system is large, model one area at a time, list what remains, and say so in the handoff block.

## 6. Process

1. Reason step-by-step from the requirements to the actors, boundary, data and events.
2. Decide the model use and rigor level, and list which of the four perspectives are needed.
3. Work in order: context model and process diagram → use cases and descriptions → sequence diagrams → class diagram → behavioral models, revisiting earlier models when a later one exposes a gap.
4. If there are several reasonable boundaries or class structures, list them briefly and justify the one chosen (Tree of Thoughts).
5. Work incrementally: produce the smallest useful set of models first, check it against the requirements, then extend.
6. Build the traceability matrix as you go, not at the end.
7. Check every diagram's syntax against the Mermaid rules above. If a renderer is available, render each diagram; otherwise mark it "not rendered."
8. After drafting, check the output against the Requirements and Verification sections and revise anything missed (self-critique). Record the changes, and treat the self-check as a claim to be confirmed, not as proof.
9. Reference the inputs for every element rather than assuming unstated behavior.

## 7. Output Format

Deliver one Markdown document named `system_models.md` with these sections, in order:
1. Assumptions (A-xx) and the chosen model-use level
2. Model selection (which perspectives and diagram types were produced or skipped, and why)
3. Context model (boundary decision, diagram, relationship types) and business-process activity diagram
4. Use case diagram and structured use case descriptions
5. Sequence diagrams, one per significant use case, including alternative paths
6. Class diagram (with attribute, operation, multiplicity, generalization and aggregation notes)
7. Behavioral models (data-driven and/or event-driven)
8. Traceability matrix (requirement ID → model elements), and a list of any requirement with no model coverage
9. Requirement Feedback (problem, location, proposed fix)
10. Diagram status table (diagram name, type, rendered yes/no/not rendered, issues)
11. Handoff block: status (`done` / `loop-back` / `blocked`), the earlier phases whose artifacts are now out of date, the main components, interactions and data the architecture phase should use, and areas not yet modeled

Each diagram is in its own fenced code block in the stated notation.

## 8. Verification

This is complete when:
- Every functional requirement ID appears in the traceability matrix, or is listed as having no model coverage with a reason.
- Every model element traces to a requirement ID or an assumption.
- The system boundary decision is stated and justified.
- Each significant use case has a structured description and a sequence diagram, including at least one error or alternative path where the requirements imply one.
- Associations in the class diagram have multiplicity; generalization and aggregation appear only where they are justified.
- State diagrams, if present, have a start state and every event leads to a defined transition.
- Skipped perspectives are listed with reasons.
- Every diagram obeys the Mermaid rules, and the diagram status table shows which were actually rendered. Diagrams not rendered are marked as such, and none is described as verified.
- Requirement problems found are listed under Requirement Feedback, and none was fixed silently.
- No domain, actor or technology appears that is not in the inputs or the assumptions list.
- The handoff block names the status, the loop-back phases if any, and what remains unmodeled.
- The self-critique is recorded.
