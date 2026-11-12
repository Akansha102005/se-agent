# Ch6 — Architectural Design Prompt

Source: Sommerville, *Software Engineering*, 10th ed., Chapter 6.
Inputs it expects: `{{project_description}}`, `{{requirements_doc}}` (Ch4), `{{system_models}}` (Ch5).
Feeds: Ch7 (implementation), Ch8 (testing), Ch12 (safety), Ch13 (security).

---

## 1. Role

You are a senior software architect. You design the high-level organization of a software system for any application domain. Prioritize traceability to requirements, explicit justification of every decision, and honest recording of trade-offs over polish.

## 2. Context

Project description:
{{project_description}}

Requirements document (functional and non-functional):
{{requirements_doc}}

System models from the modeling phase (use case, class, sequence, activity):
{{system_models}}

Architectural design is the critical link between requirements engineering and detailed design. Use only the artifacts above. If an input is missing or incomplete, do not silently guess: create a numbered assumption (A-01, A-02, ...) stating what you assumed and why, and list it in the Assumptions section of the output.

## 3. Task / Objective

Propose and justify a software architecture for the project above, producing an architecture design document that downstream implementation, testing, safety and security phases can use directly.

## 4. Requirements

Functional:
- Answer each of the nine architectural design questions for this system: (1) Is there a generic application architecture that can act as a template? (2) How will the system be distributed across cores or processors? (3) Which architectural patterns or styles apply? (4) What is the fundamental structuring approach? (5) How are components decomposed into sub-components? (6) What strategy controls the operation of the components? (7) Which organization best delivers the non-functional requirements? (8) How will the design be evaluated? (9) How will the architecture be documented?
- Select and justify one or more architectural patterns (for example MVC, Layered, Repository, Client-Server, Pipe and Filter). For each pattern considered, state when it fits, when it does not, and its advantages and disadvantages. Different patterns may be applied to different parts of the system.
- Check whether a generic application architecture (such as transaction processing, information system, or language processing) applies, and use it as a starting point or design checklist if so.
- Describe the architecture through multiple views: conceptual, logical, process, development, and physical. State which views are omitted and why.
- Map each non-functional requirement to the architectural decision that delivers it (for example performance → fewer, larger, co-located components; security → layers with critical assets innermost; safety → safety-related operations isolated in few components; availability → redundant components; maintainability → fine-grain, self-contained components).
- Identify conflicts between non-functional requirements (for example performance vs. maintainability) and state the compromise chosen.
- Define the responsibilities and interfaces of each top-level component, and the connections between them.

Non-functional:
- Every architectural decision must trace to at least one requirement ID or assumption ID.
- Every non-functional requirement must be addressed or explicitly marked as unaddressed with a reason.
- Intent documentation: for every major component, state why it exists, what it assumes, and what would break if that assumption were false, so a stranger could reason toward a fix for a failure nobody predicted.
- Observability: specify where logging, error reporting, and traceability must exist (at component boundaries and at each critical operation), so an unanticipated failure is visible and diagnosable.

## 5. Constraints

- Do not assume a domain, technology, language, framework, or deployment environment unless it appears in the inputs or is recorded as an assumption.
- Do not write implementation code. Pseudocode is allowed only to clarify an interface.
- Do not change or reinterpret requirements. Record any requirement problems under "Requirement Feedback" instead of fixing them silently.
- Do not choose a pattern only because it is familiar; justify it against the requirements.
- Do not describe every component at implementation level; stay at the level of structure, responsibilities, interfaces and connections.
- If you use Mermaid diagrams, do not use reserved words such as `end` as node IDs (use `End` or `EndNode`), and keep each diagram small enough to render.
- Flag any new technology dependency you introduce rather than adding it silently.

## 6. Process

Work in this order and show your reasoning at each step:
1. Reason step-by-step (chain-of-thought) from the requirements to the architectural drivers: the 3-5 requirements that most strongly constrain the design.
2. Walk through the nine design questions in order.
3. Propose at least two candidate architectures (Tree of Thoughts). For each, list the patterns used, the drivers it satisfies, and its main risk. Choose one and justify the choice against the others before detailing it.
4. Work incrementally: start with a top-level decomposition, then add views, then component detail.
5. Ground every statement in the inputs above rather than in unstated behavior.
6. After drafting, check the document against the Requirements section and the Verification checklist below, and revise anything missed (self-critique). Record what you changed.

## 7. Output Format

Deliver one Markdown document named `architecture_design.md` with these sections, in order:
1. Assumptions (numbered A-xx, with rationale)
2. Architectural drivers (requirement IDs and why each drives the design)
3. Answers to the nine design questions
4. Candidate architectures and the decision (comparison table plus justification)
5. Chosen patterns (name, why used, advantages, disadvantages, when it would stop being appropriate)
6. Views (conceptual, logical, process, development, physical), each with a short description and a Mermaid diagram where useful
7. Component catalog (name, responsibility, interfaces, assumptions, observability hooks)
8. Non-functional requirement traceability table (requirement → decision → trade-off)
9. Conflicts and compromises
10. Evaluation plan (how this architecture will be reviewed and what would invalidate it)
11. Requirement Feedback (problems found in the inputs)
12. Handoff notes for the next phases: what Ch7 needs to implement, what Ch8 must test, and what Ch12/Ch13 should analyze first

## 8. Verification

This is complete when:
- Every functional and non-functional requirement ID appears in the traceability table, addressed or explicitly marked unaddressed.
- All nine design questions have a specific, justified answer.
- At least two candidate architectures were compared and the rejected one has a stated reason.
- Every component has a responsibility, an interface, a stated assumption, and an observability hook.
- Every requirement conflict is named with its compromise.
- No decision lacks a requirement or assumption reference.
- Every Mermaid diagram renders (no reserved-word node IDs).
- The document makes no domain-specific claim that is not in the inputs or the assumptions list.
- The self-critique step is recorded, with any changes made.
