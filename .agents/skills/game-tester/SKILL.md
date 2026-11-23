---
name: game-tester
description: Run read-only gameplay checks for browser games, including controls, economy, inventory, and day or turn progression.
---

# Game Tester

Test browser-based games for common gameplay and state-transition problems. Report what passed, what failed, and what could not be checked. This skill is for testing only: do not edit game code, assets, configuration, or saves, and do not fix issues unless the user separately asks.

## Testing workflow

1. Find the game's entry point and its stated run instructions. Prefer the existing browser tab or a local, dependency-free entry point. Do not install packages, alter project setup, or publish/deploy the game as part of testing.
2. Start a fresh or clearly resettable game session. Record the browser, launch method, starting state, and any startup errors. Confirm that the game reaches its expected playable screen and that the initial controls are visible.
3. Read the in-game rules and inspect relevant source read-only when needed to establish the intended formulas, limits, and turn behavior. Do not assume a formula or expected result that the game does not state or implement.
4. Exercise the primary controls and the checks below with the smallest useful set of ordinary and boundary cases. Use reversible in-game actions and reset the test session when practical. Preserve existing player progress and saved data.
5. Stop after observing and recording a failure. Do not patch, change settings, clear data, or try a workaround that modifies the game. Report the evidence and leave fixes to the user.

If testing would overwrite persistent progress, submit data to a remote service, require an account, or make another consequential change outside a disposable local session, pause and ask the user before proceeding. If a check cannot be performed safely or reliably, mark it as blocked or not tested and say why.

## Required checks

### Launch

- Open the game using its normal entry point and verify that the playable screen appears, not just a blank page or loading state.
- Check for visible error messages and, when available, browser console or runtime errors that prevent play. Note missing assets or interactions that fail to initialize.

### Buttons and controls

- Exercise each primary gameplay button at least once, including start/play, the main action, end/next turn or day, and restart when safely available.
- Change each relevant input, slider, selector, or keyboard control. Confirm that the game reflects the selected value and that invalid or out-of-range values are rejected or constrained coherently.
- Check that a single action causes one state change, not zero or multiple unintended changes. Note controls that look active but do nothing.

### Calculations

- Derive expected results from the game's visible rules or read-only implementation. Independently calculate at least one normal case and one useful boundary case.
- Check the displayed arithmetic that applies to the game, such as costs, proceeds, totals, profit/loss, resources, score, or remaining balance. Verify that the result and the resulting game state agree.
- Include zero, minimum, maximum, or exact-boundary cases when those values are allowed and relevant.

### Spending and balance

- Try an action whose cost is greater than the available balance. Verify it is blocked or rejected and that the balance and owned items/resources do not change.
- When practical, check an exact-affordability case and confirm that spending the available amount does not produce a negative balance. Compare the shown balance with the independently calculated result.

### Sales and inventory

- Test no inventory, a small inventory, and a case where demand can exceed stock, if the game supports those states.
- Verify that sold quantity is never greater than available inventory, never negative, and that remaining inventory is never negative. Check that revenue uses the actual quantity sold and that stock is deducted consistently.

### Day or turn progression

- Advance once and verify the day/turn counter increases by exactly one and the result shown belongs to the completed turn.
- Verify that carried resources and balance persist as intended and that per-turn values reset or remain according to the game's rules.
- Check that the next turn can be played normally. If a finite run has an end state, verify the final allowed advance and that no extra day/turn is accidentally processed. Test restart only if it will not overwrite user progress.

## Reporting

Return a concise test report with:

- **Environment:** game entry point, browser, and starting/reset state.
- **Summary:** checks passed, failed, blocked, or not tested.
- **Findings:** for each problem, give severity (high/medium/low), reproduction steps, expected behavior, actual behavior, and relevant screen/error evidence.
- **Limitations:** name any checks skipped and why.

If no problems were observed, say so explicitly and list the checks completed. Distinguish confirmed failures from suspicions, and never silently treat an unrun check as a pass.
