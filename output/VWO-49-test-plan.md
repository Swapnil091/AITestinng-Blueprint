# Test Plan — VWO-49: [VWO] Add Passkey and SSO Login Options to VWO Login Page

![Company logo](../misc/company.png)

**Company context:** The supplied company profile describes VWO as a Wingify product focused on website A/B testing.  
**Status:** **DRAFT — pending human review.** This plan is not approved until a QA owner signs off.

| Ticket detail | Value |
|---|---|
| Issue | VWO-49 |
| Issue status | To Do |
| Priority | Medium in issue metadata; “High” in the description (conflict to resolve) |
| Epic | Authentication Enhancements |
| Components / fix version / sprint | Not specified |
| Out of scope | Passkey registration; SSO provider configuration |

## 1. Scope & Objectives

- **In scope:** Login-page visibility and placement of SSO and Passkey options; successful and unsuccessful SSO authentication; successful and unsuccessful Passkey authentication; no-registered-passkey and unsupported-environment handling; configured identity-provider compatibility; security, accessibility, responsive behavior, and relevant login regression checks.
- **Out of scope:** Passkey registration and SSO identity-provider configuration. Test setup may use existing configured providers and previously registered passkeys, but changing those configurations is not covered.
- **Objective:** Verify that eligible users can choose either new login method, reach the dashboard on successful authentication, receive an agreed safe and usable recovery/error path on failure, and do not expose sensitive authentication data. Confirm behavior across the product’s supported browser/device and configured identity-provider matrix.
- **Requirement sources:** VWO-49 acceptance criteria and listed test scenarios. No separate design, provider matrix, or detailed security specification was included in the ticket export.

### Requirement checklist review

| Checklist area | Status | Finding |
|---|---|---|
| User goal / story | ✅ Present | User wants secure and convenient login using Passkey or SSO. |
| Testable acceptance criteria | ⚠️ Ambiguous | Successful outcomes are observable; exact messages, fallback behavior, and some protocol/security expectations are not. |
| Happy paths | ✅ Present | SSO success redirects to dashboard; verified Passkey logs in and redirects. |
| Negative / error paths | ⚠️ Ambiguous | Error categories are named, but expected content, recovery, retry, and cancellation behavior are not defined. |
| Boundaries / empty states | ❌ Missing | No details for missing SSO identifiers, cancelled prompts, timeout, repeated failures, or other boundary conditions. |
| State transitions | ⚠️ Ambiguous | Login-to-provider/authenticator-to-dashboard is implied; intermediate states and return/cancel paths are unspecified. |
| Test data / preconditions | ❌ Missing | No test accounts, tenant setup, registered passkey profile, or invalid credential data identified. |
| Environment / configuration | ❌ Missing | No staging URL, feature flag, environment configuration, or supported browser/device list specified. |
| Dependencies / integrations | ⚠️ Ambiguous | OAuth/SAML and configured identity providers are mentioned, but provider names, versions, and test ownership are absent. |
| Performance | ❌ Missing | No response-time or availability expectation for either authentication flow. |
| Security / authorization | ⚠️ Ambiguous | Secure protocols and no sensitive data exposure are required, but protocol details, role coverage, and logging scope are unspecified. |
| Accessibility | ⚠️ Ambiguous | Accessibility is listed as a test scenario but no conformance target or expected keyboard/screen-reader behavior is defined. |
| Internationalization | ❌ Missing | No supported locales or translated-message expectations specified. |
| Audit / observability | ⚠️ Ambiguous | Network-log exposure is prohibited; required audit events and safe diagnostic logging are not defined. |
| Regression impact | ⚠️ Ambiguous | Existing login methods are present in context, but no explicit regression surface or expected behavior is listed. |
| Backward compatibility / migration | ❌ Missing | No expected behavior for existing accounts, tenants, or authentication configuration changes. |
| Browser / responsive matrix | ⚠️ Ambiguous | Passkeys are limited to supported browsers/devices and responsive behavior is mentioned, but the support matrix and breakpoints are absent. |
| Rollback / feature flag | ❌ Missing | No flag or rollback behavior specified. |
| Terminology / clarity | ⚠️ Ambiguous | “Valid SSO credentials” may not describe all federated SSO flows; “fallback or guidance” leaves the expected Passkey outcome open. |
| Design / mockups | ❌ Missing | No design link or approved copy for the login controls and error states is provided. |

