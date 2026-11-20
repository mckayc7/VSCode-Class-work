# Will It Break? Bridge Strength Analysis

## Purpose
- A bridge design game. Design a truss bridge across a 60 ft gap, then drive vehicles across it, from a small car up to a fire engine, and watch the members light up as they approach their capacity. The goal is the cheapest bridge that carries every vehicle.

## Inputs
- Build mode:
  - Add joint: click a location on the 1 ft grid. Reject locations outside
    the 60 ft site or on an existing joint.
  - Connect joints: select two joints to add a member. Reject self-connections
    and duplicate members.
  - Delete member: select a member and remove it.
  - Undo: reverse the most recent valid edit.
  - Reset: clear the user-added members and restore the required deck joints
    and supports.
- Test mode:
  - Vehicle selector (dropdown): Car, Loaded dump truck, Semi (AASHTO HS20
    design truck), or Fire engine.
  - Drive button: advance the selected vehicle across the bridge until it
    finishes or a member exceeds capacity.
- Vehicle definitions are fixed game rules. Axle loads are in lb, measured
  back from the front axle. Each axle load is split between the two deck
  joints on either side of it, with the nearer joint taking more:
  - Car: 2,000 at 0 ft; 2,000 at 10 ft
  - Loaded dump truck: 12,000 at 0 ft; 17,000 at 14 ft; 17,000 at 18.5 ft
  - Semi: 8,000 at 0 ft; 32,000 at 14 ft; 32,000 at 28 ft
  - Fire engine: 20,000 at 0 ft; 27,000 at 17 ft; 27,000 at 21.5 ft
- The 60 ft span, 10 ft deck-joint spacing, support locations, vehicle
  definitions, member capacities, and cost values are fixed and are not
  user-editable settings.

## Calculations
### Geometry and member directions

- Use feet and pounds internally. For member \(i\), with joints \(a\) and
  \(b\), calculate
  `dx = x_b - x_a`, `dy = y_b - y_a`,
  `L_i = sqrt(dx^2 + dy^2)`, `c_i = dx / L_i`, and
  `s_i = dy / L_i`.
- A member force is positive in tension. At joint \(a\), its force components
  are `(c_i F_i, s_i F_i)`; at joint \(b\), they are the negatives.

### Vehicle loads at deck joints

- For an axle load \(P\) at position \(x_a\), find adjacent deck joints
  \(x_L \leq x_a \leq x_R\). Apply downward joint loads
  `P_L = P (x_R - x_a) / (x_R - x_L)` and
  `P_R = P (x_a - x_L) / (x_R - x_L)`.
- If an axle is exactly on a deck joint, apply the full load to that joint.
  Axles outside the 0-60 ft span apply no load until they enter the span.
- The total downward load is `W = sum(P)`. Use positive upward reactions and
  negative applied loads in equilibrium.

### Support reactions

- The pin at x = 0 ft supplies horizontal reaction \(A_x\) and vertical
  reaction \(A_y\). The roller at x = 60 ft supplies vertical reaction
  \(B_y\).
- For vertical vehicle loads, `A_x = 0`.
- From global equilibrium:
  `A_y + B_y = W`
  and `B_y * 60 = sum(P_k x_k)`.
  Therefore `B_y = sum(P_k x_k) / 60` and `A_y = W - B_y`.

### Method of joints as one linear system

- Let the unknown vector contain every member force followed by `A_x`, `A_y`,
  and `B_y`. For every joint, write:
  `sum(Fx) = 0` and `sum(Fy) = 0`.
- For a member connected to a joint, place its direction cosine in the
  corresponding x-equilibrium coefficient and its direction sine in the
  corresponding y-equilibrium coefficient. Place support-reaction
  coefficients at the support joints, and place applied vehicle loads on the
  right-hand side.
- Solve `K u = f` with a numerically stable linear solver. Member forces are
  reported in pounds; positive values are tension and negative values are
  compression.
