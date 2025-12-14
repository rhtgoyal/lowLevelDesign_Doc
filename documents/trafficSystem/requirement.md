# Traffic Light System – Core Requirements (4‑Road Intersection)

## 1. System Assumptions

- The intersection consists of **4 roads**: North (N), South (S), East (E), and West (W).
- N–S operate as one axis; E–W operate as the perpendicular axis.
- Each approach has standard **Red / Amber / Green** vehicle signals.
- Pedestrian crossings exist on all four sides.
- No adaptive AI logic is assumed unless added later.

---

## 2. Functional Requirements

### 2.1 Signal Phase Control

The system shall manage safe and predictable signal transitions for all four approaches.

**Details:**
- N–S operate together; E–W operate together.
- Safe transition sequence: Green → Amber → All‑Red → Next Green.
- Timing parameters are configurable (minimum green, amber duration, all‑red clearance).
- Conflicting greens must never occur.

---

### 2.2 Pedestrian Crossing Management

The system shall ensure safe pedestrian movement across all four crosswalks.

**Details:**
- Provide Walk / Don’t Walk indications.
- Integrate pedestrian button requests into the next safe cycle.
- Guarantee minimum crossing time based on distance.
- Prevent vehicle phases that conflict with active pedestrian phases.
- Support optional audible cues for accessibility.

---

## 3. Non‑Functional Requirements

### 3.1 High Availability & Reliability

- Target uptime: 99.9% annually.
- Automatic fail‑safe mode (e.g., flashing yellow/red) during critical faults.
- Watchdog timers and brownout protection.
- MTBF aligned with industry standards for roadside controllers.

---

### 3.2 Safety & Regulatory Compliance

- Enforce non‑conflicting greens and mandatory clearance intervals.
- Follow national/state traffic signal standards.
- Support accessibility norms (audible pedestrian cues where required).
- Maintain auditability for safety inspections.

---

### 3.3 Security & Access Control

- Role‑based access control for configuration changes.
- Encrypted communication for remote monitoring.
- Authentication required for all configuration changes.
- Logging of all administrative actions with timestamps.

---

## 4. Light Switching Algorithm (Deterministic Cycle)

### 4.1 Phase Definitions

| Phase | Vehicle Movement Allowed | Pedestrian Allowed |
|-------|---------------------------|---------------------|
| P1 | North–South Green | East–West Pedestrians |
| P2 | North–South Amber | None |
| P3 | All‑Red | None |
| P4 | East–West Green | North–South Pedestrians |
| P5 | East–West Amber | None |
| P6 | All‑Red | None |

---

### 4.2 Pseudocode Algorithm

```text
loop forever:

  # Phase 1: N-S Green
  set(NS = GREEN, EW = RED)
  set(Ped_EW = WALK, Ped_NS = DONT_WALK)
  wait(min_green_NS)

  # Phase 2: N-S Amber
  set(NS = AMBER, EW = RED)
  set(Ped_EW = DONT_WALK)
  wait(amber_time)

  # Phase 3: All-Red Clearance
  set(NS = RED, EW = RED)
  wait(all_red_time)

  # Phase 4: E-W Green
  set(EW = GREEN, NS = RED)
  set(Ped_NS = WALK, Ped_EW = DONT_WALK)
  wait(min_green_EW)

  # Phase 5: E-W Amber
  set(EW = AMBER, NS = RED)
  set(Ped_NS = DONT_WALK)
  wait(amber_time)

  # Phase 6: All-Red Clearance
  set(NS = RED, EW = RED)
  wait(all_red_time)

end loop