## 2. Gaps & Questions for the author

| # | Area | Finding | Question to author |
|---|---|---|---|
| 1 | Priority | ⚠️ Issue metadata says Medium; description says High. | Which priority should drive release and test triage? |
| 2 | SSO flow | ⚠️ “Valid/invalid SSO credentials” is underspecified for federated identity. | Which identity providers and protocols are in scope, and what are the expected success, rejection, account-linking, cancellation, timeout, and recovery outcomes for each? |
| 3 | Passkey failures | ⚠️ “Fallback or guidance” and “authentication failure” do not define exact behavior. | What should users see/do for no registered passkey, user cancellation, timeout, authenticator rejection, and invalid/expired challenge? Is password login the intended fallback? |
| 4 | Error copy | ⚠️ Expected message and page state are not defined in the AC. | What approved message, retry behavior, and destination should each failure use? The ticket comment’s “Invalid credentials” is an execution expectation, not an explicit AC. |
| 5 | Test setup | ❌ Test identities and prerequisites are absent. | Can QA get accounts for successful/failed SSO per provider, accounts with/without registered passkeys, and access to provider test tenants? |
| 6 | Compatibility | ⚠️ No browser, OS, device, or provider support matrix. | Which exact desktop/mobile browsers, operating systems, authenticators, and identity-provider configurations must pass? |
| 7 | Security | ⚠️ Protocol names and no-exposure rule are broad. | Which OAuth/SAML/WebAuthn versions/configurations are expected, which roles/tenants must be covered, and what specific UI, browser storage, URL, and log fields are considered sensitive? |
| 8 | Quality attributes | ❌ Performance and localization targets absent; accessibility target unclear. | What performance thresholds, supported locales, and accessibility standard/level should be used? |
| 9 | Existing login / rollout | ⚠️ Regression and rollback expectations are not stated. | Which existing login methods and account states must be regression-tested, and is there a feature flag or rollback procedure? |
| 10 | Design / ownership | ❌ No mockup, component owner, fix version, or sprint supplied. | Is there an approved design/copy source, and what release target and QA/dependency owners should be recorded? |
| 11 | Current test readiness | ⚠️ Ticket comments record four automated runs, each inconclusive with 0 passed, 0 failed, and 4 blocked; “Actual” is blank. | What specifically blocked execution, and has the blocker been resolved? Please provide the latest reproducible environment/result before sign-off. |

## 3. Test Scenarios

Priorities are proposed test-plan priorities, not a resolution of the ticket’s conflicting priority values. **P0** covers core authentication/security paths; **P1** covers important negative, compatibility, and usability coverage; **P2** covers additional robustness once requirements and environment are available.

