# Connection Capability
The connection capability is what enables civilians and officers to connect through the *Hey, Blue!* ecosystem. The functionalities provided are
- Civilians can look up officers who agreed/configured on their site to be found by search.
- Civilians and officers creating a connect via QR code or online lookup and the respective notifications.
- Civilians being rewarded Hey, Blue! points into their wallet.
- Proximity matching when connection via QR code.
- Upload connection to social media.

The following diagram describes the microservice architecture for this capability in detail.
<p align="center">
<img width="800" src="../domain/resources/hey-blue-connection.drawio.svg">
</p>

<p align="center">
<img width="450" src="../domain/resources/hey-blue-connection-legend.drawio.svg">
</p>

## Services

### Officer Lookup
- Stores officer information when officers opted in for lookup by setting the respective preferences in their profile.
- Civilians can find those officers by lookup. A restriction on location / zip code might be put in place.

### Proximity Matching
- Database holding location streaming data, e.g. {user: "userId", {long: "...", lat: "..."}} entries, for all users and officers with active location tracking.
- Us being used by the connection service to make sure officers and users are nearby when a connection is being established.
- Since we use a Kafka cluster anyway, spatial data can be streamed and stored within Kafka. This is espetially handy since we have no intention of storing this spatial data long-term in which case Kafka might not be the best choice.

### PConnection
- This service stores the actual connections. It is being triggered by user interaction through the BFF, checks up on user data, reconciles with the proximity matching and triggers the appropriate notifications for the connection establishment workflow.

### Social Media
- This is simply an abstraction layer to publish connections on various social media platforms.

### User
- This service is described in more detail in the [User capability](../domain/user-capability.md).
- Points created during a connection establishment need to be awarded to the user.

### Notification
- Handles incoming notifications and produces outgoing notifications for the user.
- Makes sure the user is not spammed with notifications, so a history of notifications per user has to be retained. Example: A user is nearby a shop which participates on the Hey, Blue! Platform.

## Synchronous vs Asynchronous Calls
Notifications and the awarding of points can be asynchronous to decouple the system and handle load spikes gracefully. Some operations like the lookup of an officer or the connection service querying the proximity matcher should be synchronous to receive an instantaneous result which blocks the current flow of interactions.

## Related ADRs
- [BFF](../ADRs/02-bff.md)

