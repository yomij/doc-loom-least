# TDD Exceptions

Only an approved plan or confirmed amendment may declare an exception; it never
removes verification.

Allowed: pure docs, config, build script, logging/telemetry, UI copy, dead-code
deletion, exploratory spike, urgent hotfix, or an environment change that
cannot be automated reliably.

Record category/reason and concrete alternative verification, including manual
steps/environment/result/limitations when applicable. Behavior-preserving
refactors use `characterize -> refactor -> verify`. Never invent a meaningless
Red.
