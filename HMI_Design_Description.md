# HMI Design - TIA Portal v18

## Screen Overview

HMI shows the operator:
- Current step in sequence
- Active outputs (motors, pumps, cylinders)


## HMI Elements (Tag Connections)

| Element | Type | Tag | Color Logic |
|---------|------|-----|-------------|
| System Status | Text field | System_Run | Green = Running, Red = Stopped |
| Grabber Status | Shape/Icon | Grabber_Confirmed | Green = Closed, Gray = Open |
| Fill Timer | Numeric | T1_TIME.ET | Shows remaining seconds |
| Cap Timer | Numeric | T2_TIME.ET | Shows remaining seconds |
| PS4 Indicator | Circle | PS4 (I1.0) | Green = 1, Gray = 0 |
| PSRight | Circle | PSRight (I0.6) | Green = 1, Gray = 0 |
| PSLeft | Circle | PSLeft (I0.7) | Green = 1, Gray = 0 |
| PS3 Indicator | Circle | PS3 (I0.5) | Green = 1, Gray = 0 |
| LS1 (Home) | Circle | LS1 (I0.3) | Green = 1, Gray = 0 |
| LS2 (End) | Circle | LS2 (I0.4) | Green = 1, Gray = 0 |
| Motor1 (Belt) | Icon | Motor1 (Q0.0) | Animated when = 1 |
| Pump1 | Icon | Pump1 (Q1.2) | Animated when = 1 |
| Start Button | Button | Start_PB (I0.0) | Momentary |
| Stop Button | Button | Stop_PB (I0.1) | Momentary |
| EM Stop Indicator | Banner | EM_PB (I0.2) | Red background when = 0 |
| Step Indicator | Text | Step_Counter | Shows current step number |


## HMI Best Practices Applied

- ✅ Color coding (Green = OK, Red = Fault, Yellow = Warning)
- ✅ Clear labeling of all I/O

## Future HMI Enhancements

- Production counter (bottles/hour)
- Fault history log with timestamps
- Recipe selection (different fill volumes)
- Operator login/security levels
- Trend graphs for cycle times
