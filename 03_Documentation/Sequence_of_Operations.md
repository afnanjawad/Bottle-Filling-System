# Sequence of Operations - Detailed

## System States

| State | Description | Outputs Active |
|-------|-------------|----------------|
| IDLE | Waiting for start | None |
| READY | Prerequisites met, waiting for bottle | None |
| GRABBING | Securing bottle | M2, M3, M4, M5 |
| FILLING | Adding liquid | Cylinder3 (down), Pump1 |
| TRANSFER_1 | Moving to capping | Motor1 (FWD) |
| CAPPING | Tightening cap | Cylinder2 (down), M6 |
| TRANSFER_2 | Moving to ejection | Motor1 (FWD) |
| EJECTING | Pushing bottle out | Cylinder1 |
| RETURNING | Returning to home | Motor1 (REV) |
| EMERGENCY | Fault state | All OFF |

## Step-by-Step Sequence

### Step 0: Power Up & Initialization
- Compressor ON (manual or automatic)
- All cylinders retracted (check LS3, LS6 for home)
- Belt returns to LS1 (home) if not already there
- System enters IDLE state

### Step 1: Start & Bottle Detection
- Operator presses `Start_PB (I0.0)`
- System waits for bottle at grabber station
- `PS4 (I1.0)` detects bottle → System moves to GRABBING

### Step 2: Grabber Closure
- `Motor2 (Q0.1)` through `Motor5 (Q0.4)` energize (close hands)
- Wait for `PSRight (I0.6)` AND `PSLeft (I0.7)` = HIGH
- **Timeout:** None currently (Future: 3-second timeout → fault)
- Once both sensors HIGH → Move to FILLING

### Step 3: Liquid Filling
- `Cylinder3 (Q1.1)` energizes → Nozzle lowers
- Wait for `LS7 (I1.7)` = HIGH (nozzle fully down)
- `Pump1 (Q1.2)` runs for 5 seconds (Timer T1)
- After 5 sec → Pump stops
- `Cylinder3 (Q1.1)` de-energizes → Nozzle raises
- Wait for `LS6 (I1.6)` = HIGH (nozzle home)
- Move to TRANSFER_1

### Step 4: Transfer to Capping Station
- `Motor1 (Q0.0)` energizes FORWARD
- Belt runs until `PS3 (I0.5)` = HIGH (bottle at capper)
- `Motor1` stops
- Move to CAPPING

### Step 5: Cap Dispensing
- **Note:** Cap is hanging, bottle moves under it to "grab" it
- `Motor7 (Q1.0)` energizes (dispenser motor)
- Wait for `PS1 (I1.4)` AND `PS2 (I1.5)` = HIGH (cap passed both sensors)
- `Motor7` stops
- **Future:** Timeout if cap not detected within 3 seconds
- Move to CAPPING (tightening)

### Step 6: Capping (Tightening)
- `Cylinder2 (Q0.6)` energizes → Capper lowers
- Wait for `LS5 (I1.3)` = HIGH (capper fully down)
- `Motor6 (Q0.7)` runs for 5 seconds (Timer T2)
- After 5 sec → Motor6 stops
- `Cylinder2 (Q0.6)` de-energizes → Capper raises
- Wait for `LS4 (I1.2)` = HIGH (capper home)
- Move to TRANSFER_2

### Step 7: Transfer to Ejection
- `Motor1 (Q0.0)` energizes FORWARD
- Belt runs until `LS2 (I0.4)` = HIGH (end position)
- `Motor1` stops
- Move to EJECTING

### Step 8: Ejection
- `Cylinder1 (Q0.5)` energizes → Pushes bottle out
- Wait 2 seconds (ensure bottle clears)
- `Cylinder1` de-energizes → Returns home
- Wait for `LS3 (I1.1)` = HIGH (cylinder home)
- Move to RETURNING

### Step 9: Return to Home
- `Motor1 (Q0.0)` energizes REVERSE
- Belt runs reverse until `LS1 (I0.3)` = HIGH (home position)
- `Motor1` stops
- Move to READY state (wait for next bottle)

### Step 10: Cycle Repeats
- System waits for next bottle at PS4
- Repeat from Step 1

## Emergency Stop Behavior

When `EM_PB (I0.2)` is pressed (signal goes LOW):
1. All outputs (Q0.0 - Q1.6) turned OFF immediately
2. All motors stop
3. All cylinders de-energize (return by spring or air dump)
4. Sequence halts
5. To resume: Release EM_PB → Press Start_PB → System re-initializes

## Timing Summary

| Operation | Duration | Timer Used |
|-----------|----------|------------|
| Pump run (fill) | 5 seconds | T1 |
| Capping motor run | 5 seconds | T2 |
| Ejection delay | 2 seconds | T3 (hardcoded) |
| **Total cycle** | ~30 seconds | - |
