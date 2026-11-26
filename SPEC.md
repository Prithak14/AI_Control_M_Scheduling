# Specification:

Scope: an AI augmentation of the existing Control-M report.

## Problem and goal

Help a reviewer explain a reported scheduling finding using supplied evidence and a repeatable AI instruction. The existing Python report remains the source of the finding. This increment does not calculate or approve an alternative schedule.

## Workflow

1. A reviewer runs or obtains the existing "Control_M_Report.xlsx" using the established process.
2. The tool extracts one "Scheduling_Conflicts" row, aliases both application names, and constructs a structured, source-referenced finding.
3. The tool creates a prompt with versioned instructions. The reviewer checks the prompt and any data-sharing approval before manually using an approved LLM.
4. The reviewer validates the response against the finding and operating procedure. Any scheduling change remains subject to the scheduling team's process.

## Requirements and tasks

- read-only integration: accept one existing report conflict row with the expected headers; never modify the workbook. 
- minimisation: replace application identifiers with `Job A` and `Job B`; do not pass source free-text notes or workbook paths to the prompt.
- provenance and uncertainty: preserve workbook cell references, label dates as samples, and include limitations of the existing generator.
- context and trust boundary: prepare instructions that treat finding values as untrusted data, prohibit unsupported claims and require human review.
- response contract: request JSON with finding ID, explanation, evidence references, uncertainties and review actions. 
- safe operation: no networking or production scheduling changes, no automatic upload, and no silent overwrite of user files.

## Acceptance criteria

- A report fixture with the same "Scheduling_Conflicts" layout produces an aliased finding with exact source cell references and no raw application names.
- Missing sheets, incorrect headers, blank rows and malformed fields are rejected rather than silently producing a finding.
- Existing report generation and workbook files are not changed.

## Boundaries and unresolved decisions

- The original report excludes some overlaps, uses a 2026 calendar, ignores US holidays in first-business-day calculation, and approximates frequency when proposing slots. It cannot establish all actual overlaps, a safe schedule or production status.
- Eliza integration, a deployed custom skill, reviewer identity, model choice, approved data classifications, actual SOP sharing rights, success thresholds and business rules require client input.