| ID | Priority | Type | Scenario | Maps to (AC / gap) |
|---|---|---|---|---|
| TS-01 | P0 | Positive | Open the login page and verify both exact button labels are visible below “Sign in with Google” in the specified order. | AC: UI Visibility |
| TS-02 | P0 | Positive | Select SSO, complete authentication with a valid test identity at each configured provider, and verify an authenticated session and dashboard redirect. | AC: SSO Login Flow, Compatibility; Gap 2 |
| TS-03 | P0 | Positive | Select Passkey on a supported browser/device, verify with a previously registered passkey, and confirm an authenticated session and dashboard redirect. | AC: Passkey Login Flow, Compatibility |
| TS-04 | P0 | Security | Inspect the authentication flow’s browser-visible UI, URL, storage, and network/log outputs to confirm secrets, tokens, and sensitive credentials are not exposed; verify secure protocol use against the approved configuration. | AC: Security; Gap 7 |
| TS-05 | P1 | Negative | Attempt SSO authentication with a provider-rejected test identity and verify the approved error, safe recovery/retry behavior, and no authenticated session. | AC: SSO Login Flow, Error Handling; Gaps 2–3 |
| TS-06 | P1 | Negative | Start Passkey login when the account has no registered passkey; verify the approved fallback/guidance and that no session is created. | AC: Passkey Login Flow; Gap 3 |
| TS-07 | P1 | Negative | On a supported environment, reject/cancel the Passkey prompt or let it time out; verify the agreed message, retry/fallback behavior, and unauthenticated state. | AC: Error Handling; Gap 3 |
| TS-08 | P1 | Negative | Cause a Passkey verification failure using an approved negative test fixture; verify safe error handling, no login, and no sensitive detail in UI/logs. | AC: Error Handling, Security; Gaps 3, 7 |
| TS-09 | P1 | Compatibility | Attempt Passkey login on a browser/device confirmed as unsupported; verify the specified unsupported-environment message or guidance and continued access to other login options. | AC: Error Handling, Compatibility; Gap 6 |
| TS-10 | P1 | Compatibility | Execute the SSO success and rejection paths against every provider in the approved configuration matrix. | AC: Compatibility; Gap 2 |
| TS-11 | P1 | Regression | Verify existing login methods and their established success/failure behavior remain available and unchanged alongside the new options. | AC: UI Visibility; Gap 9 |
| TS-12 | P1 | Accessibility | Use keyboard-only navigation and a screen reader to reach, identify, and activate both controls; verify focus order/visibility and error announcement against the agreed accessibility target. | Ticket test scenario; Gap 8 |
| TS-13 | P1 | Boundary | Resize supported viewport sizes and test mobile/responsive layouts; verify controls remain visible, correctly ordered, operable, and do not obscure existing login methods. | Ticket test scenario; Gap 6 |
| TS-14 | P2 | Negative | Verify recovery from provider unavailable, malformed/expired SSO response, or interrupted callback when such cases are supported by the test environment; ensure no partial session is created. | AC: Error Handling / secure flow; Gaps 2, 3 |
| TS-15 | P2 | Boundary | Exercise repeated attempts and refresh/back navigation during an in-progress SSO or Passkey attempt; verify no stale or duplicate authenticated state and the agreed retry behavior. | AC: Error Handling; Gaps 3, 5 |
| TS-16 | P2 | Non-functional | Measure SSO and Passkey completion/feedback time against an agreed performance target and verify safe, useful diagnostics are available without recording secrets. | Gaps 7–8 |
| TS-17 | P2 | Localization | If multiple locales are supported, verify button labels, errors, and guidance are translated, fit the UI, and retain correct meaning. | Gap 8 |

## 4. Test Data & Environment

- **Data:** To be supplied by the ticket owner/identity team: a successful SSO test identity and provider tenant per configured IdP; a provider-rejected test identity; accounts with and without a registered Passkey; and safe negative fixtures for cancellation, timeout, and verification failure. Avoid using production credentials or real user secrets.
- **Environment / flags:** Use the approved non-production staging environment with the feature enabled and working callback/WebAuthn origins. Exact URL, flag name/value, and deployment build are TBD. The ticket comments refer to `app.vwo.com` and `app.wingify.com/#/login`, with all listed automation cases blocked; they do not establish an approved QA environment.
- **Roles / permissions:** Account/tenant roles and expected authorization behavior are not specified; confirm whether role-specific SSO policies or tenant boundaries need coverage before sign-off.
- **Platform matrix:** Pending an owner-approved list of browsers, versions, operating systems, devices, and authenticators. Include at least the agreed supported and unsupported Passkey environments.
- **Dependencies:** Configured OAuth/SAML identity providers, reachable callback endpoints, WebAuthn-capable browser/device for positive Passkey coverage, existing passkey enrollment (registration itself is out of scope), and test accounts.

