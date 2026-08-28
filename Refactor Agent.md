Yes, writing a parallel new package alongside the existing one is actually a safer and more professional refactoring strategy, especially for large or mission-critical codebases.
This approach is known in software architecture as the Parallel Change Pattern (or the Strangler Fig Pattern).
Why This Approach Is Superior
 * Zero Risk to Production: The working code remains untouched and execution-ready while the new structure is built.
 * Side-by-Side Comparison: You can write integration or unit tests that run input against both the old package and the new package simultaneously to verify identical outputs.
 * Safer AI Refactoring: AI models sometimes hallucinate or silently drop edge cases during direct in-place refactoring. Building a standalone package forces the model to construct clean contracts and interfaces from scratch.
 * Easy Rollback: If the new package fails tests or lacks functionality, you don't need to revert git commits—you simply keep using the original package.
Updated Agent Refactoring Prompt (New Package Strategy)
Use this prompt to instruct your AI agent to follow the parallel package pattern:
> Role & Objective
> You are an expert software architect. You will create a brand-new, refactored package/module that reproduces all functionality of the existing codebase. The new package must follow SOLID principles, clean architecture, and modern design patterns. Do not modify or delete any code in the existing package.
> Core Rules
>  * Parallel Construction: Build the entire refactored code inside a new directory/package (e.g., pkg_v2 or refactored/).
>  * Strict Parity: All public functions, outputs, and side effects must produce identical results to the legacy package.
>  * Clean Architecture: Use Single Responsibility (SRP), clean interface abstractions (DIP), dependency injection, and guard clauses.
>  * Zero Shared State: The new package must operate independently without relying on internal classes of the old package.
> Step-by-Step Plan
>  * Step 1: Domain & Contract Mapping
>    * Analyze the old package and define the public interfaces/contracts for the new package.
>    * Show me the proposed directory structure and core interfaces.
>    * Wait for my approval before writing concrete implementation code.
>  * Step 2: Implementation of the New Package
>    * Write the complete, production-ready code inside the new package.
>    * Include brief inline documentation explaining architectural decisions.
>  * Step 3: Verification & Migration Strategy
>    * Provide unit tests comparing the outputs of the old package vs. the new package for identical inputs.
>    * Provide a step-by-step checklist to safely swap imports from the old package to the new package.
> 
Recommended Migration Workflow
[ Legacy Package: pkg_v1 ] (Active)
         │
         ▼  (1. Build alongside)
[ New Package: pkg_v2 ]    (Under test)
         │
         ▼  (2. Run side-by-side test verification)
[ Swap Imports to pkg_v2 ] 
         │
         ▼  (3. Confirm stable & delete pkg_v1)

 * Build pkg_v2: Let the AI create the full implementation in the new folder.
 * Run Dual Tests: Pass real test payloads through both pkg_v1 and pkg_v2 to verify that outputs match 100%.
 * Switch References: Change your application's imports/instantiations to point to pkg_v2.
 * Decommission pkg_v1: Once pkg_v2 has run successfully in your test/staging environment, safely delete the old package directory.
