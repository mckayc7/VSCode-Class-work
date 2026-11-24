# Prompt Log

## 2026-11-23
- **Asked:** Add a distinct vehicle animation for each of the four test vehicles.
- **Changed:** Added separate SVG graphics for the car, loaded dump truck, semi truck, and fire engine while preserving the selected vehicle's axle-positioned drive animation.
- **Worked:** Yes; browser verification rendered a different SVG shape set for all four vehicle selections.

## 2026-11-23
- **Asked:** Add a celebration screen when all four vehicles complete the bridge.
- **Changed:** Added a confetti overlay with a challenge-complete card and close button. It appears only after all four vehicle scoreboard entries are passed.
- **Worked:** Yes; browser verification showed the celebration card with 90 confetti pieces and confirmed the Keep designing button closed it.

## 2026-11-23
- **Asked:** Default to the Select / Inspect tool after running a vehicle in Test mode.
- **Changed:** Activate Select / Inspect when entering Test mode, starting or resetting a vehicle run, and returning to Build mode.
- **Worked:** Yes; browser verification confirmed Select / Inspect was active during a run and after returning to Build mode.

## 2026-11-23
- **Asked:** Make tension member free-body diagram arrows point outward.
- **Changed:** Updated member free-body arrow directions to point outward when axial force is tensile and inward when compressive.
- **Worked:** Yes; browser verification found a 833 lb tension member and confirmed the left arrow ran outward from x=48 toward x=27.

## 2026-11-20
- **Asked:** Create a prototype of Test mode.
- **Changed:** Added vehicle selection, drive/pause/reset controls, animated vehicle travel, method-of-joints member-force solving, green/yellow/red/black member utilization colors, failure stopping, and a four-vehicle scoreboard.
- **Worked:** Yes; browser verification opened Test mode, ran the semi, stopped at a failed top member, and updated its scoreboard status to Failed.

## 2026-11-20
- **Asked:** Allow the delete build tool to delete joints as well as members.
- **Changed:** Renamed the delete tool to Delete elements and enabled deletion of user joints. Fixed deck joints and required supports remain protected. Deleting a user joint removes its connected members.
- **Worked:** Yes; browser verification deleted a user joint and three connected members, while fixed support joints remained protected.

## 2026-11-20
- **Asked:** Include the member forces and a small free-body diagram in the member details card.
- **Changed:** Added axial-force and force-state rows plus a compact free-body diagram with equal-and-opposite force arrows. Build mode currently reports 0 lb because no vehicle load is applied.
- **Worked:** Yes; browser verification showed the force rows and free-body diagram.

## 2026-11-20
- **Asked:** Have Select / Inspect show a details card for selected joints and members.
- **Changed:** Added a live details card showing joint location, joint type, and connected member IDs, or member ID, end joints, length, and cost.
- **Worked:** Yes; browser verification showed joint details and member details for selected starter-truss elements.

## 2026-11-16
- **Asked:** Make the default app view show a Warren truss with a standard bridge across the bottom and required supports.
- **Changed:** Added a starter Warren truss with fixed bottom deck joints/supports, bottom chord, top chord, and alternating diagonals. Reset now restores the same starter design.
- **Worked:** Yes; the default design was verified in the browser.

## 2026-11-16
- **Asked:** Show the X and Y joint-location inputs only after clicking an Add joint button under Build tools.
- **Changed:** Renamed the build tool to Add joint and hid the coordinate form by default. Selecting Add joint reveals the form; selecting another tool hides it again.
- **Worked:** Yes; browser verification confirmed the form is hidden initially and appears when Add joint is selected.

## 2026-11-16
- **Asked:** Change joint placement so the user enters the desired location instead of clicking the bridge.
- **Changed:** Replaced click-to-add behavior with X and Y coordinate inputs in feet and an Add joint button. Whole-foot grid, range, duplicate, undo, reset, and status validation remain active.
- **Worked:** Yes; browser interaction verified the coordinate-entry workflow.

## 2026-11-16
- **Asked:** Build and show a prototype of Build mode.
- **Changed:** Created a self-contained `index.html` prototype with Build/Test mode tabs, fixed deck joints and supports, a 1 ft grid, add-joint snapping, member connection and deletion, undo/reset, status hints, and live joint/member/cost totals.
- **Worked:** Yes; static checks passed. Node.js was unavailable for runtime parsing.

## 2026-11-16
- **Asked:** Add structural-analysis formulas to the calculations section of the bridge specification.
- **Changed:** Added geometry, axle-load interpolation, support reactions, global method-of-joints equations, stability checks, capacity/utilization, cost, and worked semi-at-x=44-ft validation formulas and values.
- **Worked:** Yes; specification validation content is now explicit.

## 2026-11-16
- **Asked:** Identify the inputs needed to accomplish the bridge app design purpose.
- **Changed:** Clarified the Build-mode and Test-mode controls, fixed game-rule values, and calculated outputs. Build mode includes adding joints, connecting and deleting members, undo, and reset; Test mode uses a vehicle selector and drive button.
- **Worked:** Yes; the input contract is now explicit.