## 5. Risks & Assumptions

- **Assumptions made:**
  - The ticket acceptance criteria are the requirements baseline; unspecified behavior is recorded as a question rather than treated as a requirement.
  - “Dashboard” means the authenticated user’s expected landing page; its exact route and post-login destination rules require confirmation.
  - Passkey tests use accounts enrolled through the separate registration story; registration is not tested here.
  - Test scenarios use approved non-production identities and providers.
- **Risks:**
  - The issue metadata and description conflict on priority.
  - Four recorded automation executions (Oct 10, 13, 17, and 20, 2026) are inconclusive: 0 passed, 0 failed, 4 blocked each time, with blank actual results. The cause is unknown, and the ticket status is To Do; readiness and implementation behavior are therefore unverified.
  - Undefined provider coverage, error copy, supported platforms, and test identities can block reproducible execution or produce inconsistent acceptance decisions.
  - Broad security wording may leave important token/credential exposure, callback, and logging expectations unverified unless security owners define measurable checks.
- **Company context:** The supplied profile describes VWO as a Wingify product focused on website A/B testing. No additional company or product claims are assumed.

## 6. Entry / Exit criteria

- **Entry:**
  - Build is deployed to an agreed non-production environment, and VWO-49 is available for QA.
  - Login page, SSO providers/callbacks, and Passkey origin are configured and reachable.
  - Valid/invalid SSO test identities and registered/unregistered Passkey accounts are available.
  - Supported browser/device matrix, expected messages/recovery behavior, and priority conflict are clarified.
  - The blocker from the ticket’s prior inconclusive runs is understood or demonstrably resolved.
- **Exit:**
  - All P0 and agreed P1 scenarios are executed with recorded actual results and evidence.
  - No open Critical/High defects remain; lower-severity issues have documented disposition and owner approval.
  - Security checks show no sensitive authentication data exposure and use the approved protocol configuration.
  - Supported and unsupported environment behavior, regression, and accessibility checks meet the agreed criteria.
  - Any blocked or deferred scenarios have explicit rationale, owner, and risk acceptance.
  - QA owner approves the plan and execution results.

## 7. Resource Allocation

| Role | Allocation / responsibility |
|---|---|
| QA owner | TBD — approve scope, priority, open questions, and exit decision |
| QA tester | TBD — execute functional, compatibility, regression, and accessibility scenarios |
| Identity-provider / authentication engineer | TBD — provide test tenant/accounts and troubleshoot provider/callback dependencies |
| Security reviewer | TBD — confirm protocol, token-handling, and logging expectations |
| Product/design owner | TBD — confirm copy, behavior, supported matrix, and priority |

## 8. Timelines, Sprint Details

- **Sprint / fix version:** Not specified in the ticket export.
- **Execution dates / duration:** TBD after environment readiness, test data, and open-question resolution.
- **Milestones:** Resolve requirements and prior-run blocker → prepare test environment/data → execute P0/P1 → execute agreed P2 → QA review and sign-off.

---
## HUMAN REVIEW GATE

- **I assumed:** Testing uses non-production accounts and configured providers; Passkey registration is covered elsewhere; the ticket AC is the source of truth where details are explicit.
- **I could not confirm:** Current implementation/deployment, approved test environment, SSO providers, supported Passkey platform matrix, test data, exact error/fallback copy, performance/accessibility/localization targets, feature flag/rollback, or the cause of prior blocked runs.
- **Open questions blocking sign-off:** Resolve the priority conflict and questions in Section 2, especially provider/platform coverage, failure behavior, test setup, security expectations, and the repeated blocked test executions.
- **Approve or edit this draft before it is treated as approved. Do not start detailed test-case or automation authoring until a human approves the plan.**
