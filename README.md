**Chapter 5**

**Quality Assurance and Software Testing**

## **5.1 Introduction**

The reliability of an Industrial IoT-based quality inspection platform depends not only on the correctness of its individual components, but also on the consistency of the system as a whole. The platform is composed of two independent client applications — a React-based web dashboard, and the Industrial Monitoring App, a Flutter-based mobile application — which operate simultaneously against a shared FastAPI backend. Because operators and administrators may issue commands, review inspections, and manage users from either client at any time, the system must guarantee that data remains accurate, synchronized, and secure across both interfaces at all times.

To achieve this level of reliability, a comprehensive manual Quality Assurance (QA) process was carried out covering three complementary areas: the mobile application, the web application, and the integration between them. Each area was documented independently through a dedicated test plan, test strategy, and set of manual test cases, all derived directly from a static review of the application source code rather than assumptions about intended behaviour. In total, the QA process produced several manual test cases distributed across 26 feature and integration areas, supported by structured defect-management procedures and formal entry and exit criteria for release readiness.

This chapter presents the QA methodology adopted for the platform, the scope and coverage of testing across the mobile application, the website, and their cross-system integration, and the complete set of manual test cases authored and executed for each area.

## **5.2 Objectives of the Quality Assurance Process**

The QA process was designed to ensure that every user-facing feature of the platform behaves correctly, securely, and consistently, both within each client application and across the two clients when operating against the shared backend. The main objectives of the testing effort are summarized as follows:

**1. Functional Validation**

- Verify that every implemented feature in the mobile application and the website behaves according to its intended functionality.

- Cover positive flows, negative flows, and boundary conditions for each feature.

**2. Cross-System Data Consistency**

- Confirm that actions performed on one client (e.g., machine control, quality inspection review, user management) are correctly reflected on the other client.

- Validate that both clients remain synchronized with the shared backend within their respective polling intervals.

**3. Security Verification**

- Validate authentication token handling on both clients, including the session-based mechanism used by the website and the bearer-token mechanism used by the mobile application.

- Confirm that sensitive data is cleared on logout and is never exposed in the UI or in application logs.

**4. Performance and Compatibility**

- Assess screen load times, scrolling smoothness, and behaviour under slow or absent network conditions.

- Verify consistent behaviour across Android and iOS devices, multiple screen sizes, and the Chrome, Firefox, and Edge browsers.

**5. Usability and Error Handling**

- Confirm that users receive clear feedback — loading indicators, success messages, and error banners — for every action.

- Validate graceful degradation of both clients under network loss and backend error conditions.

**6. Defect Identification and Documentation**

- Detect, classify, and document functional and integration defects using a standardized severity and priority framework.

- Maintain traceability between each defect and the test case that revealed it.

**7. Release Readiness Assessment**

- Establish clear entry and exit criteria for each testing phase.

- Provide a data-driven basis for deciding whether the platform is ready for release.

By pursuing these objectives, the QA process contributes directly to the overall dependability of the platform, ensuring that operators can trust the information displayed by either client and that critical actions, such as starting or stopping the production line, are executed safely and consistently.

## **5.3 Testing Strategy and Methodology**

The testing strategy adopted for the platform is built around a risk-based, source-code-driven methodology. Rather than relying on assumed behaviour, every test case was authored after a static review of the actual mobile (Dart/Flutter), web (React/JSX), and backend integration code, ensuring that the resulting test suite reflects the system as implemented rather than as originally specified.

### **5.3.1 Risk-Based Testing Approach**

Testing effort was allocated according to feature risk and criticality rather than distributed evenly across all functionality. Features whose failure could compromise production safety or data integrity — such as authentication, machine control, and quality inspection review — received the deepest coverage, while lower-risk cosmetic or informational features received proportionally fewer test cases. This approach maximizes defect-detection efficiency within the available manual testing effort.

### **5.3.2 Testing Layers**

The QA process is organized into four testing layers that apply consistently across the mobile application, the website, and the integration between them:

| **Layer**                             | **Approach**                                                                                        | **Primary Focus**                                            |
|---------------------------------------|-----------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| Manual Functional Testing             | Black-box validation of each feature against its implemented requirements.                          | All feature areas of both clients.                           |
| Integration and State Testing         | Validation of state synchronization, screen-to-screen data flow, and cross-client data consistency. | BLoC/Context state, session data, shared backend endpoints.  |
| Security Testing                      | Validation of authentication token handling and data clearance on logout.                           | Authentication, session management, sensitive data exposure. |
| Compatibility and Performance Testing | Verification across devices, browsers, screen sizes, and load-time checks.                          | Android, iOS, Chrome, Firefox, Edge, multiple resolutions.   |

