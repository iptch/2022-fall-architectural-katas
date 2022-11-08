# User Capability

This capability is responsible for maintaining user sessions, storing user data, registering new users and keeping track
of connections between officers and civilians.

![User Capability](resources/hey-blue-user.drawio.svg)
<img width="750" src="resources/hey-blue-legend.drawio.svg">

## Components

### Profile
- Service responsible for handling user sessions, new user registrations and keeping track of connection data.
- User identity is fetched from external identity provider.

### External Identity Provider
- E.g. Auth0
- Handles user sign in using SSO, user/password, etc.

### User Data
- Contains profile, location history, connections, points awarded and purchase history.
- GDPR compliant

### Point queue
- E.g. Kafka Topic
- Connection service awards points which are then persisted to the respective user profiles


## Related ADRs
- [ADR01 Microservice Architecture](../ADRs/01-microservice-architecture.md)
- [ADR02 Backend-for-Frontend Pattern](../ADRs/02-bff.md)
- [ADR03 Points Redemption Framework](../ADRs/03-redeem-points.md)
- [ADR06 GDPR Compliance](../ADRs/06-GDPR-compliance.md)


## Related Views
- [Connection Capability](connection-capability.md)