# Example: Backend Microservices with Protobuf Contracts

## Objective
Implement inter-service communication across independent services while enforcing backward compatibility on Protobuf definitions and message queues.

## Dependency DAG
```text
WP-100 Proto Contract Freeze
  +--> WP-210 Order Service -------+
  +--> WP-220 Payment Service -----+--> WP-500 Cross-Service E2E
  +--> WP-230 Notification Service +
```

## Boundary and Resource Allocation
```text
WP-100: proto/** (Frozen contracts)
WP-210: services/order/**, Port 8081, Queue `order.events`
WP-220: services/payment/**, Port 8082, Queue `payment.events`
WP-230: services/notification/**, Port 8083, Consumer only
```

Central API gateway configurations and service discovery manifests are designated `INTEGRATION_ONLY`.

## Verification Protocol
Each service executes isolated unit and contract mock tests. The Integrator applies sequential merges, validates Protobuf wire compatibility, and executes end-to-end integration tests using localized network bridges.