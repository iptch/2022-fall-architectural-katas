# Connection Capability
The connection capability enables civilians and officers to connect through the **Hey, Blue!** ecosystem. The following capabilities are provided by this capability.
- Civilians can look up officers who agreed/configured on their site to be found by search. In that case an officer opted in for look-up.
- Civilians and officers might connect via QR code or the previously described online look-up. The processes are clearly defined and supported by a notification infrastructure based on messaging.
- Civilians are being rewarded **Hey, Blue!** points when successfully connecting with officers.
- Officers are being rewarded **Hey, Blue!** points when successfully connecting with civilians.
- When connecting via QR code, a proximity matching service verifies proximity of the participants by assessing a history of geospatial data.
- Civilians might share their connection with social media platforms.

The following diagram describes the microservice architecture for this capability in detail.
<p align="center">
<img width="1000" src="../domain/resources/hey-blue-connection.drawio.svg">
</p>

<img width="550" src="resources/hey-blue-legend.drawio.svg">


## Services

### Officer Lookup
- Stores officer information when officers opted in for look-up by setting the respective preferences in their profile.
- Civilians can find those officers by look-up in their mobile application. A restriction on location / zip code might be put in place. Further policies might be enforced.

### Proximity Matching
- Database holding location streaming data, e.g. `{"user": "userId", {"long": "...", "lat": "..."}, "timeStamp": ...}` entries, for all users and officers with active location tracking.
- The service is being used by the connection service to make sure officers and users are nearby when a connection is being established.
- Since we use a messaging system, such as Kafka, anyway, spatial data can be streamed and stored within Kafka. This is espetially handy since we have no intention of storing this spatial data long-term in which case Kafka might not be the best choice.

### Connection
- This service stores the actual connections. 
- It is being triggered by user interaction through the BFF, checks up on user data, reconciles with the proximity matching and triggers the appropriate notifications for the connection establishment workflow.

### Social Media
- This is simply an abstraction layer to publish connections on various social media platforms.
- Various social media platforms might be added as plugins over time.

### User
- This service is described in more detail in the [User capability](../domain/user-capability.md).
- Points created during a connection establishment need to be awarded to the user (both civilians and officers).

### Notification
- Handles incoming notifications and produces outgoing notifications for the user.
- Makes sure the user is not spammed with notifications, so a history of notifications per user has to be retained. Example: A user is nearby a shop which participates on the Hey, Blue! Platform.
- Notifications are being distributed over an asynchronous eventing system like Kafka or Azure Event Hub.

## Synchronous vs Asynchronous Calls
Notifications and the awarding of points can be asynchronous to decouple the system and handle load spikes gracefully (see [ADR02 Event-Driven Design](../ADRs/2022-10-31_02-event-driven-design.md)). Some operations like the lookup of an officer or the connection service querying the proximity matcher should be synchronous to receive an instantaneous result which blocks the current flow of interactions.

## Related ADRs
- [ADR01 Microservice Architecture](../ADRs/2022-10-31_01-microservice-architecture.md)
- [ADR02 Event-Driven Design](../ADRs/2022-10-31_02-event-driven-design.md)
- [ADR05 Backend-for-Frontend Pattern](../ADRs/2022-11-01_05-bff.md)

