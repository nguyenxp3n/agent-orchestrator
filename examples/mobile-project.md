# Example: Mobile App

## Boundaries

Decompose the domain/data layer, feature screens, design system, platform integrations, and backend contract consumers. Shared navigation root, app bootstrap, and entitlement/signing config are commonly integration-only.

## Example Allocation

```text
WP-PROFILE-DATA   -> src/features/profile/data/**
WP-PROFILE-UI     -> src/features/profile/ui/**
WP-NOTIFICATIONS  -> platform notification adapter
WP-TESTS          -> feature integration tests
NAVIGATION ROOT   -> INTEGRATION_ONLY
```

## Risks

Simulator/device differences, signing credentials, iOS/Android platform code, generated client code. Secret/signing actions may require human authority.

## Verification

Use actual project commands: Gradle/Xcode/Flutter/React Native tests. If the device farm is inaccessible, local acceptance may pass while release/device assurance remains `UNKNOWN`.