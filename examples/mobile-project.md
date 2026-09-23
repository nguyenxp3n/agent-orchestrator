# Example: Mobile App

## Boundaries

Tách domain/data layer, feature screens, design system, platform integrations và backend contract consumers. Shared navigation root, app bootstrap, entitlement/signing config thường là integration-only.

## Example Allocation

```text
WP-PROFILE-DATA   -> src/features/profile/data/**
WP-PROFILE-UI     -> src/features/profile/ui/**
WP-NOTIFICATIONS  -> platform notification adapter
WP-TESTS          -> feature integration tests
NAVIGATION ROOT   -> INTEGRATION_ONLY
```

## Risks

Simulator/device differences, signing credentials, iOS/Android platform code, generated client code. Secret/signing actions có thể cần human authority.

## Verification

Dùng project commands thực tế: Gradle/Xcode/Flutter/React Native tests. Nếu device farm không truy cập được, local acceptance có thể pass nhưng release/device assurance vẫn `UNKNOWN`.