---
name: art
description: >
  Use when writing, refactoring, or reviewing code. Covers design boundaries,
  dependencies, types, error handling, config and secrets, tests, and docs,
  so that the code feels like art.
license: MIT
---

# Art

> "_Code is an art: Everybody can draw, but only few can actually paint._"

These are guidelines and intuitions to help you write the right code.
Most rules come with a reason. When a case is not covered, follow the reason, not the wording.

## Design

- The best code is the one that is never written. See skill "ponytail" for more, if available.
    - Short version: solutions should be clear and concise.
- Workarounds and solutions that go against the philosophy of programming, the current tool, or the language being used are not allowed.
    - Why: code that fights its tools is hard to read for everyone who knows those tools.
    - If a rule in this skill conflicts with the idioms of the language, the language wins.
- There should be clear and clean boundaries in code, which includes: domain concepts, clear tasks and responsibilities with boundaries, high and clear separation of concerns.
- Code should be organized hierarchically into modules.
- Modules should have loose coupling.
    - Why: you should be able to change one module without reading the others.

## Dependencies

- Use existing libraries for non-trivial problems before writing your own solutions.
- Do not add a dependency for something you can write clearly in a few lines.
    - Why: every dependency is code you did not write, but still own.
- A library must be stable and actively maintained (recent releases, issues get answers), and at least one of the following:
    - Widely adopted (stars are a hint, not proof).
    - From an "official" source, e.g. an organization, well known person, or enterprise.

## Types and tooling

- Everything should be typed/typesafe and annotated.
- Code needs to be formatted, linted, …
- Use the tooling supported by the language (e.g. `cargo fmt` + `clippy`, `gofmt` + `go vet`, `ruff` + `ty`).
    - Why: standard tools mean no style debates and no surprises for other people.

## Error handling

- Exception and optional result handling like done in Rust is beautiful. Use it where the language supports it naturally (Rust, Go, Kotlin, Swift, TypeScript with discriminated unions).
- In languages built around exceptions (Python, Java), use exceptions, but keep the spirit:
    - Make failure explicit in signatures and docs.
    - Catch narrow, handle close to the cause, never swallow silently.
    - Use optional types (`Optional`, `X | None`) instead of magic values like `-1` or `""`.

## Config, secrets and security

- Configs should be read from files (at best YAML). The file is the source of truth.
    - Env vars are allowed only as deployment overrides, e.g. when Docker or Kubernetes injects them.
- Secrets must be read from files (e.g. Docker secrets, mounted Kubernetes secrets), never from env vars or code.
    - Why: env vars leak into process listings, crash dumps, logs and child processes.
- Aim for hardened approaches with least privileges.
    - Why: what a component cannot do, it cannot do wrong.

## Tests

- Do not create tests just for the point of having tests.
    - E.g. a useless test is something that is very obvious: putting data in some container and then checking the value in the container.
- Tests should be created where required, but they should have a clear-cut reason to exist.
    - Good reasons: behavior at boundaries, edge cases, bugs that happened before, contracts other code depends on.
- Test behavior, not implementation. A refactor should not break a good test.

## Docs and comments

- Every public function, class, module, struct, … must be documented, in the way the language supports.
- Private items need docs only when they are not obvious.
    - `fn name() -> &str` does not need `/// Returns the name.`
- Doc comments should never be longer than two small paragraphs.
- Other comments of general code pieces should be brief but clear, and are welcome.
    - Comments explain why. The code explains what.
- Docs are good and encouraged, but should be formatted well with frequent and clear boundaries.
    - No one wants to read a 100 sentence paragraph, in a file with 10 of those paragraphs.
- All docs and comments should be, unless unsuited, in ASD-STE 100 (Simplified Technical English). Core rules:
    - Max 20 words per sentence for instructions, 25 for descriptions.
    - Use the active voice.
    - One instruction per sentence.
    - Use simple, common words with one meaning. Use one term for one concept.
    - Write instructions in the imperative ("Close the file", not "The file should be closed").

## Misc

- Always use conventional commits and general automatic tooling (e.g. semantic release) when available.

## The bar

You should be able to show your code with confidence to other people, without being afraid that they might not find it right.
If you are unsure to present your solution to Google or Meta Staff Engineers, then you might have not reached a good enough solution.

Concrete test: could a new teammate understand each module in five minutes, without asking you?

You should be proud of the code. If you aren't, then the code is probably not sufficient.

On a meta level:

- It needs to be intuitive.
- It should have good "taste".
- It should somewhat be elegant and pleasing to the eye.

## Process

- Sometimes it is best to first create an imperfect solution, and then improve and refactor it iteratively to reach these criteria.
- If a clean and nice solution is not possible due to the guidelines given above, don't develop it, but play back the problem and ask for confirmation to still solve it while "breaking" some of the rules above.
    - Ask once per problem, not once per rule. List every rule you would break, and why.
- If something simply cannot be solved, found, or simplified, that is absolutely okay and also a result. There is no such thing as infinite improvement.

These guidelines will inevitably lead to "_damned if you do, damned if you don't_". This is a good sign: it usually means we reached the end of the design, and there exists no better solution.
