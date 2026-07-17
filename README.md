
## Overview

The platform lets operators and administrators issue commands, review inspections, and manage users from either the web dashboard or the mobile app, both of which talk to the same backend in real time. Because either client can be used interchangeably, QA had to confirm that data stays accurate, synchronized, and secure across both — not just within each app individually.

Testing was carried out manually across three areas, each with its own test plan, strategy, and test cases derived from static review of the actual source code (not assumed specs):

- **Mobile Application** — Flutter (BLoC / Cubit)
- **Website** — React 19, Vite, React Router v7, Tailwind CSS v4, Recharts
- **Cross-System Integration** — shared FastAPI backend

**202 manual test cases** across **26 feature and integration areas**.

## Objectives

- **Functional validation** — positive, negative, and boundary flows for every feature
- **Cross-system data consistency** — actions on one client reflected correctly on the other, within polling intervals
- **Security** — token handling (session-based on web, bearer-token on mobile), sensitive data cleared on logout
- **Performance & compatibility** — load times, scroll smoothness, degraded network handling, cross-device/browser behavior
- **Usability & error handling** — clear feedback (loading, success, error states) and graceful degradation
- **Defect tracking** — standardized severity/priority classification with full traceability to test cases
- **Release readiness** — defined entry/exit criteria to support go/no-go decisions

## Methodology

A risk-based, source-code-driven approach: effort was allocated by feature criticality rather than spread evenly. Authentication, machine control, and quality inspection review received the deepest coverage since failures there could compromise safety or data integrity.

### Testing Layers

| Layer | Approach | Primary Focus |
|---|---|---|
| Manual Functional Testing | Black-box validation against implemented behavior | All feature areas, both clients |
| Integration & State Testing | State sync, screen-to-screen flow, cross-client consistency | BLoC/Context state, session data, shared endpoints |
| Security Testing | Auth token handling, logout data clearance | Authentication, session management |
| Compatibility & Performance | Cross-device/browser checks, load times | Android, iOS, Chrome, Firefox, Edge |

### Execution Phases

1. **Static code analysis** — manual review of source files to identify features, logic, and risks
2. **QA scope definition** — authoring test plan, strategy, and test cases
3. **Manual test execution** — on physical devices, emulators, simulators, and browsers
4. **Regression testing** — re-execution of linked test cases after each fix
5. **Sign-off** — review against exit criteria

### Entry / Exit Criteria

**Entry:** app installs/deploys successfully, reaches initial screen without errors, minimum test data and accounts available.

**Exit:** 100% of Critical/High test cases executed, ≥80% of Medium test cases executed, zero open Critical defects, zero release-blocking High defects, all resolved defects passed regression.

## Test Coverage

### Mobile App (Flutter) — 83 test cases across 11 feature areas

| Feature Area | Total | Critical | High | Medium | Low |
|---|---|---|---|---|---|
| Authentication | 14 | 4 | 6 | 3 | 1 |
| Home Dashboard | 11 | 4 | 5 | 2 | 0 |
| Session History | 6 | 0 | 3 | 3 | 0 |
| Machine Control | 8 | 3 | 4 | 1 | 0 |
| Quality Log | 15 | 2 | 6 | 5 | 2 |
| Settings | 11 | 2 | 5 | 3 | 1 |
| Navigation | 4 | 1 | 2 | 1 | 0 |
| Security | 4 | 3 | 1 | 0 | 0 |
| Performance | 3 | 0 | 2 | 1 | 0 |
| Compatibility | 4 | 0 | 2 | 1 | 1 |
| UX | 3 | 0 | 1 | 2 | 0 |
| **Total** | **83** | **19** | **37** | **22** | **5** |

### Website (React) — 77 test cases across 8 feature areas

| Feature Area | Total | Critical | High | Medium | Low |
|---|---|---|---|---|---|
| Authentication | 13 | 5 | 5 | 2 | 1 |
| Dashboard | 13 | 5 | 5 | 3 | 0 |
| Quality Log | 16 | 6 | 6 | 4 | 0 |
| Machine Control | 9 | 4 | 4 | 1 | 0 |
| Settings | 14 | 3 | 6 | 4 | 1 |
| Inspection Simulator | 6 | 0 | 3 | 3 | 0 |
| Navigation & Layout | 4 | 1 | 2 | 1 | 0 |
| Compatibility & UX | 2 | 0 | 1 | 2 | 0 |
| **Total** | **77** | **24** | **32** | **17** | **4** |

### Cross-System Integration — 42 test cases across 7 areas

The web client uses an `x-session-id` header while the mobile client uses a bearer token via `Authorization` — both must be accepted correctly by the shared backend. The website polls dashboard stats every 10s and telemetry every 2s, so timing-sensitive sync checks were a key focus.

| Integration Area | Total | Critical | High | Medium |
|---|---|---|---|---|
| Authentication Consistency | 6 | 3 | 2 | 1 |
| Machine Control Synchronization | 7 | 4 | 2 | 1 |
| Quality Inspection Synchronization | 8 | 4 | 3 | 1 |
| Dashboard Data Consistency | 6 | 2 | 3 | 1 |
| User Management Propagation | 7 | 2 | 3 | 2 |
| Session State Consistency | 5 | 2 | 2 | 1 |
| Simultaneous Operation | 3 | 1 | 1 | 1 |
| **Total** | **42** | **18** | **16** | **8** |

### Combined Summary — 202 test cases

| Priority | Mobile | Website | Integration | Total |
|---|---|---|---|---|
| Critical | 19 | 24 | 18 | 61 |
| High | 37 | 32 | 16 | 85 |
| Medium | 22 | 17 | 8 | 47 |
| Low | 5 | 4 | 0 | 9 |
| **Total** | **83** | **77** | **42** | **202** |

Critical and High priority cases make up 72% of the suite (146/202), reflecting the risk-based focus on safety- and data-critical features.

## Test Types Applied

- **Functional** — positive/negative flows, boundary conditions
- **Integration & State** — data flow between screens and across clients
- **UI & Theme** — layout integrity, loading/empty states, dark/light rendering
- **Navigation** — routing, back-stack, tab switching, protected routes
- **Validation** — form inputs, empty/whitespace fields, boundary values
- **Error Handling** — network loss, server errors, cold-start delays
- **Security** — token handling, logout clearance, no sensitive data leaks
- **Role-Based Access** — admin controls hidden from non-admin roles
- **Real-Time Polling** — correct refresh intervals, clean teardown
- **Performance** — load times, scroll smoothness
- **Compatibility** — Android/iOS versions, screen sizes, Chrome/Firefox/Edge

## Summary

This QA effort validated the mobile app, the website, and their integration independently and as a combined system, producing 202 manual test cases across 26 feature/integration areas. The result is a data-driven basis for assessing the platform's reliability and readiness for release.
