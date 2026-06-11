# Fault Handling Matrix

## Current Implementation (Phase 1)

| Fault Condition | Detection Method | Current Response |
|----------------|------------------|------------------|
| Emergency stop | EM_PB (I0.2) = 0 | All outputs OFF immediately |
| Stop button | Stop_PB (I0.1) = 0 | Sequence pauses, resets on Start |
| Power loss | PLC power monitor | All outputs OFF, requires manual restart |

## Future Implementation (Phase 2 - Planned)

| Fault Condition | Detection | Response | Priority |
|----------------|-----------|----------|----------|
| Grabber not closed | PSRight/PSLeft not HIGH after 3 sec | Stop sequence, HMI alarm "Grabber Jam" | High |
| Cap not dispensed | PS1/PS2 not HIGH after 3 sec of M7 running | Retry once, then fault "Cap Feeder Jam" | High |
| Pump dry run | Add flow sensor (future hardware) | Stop pump, alarm "No Flow" | Medium |
| Belt overtravel | LS2 without LS1 sequence | Emergency stop, alarm "Belt Position Error" | Medium |
| Cylinder timeout | LS not reached within 5 sec of solenoid ON | Alarm, cycle stop | Medium |
| Production counter | Count cycles, compare to target | Stop when target reached, notify operator | Low |

## Fault Latching Logic (Planned)


// Pseudo-code for future fault handling
     Fault_Condition    System_Running    Fault_Latched
-------| |-----------------|/|-----------------(SET)----

     Fault_Latched    All_Outputs_Enable
-------| |------------------(RESET)---- (Outputs forced OFF)

     Acknowledge_PB    Reset_Timer    Fault_Latched
-------| |----------------(TON)-----------(RESET)---- (3 sec hold)
