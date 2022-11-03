# Order Capability
The order capability describes the service landscape enabling Civilians or Charities to redeem their points. The shown services support the architecture decision described in [](ADRs/redeem-points.md).

![Order Capability](resources/hey-blue-order-capability.drawio.svg)

## Services

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
- Microkernel Architecture with Plugins for communicating orders with Businesses (see [ADR04](../ADRs/04-dispatcher-architecture.md)).
- Takes orders from Order services.
- Sends order to Business.
- Receives order confirmation, declines and shipment updates from Business.
- Queues status updates for Order servie.

### Notification
- Dedicated service to process notifications.

## Behavior
TODO: create sequence diagram