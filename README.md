# NYS Driving Manual — ErgoAI Knowledge Base

A formal logic knowledge base encoding the **New York State Driver's Manual (Part 2)** using [ErgoAI 3.0](https://coherentknowledge.com/ergoai-platform/). Built as an honors project for CSE 495 at Stony Brook University (April 2026).

## Overview

Autonomous vehicles must make thousands of legal decisions per minute. This project encodes NYS traffic law as a queryable logic knowledge base — providing **auditable, verifiable, exception-aware** legal reasoning that neural networks cannot match.

## Files

| File | Description |
|------|-------------|
| `NYSdrivingmanual.ergo` | Main knowledge base — all rules |
| `tests1.ergo` | Test suite — 150+ test cases, all passing |

## Coverage

The KB covers all major sections of NYS Driver's Manual Part 2:

- **Right-of-Way** (12 rules) — intersections, driveways, pedestrians, emergency vehicles
- **Traffic Signals** — red, yellow, green, flashing signals
- **Pavement Markings** — lane lines, crosswalks, no-passing zones
- **Signs** — stop, yield, speed limit, regulatory and warning signs
- **Passing Rules** — when passing is legal/illegal
- **School Buses** — stopping requirements, passing rules
- **Parking Regulations** — where parking is prohibited
- **Railroad Crossings** — stopping and yielding rules
- **Traffic Officers** — defeasible override of all signs and signals
- **Speed Limits** — statutory and posted limits
- **Funeral Processions** — right-of-way rules
- **U-Turns** — when U-turns are prohibited

## Running

Requires [ErgoAI 3.0](https://coherentknowledge.com/ergoai-platform/).

```prolog
# Load the knowledge base
?- [NYSdrivingmanual].

# Run the test suite
?- [tests1].
```

All 150+ tests print `=ok` on load. Any `=failed` indicates a regression.

## Key Design Features

**Negation as failure (`\naf`)** handles rules with exceptions cleanly:

```prolog
mayTurnRightOnRed(?Driver, ?Drive) :-
    rightTurn(?Driver, ?Drive),
    facingSteadyRed(?Drive),
    fullStop(?Driver, ?Drive),
    yieldedToOncoming(?Driver, ?Drive),
    \naf noTurnOnRedSign(?Drive),
    \naf turnProhibitedByMarking(?Drive),
    \naf inNYC(?Drive),
    \naf schoolBusWithStudents(?Driver).
```

**Defeasible reasoning** handles officer overrides — a traffic officer's direction defeats any sign or signal.

**Formal verification** — every rule has corresponding tests, making the KB provably correct against the law.

## Why Logic over ML?

| | Neural Networks | This KB |
|---|---|---|
| Explainability | Black box | Fully traceable |
| Exception handling | Brittle | Systematic (`\naf`) |
| Jurisdiction updates | Retrain model | Swap rule module |
| Formal verification | Not possible | 150+ passing tests |

## Course

CSE 495 — Honors Project, Stony Brook University, April 2026