**Table 5.1: Testing Layers Applied Across the Platform**

### **5.3.3 Testing Phases**

Each of the three QA documents — covering the mobile application, the website, and their integration — followed the same five-phase execution process:

| **Phase** | **Activity**                              | **Method**                                                            |
|-----------|-------------------------------------------|-----------------------------------------------------------------------|
| Phase 1   | Static code analysis and codebase review  | Manual review of source files to identify features, logic, and risks. |
| Phase 2   | QA scope definition and documentation     | Authoring of the test plan, strategy, and test cases.                 |
| Phase 3   | Manual test case execution                | Execution on physical devices, emulators, simulators, and browsers.   |
| Phase 4   | Regression testing on resolved defects    | Re-execution of linked test cases after each bug fix.                 |
| Phase 5   | Sign-off and release readiness assessment | Review of results against the defined exit criteria.                  |

**Table 5.2: QA Execution Phases**

### **5.3.4 Entry and Exit Criteria**

To ensure that testing begins and ends under well-defined conditions, entry and exit criteria were established for each of the three testing efforts. Entry criteria require that the application under test is successfully installed or deployed, reaches its initial screen without errors, and has the minimum test data and test accounts available. Exit criteria require that 100% of Critical- and High-priority test cases have been executed, at least 80% of Medium-priority test cases have been executed, zero Critical defects and zero release-blocking High defects remain open, and all resolved defects have passed regression testing before final sign-off.

## **5.4 Test Scope and Coverage**

The QA process covers three distinct but interrelated scopes: the Industrial Monitoring App (mobile), Nexus Factory OS (website), and the cross-system integration between them. Together, these efforts produced 202 manual test cases across 26 feature and integration areas.

### **5.4.1 Mobile Application Testing**

The mobile application testing scope covers the Flutter-based Industrial Monitoring App, which uses the BLoC pattern for the Home module and Cubit for authentication, with SharedPreferences used for local token and session persistence. Eleven feature areas were identified through source-code review, resulting in 83 test cases.

| **Feature Area** | **Total TCs** | **Critical** | **High** | **Medium** | **Low** |
|------------------|---------------|--------------|----------|------------|---------|
| Authentication   | 14            | 4            | 6        | 3          | 1       |
| Home Dashboard   | 11            | 4            | 5        | 2          | 0       |
| Session History  | 6             | 0            | 3        | 3          | 0       |
| Machine Control  | 8             | 3            | 4        | 1          | 0       |
| Quality Log      | 15            | 2            | 6        | 5          | 2       |
| Settings         | 11            | 2            | 5        | 3          | 1       |
| Navigation       | 4             | 1            | 2        | 1          | 0       |
| Security         | 4             | 3            | 1        | 0          | 0       |
| Performance      | 3             | 0            | 2        | 1          | 0       |
| Compatibility    | 4             | 0            | 2        | 1          | 1       |
| UX               | 3             | 0            | 1        | 2          | 0       |
| Total            | 83            | 19           | 37       | 22         | 5       |

**Table 5.3: Mobile Application — Test Coverage by Feature Area**

### **5.4.2 Website Testing**

The website testing scope covers Nexus Factory OS, built with React 19, Vite, React Router v7, Tailwind CSS v4, and Recharts, using the Context API for authentication, inspections, and theming, with session identifiers stored in localStorage. Eight feature areas were identified, resulting in 77 test cases. Testing for this client also addresses concerns specific to a browser-based interface, including dark and light theme rendering, real-time polling behaviour, and role-based visibility of administrative controls.

| **Feature Area**      | **Total TCs** | **Critical** | **High** | **Medium** | **Low** |
|-----------------------|---------------|--------------|----------|------------|---------|
| Authentication        | 13            | 5            | 5        | 2          | 1       |
| Dashboard             | 13            | 5            | 5        | 3          | 0       |
| Quality Log           | 16            | 6            | 6        | 4          | 0       |
| Machine Control       | 9             | 4            | 4        | 1          | 0       |
| Settings              | 14            | 3            | 6        | 4          | 1       |
| Inspection Simulator  | 6             | 0            | 3        | 3          | 0       |
| Navigation and Layout | 4             | 1            | 2        | 1          | 0       |
| Compatibility and UX  | 2             | 0            | 1        | 2          | 0       |
| Total                 | 77            | 24           | 32       | 17         | 4       |

**Table 5.4: Website — Test Coverage by Feature Area**