- A truss is solvable only when `number of members + 3 = 2 * number of
  joints`, the support conditions are present, and the matrix is not nearly
  singular. Otherwise show an instability message instead of force values.

### Capacity, utilization, and cost

- For member force \(F_i\), use allowable tension \(T=90,000\) lb and
  allowable compression \(C=75,000\) lb:
  `allowable_i = T if F_i >= 0 else C`.
- `utilization_i = abs(F_i) / allowable_i`. A member is green below 0.70,
  yellow from 0.70 through 1.00, red above 1.00, and black when it fails
  during a vehicle test.
- `memberCost = 100 * sum(L_i)` dollars and
  `jointCost = 500 * numberOfUserJoints` dollars. The displayed bridge cost
  is their sum.

### Worked validation: semi front axle at x = 44 ft

- The semi axle positions are 44 ft, 58 ft, and 72 ft. The first two axles
  are on the bridge, so the active loads are 8,000 lb at 44 ft and 32,000 lb
  at 58 ft. The 32,000 lb axle at 72 ft is outside the span.
- Total load:
  `W = 8,000 + 32,000 = 40,000 lb`.
- Taking moments about the pin:
  `B_y = (8,000*44 + 32,000*58) / 60 = 36,800 lb`.
  Therefore `A_y = 40,000 - 36,800 = 3,200 lb`, and `A_x = 0`.
- For a cut through the 20-30 ft panel, use the left free body. There are no
  vehicle loads to the left of the cut. At a section location \(x\) in that
  panel, `V = A_y = 3,200 lb` and `M(x) = A_y*x`; at the panel midpoint
  x = 25 ft, `M = 80,000 lb-ft`.
- For the 10 ft-high Pratt truss, the chord-force magnitude at that section
  is `M / h = 80,000 / 10 = 8,000 lb`: top chord compression and bottom chord
  tension.
- The panel diagonal has `sin(theta) = 10 / sqrt(10^2 + 10^2) = 0.7071`.
  Its force magnitude is `V / sin(theta) = 3,200 / 0.7071 = 4,525 lb`.
  For the left-half diagonal sloping down toward the center, the left-cut
  convention reports compression.
- Compare these reaction and section-force values with the app's joint and
  member tables. Then test all four vehicles: the validation bridge should
  carry the car and loaded dump truck and fail under the semi.

## Outputs
- A bridge site with deck joints every 10 ft from x = 0 to x = 60, a pin at x = 0, and a roller at x = 60, which can't be moved or deleted
- A truss solver using the method of joints as one linear system, with a message instead of numbers when the truss is unstable (it needs members + 3 = 2 × joints, and a system that isn't nearly singular)
- Member capacities of 90,000 lb in tension and 75,000 lb in compression.
- In Test mode, members are colored by how close they are to capacity (green, yellow, red), and a member that goes over turns black and stops the vehicle.
- A cost of $100 per foot of member and $500 per joint, and a scoreboard of which vehicles the current design has carried
- A details panel, closed by default, with the joint and member force tables you need for validation


## Look and feel
- Two modes. In Build mode, the user places joints on a 1 ft grid and connects them with members, and a hint on screen says what the next click will do. In Test mode, the drawing tools are hidden and the user picks a vehicle and drives it across.
- bridge visual with car driving across is in it's own panel
- inputs on their own panel
- Clean and professional, like a commercial engineering web app: light
  background, white cards with soft shadows, accent colors (blue, orange, pink and yellow), a modern
  system font, generous spacing. No clutter.
- Every input has a short label with units. Hovering over a check card
  explains the formula used.
- On a phone, the panels stack.

## Validation
- Build a 60 ft Pratt truss, 10 ft tall, with six 10 ft panels and diagonals sloping down toward the center. Park the semi with its front axle at x = 44 ft, compute the reactions by hand, and use the method of sections to compute the forces in the top chord, bottom chord, and diagonal in the panel from x = 20 to 30. Compare them to the app. Then drive all four vehicles across: this truss should carry the car and the dump truck and fail under the semi.

## Out of scope
