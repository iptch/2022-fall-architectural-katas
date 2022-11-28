# Order Capability
The order capability describes the service landscape enabling Civilians or Charities to redeem their points. The shown 
services support the architecture decision described in [the redeem points ADR](../ADRs/03-redeem-points.md).

![Order Capability](resources/hey-blue-order-capability.drawio.svg)
<img width="750" src="resources/hey-blue-legend.drawio.svg">


It should be mentioned that the process of redeeming points is a triangular contract between a user 
(might be a civilian or charity in that case), the business itself and the **Hey, Blue!** webshop as 
shown in the following diagram.

<p align="center">
<img width="550" src="../ADRs/resources/hey-blue-redeem-points-modelling.drawio.svg">
</p>

## Services
The following list describes main responsibilities of the different services in the Order Capability.

### Catalog
- Provides the catalog of purchasable goods and services

### Checkout
- Provides a shopping cart per user. This cart is persisted to a database to enable a seamless transition between devices.
- Calculates discounted prices with informations from Catalog (offering) and User (available Points) services.
- Verifies the payment authorization with the Payment Gateway service.
- Sends the finalized order to the Order service.

### User
- Provides information about available Points of the logged-in User.

### Payment Gateway
- Anti-Corruption Layer to integrate different modern Payment Providers (e.g. PayPal, Stripe, Apple Pay, ...)
- Acknowledges the Checkout service that a payment is authorized.
- Acknowledges the Order service that a payment is being processed.

### Order
- As orders are long-living entities in our domain, they are persisted in a database.
- Instructs Payment Gateway to process a payment.
- Updates catalog accordingly to order.
- Queues paid orders for dispatching.
- Updates order state when an order is dispatched or shipped

### Dispatcher
- Microkernel Architecture with Plugins for communicating orders with Businesses (see [ADR04 Dispatcher Architecture](../ADRs/04-dispatcher-architecture.md)).
- Takes orders from Order services.
- Sends order to Business.
- Receives order confirmation, declines and shipment updates from Business.
- Queues status updates for Order servie.

### Notification
- Dedicated service to process notifications.

## Behavior
The following sequence diagram shows the control flow from the moment in which the User decides to checkout his cart.

![](resources/hey-blue-order-behavior.drawio.svg)

## Synchronous vs Asynchronous Calls
Some calls, e.g. to Notification service, can be asynchronous to decouple the system and handle load spikes gracefully (see [ADR02 Event-Driven Design](../ADRs/2022-10-31_02-event-driven-design.md)). Especially the communication with the Dispatcher service, where each Plugin may handle orders differently (including different delays and responses), should not block the checkout process. It is common for Users to receive information about shipment hours or even days after checkout.

Operations that require interaction with the User (e.g. authorizing a payment) should be synchronous to block the current process until the result is received.

## Related ADRs
- [ADR01 Microservice Architecture](../ADRs/2022-10-31_01-microservice-architecture.md)
- [ADR02 Event-Driven Design](../ADRs/2022-10-31_02-event-driven-design.md)
- [ADR03 Points Redemption Framework](../ADRs/2022-10-31_03-redeem-points.md)
- [ADR04 Dispatcher Architecture](../ADRs/2022-10-31_04-dispatcher-architecture.md)
- [ADR05 Backend-for-Frontend Pattern](../ADRs/2022-11-01_05-bff.md)