### **5.4.3 Cross-System Integration Testing**

Because both clients read from and write to the same FastAPI backend, an independent integration testing effort was carried out to validate that actions performed on one client are correctly and consistently reflected on the other. This is particularly significant given that the two clients use different authentication mechanisms — the website relies on a session identifier sent through an x-session-id header, while the mobile application relies on a bearer token sent through an Authorization header — both of which must be accepted correctly by the shared backend.

Integration test cases were designed around key risk areas such as simultaneous machine-control commands, quality-inspection synchronization, and user-management propagation, with particular attention paid to the polling intervals of each client (the website refreshes dashboard statistics every 10 seconds and telemetry every 2 seconds). Seven integration areas were identified, resulting in 42 cross-client test cases.

| **Integration Area**               | **Total TCs** | **Critical** | **High** | **Medium** |
|------------------------------------|---------------|--------------|----------|------------|
| Authentication Consistency         | 6             | 3            | 2        | 1          |
| Machine Control Synchronization    | 7             | 4            | 2        | 1          |
| Quality Inspection Synchronization | 8             | 4            | 3        | 1          |
| Dashboard Data Consistency         | 6             | 2            | 3        | 1          |
| User Management Propagation        | 7             | 2            | 3        | 2          |
| Session State Consistency          | 5             | 2            | 2        | 1          |
| Simultaneous Operation             | 3             | 1            | 1        | 1          |
| Total                              | 42            | 18           | 16       | 8          |

**Table 5.5: Cross-System Integration — Test Coverage by Area**

### **5.4.4 Overall Test Coverage Summary**

Combining the three testing efforts provides an overall view of the QA coverage achieved across the platform:

| **Priority** | **Mobile App** | **Website** | **Integration** | **Total** |
|--------------|----------------|-------------|-----------------|-----------|
| Critical     | 19             | 24          | 18              | 61        |
| High         | 37             | 32          | 16              | 85        |
| Medium       | 22             | 17          | 8               | 47        |
| Low          | 5              | 4           | 0               | 9         |
| Total        | 83             | 77          | 42              | 202       |

**Table 5.6: Combined Test Coverage Summary — 202 Test Cases**

The distribution shows that Critical- and High-priority test cases together account for 72% of the total suite (146 of 202 test cases), reflecting the risk-based approach applied throughout the QA process and its emphasis on the features most likely to affect safety, data integrity, or core operational workflows.

## **5.5 Test Types and Techniques**

A consistent set of test types was applied across the mobile application, the website, and the integration test suite, adapted where necessary to the characteristics of each client:

| **Test Type**                 | **Description**                                                                                                                          |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Functional Testing            | Validates that each feature works correctly, covering positive flows, negative flows, and boundary conditions.                           |
| Integration and State Testing | Validates data flow between screens, state layers, and — for cross-system tests — between the two client applications.                   |
| UI and Theme Testing          | Validates visual correctness, layout integrity, loading and empty states, and dark/light theme rendering on the website.                 |
| Navigation Testing            | Validates routing, back-stack behaviour, tab switching, and protected-route enforcement.                                                 |
| Validation Testing            | Validates form inputs, including empty fields, whitespace handling, mismatched values, and boundary values.                              |
| Error Handling Testing        | Validates graceful degradation under network loss, server errors, and cold-start delays.                                                 |
| Security Testing              | Validates token handling, session clearance on logout, and absence of sensitive data in the UI or logs.                                  |
| Role-Based Access Testing     | Validates that administrative controls are hidden from non-administrator roles.                                                          |
| Real-Time Polling Testing     | Validates that periodic data refreshes fire at the correct interval, update the UI accurately, and stop cleanly when a screen is closed. |
| Performance Testing           | Validates screen load times, scroll smoothness, and behaviour under extended loading conditions.                                         |
| Compatibility Testing         | Validates consistent behaviour across Android and iOS versions, screen sizes, and Chrome, Firefox, and Edge browsers.                    |

**Table 5.7: Test Types Applied Across the Platform**

## **5.6 Chapter Summary**

This chapter presented the Quality Assurance and testing process carried out for the platform, covering the Industrial Monitoring mobile application, the Nexus Factory OS website, and the integration between them. It described the risk-based testing methodology and execution phases applied consistently across all three areas, the scope and coverage achieved through 202 manual test cases spanning 26 feature and integration areas, and the complete set of test cases authored for each application. By validating each client independently and their consistency as an integrated system, the QA process provides a data-driven basis for assessing the reliability and release readiness of the platform.
