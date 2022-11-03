# ADR03 Points Redemption Framework

## Title
How do Civilians and Charities redeem their Points?

## Status
Proposed

## Context
The design document provided by the Product Owner does not specify the process of redeeming Points. We identify this process as important to specify because of two reasons:
1) Money is a sensitive topic. Included Actors (User (Civilians or Charities), **Hey, Blue!**, Business) want their information to be processed in a secure and trusted manner.
2) *Ease of integration* is key to onboard new Businesses to **Hey, Blue!**. All kinds of Businesses, regardless of their size or the goods they deal with, need to be able to participate in **Hey, Blue!**. The way Users redeem their Points highly influences how **Hey, Blue!** is integrated in the system of the Business.

## Decision
The User can list and checkout offerings of all Businesses in the **Hey, Blue!** webshop (1). The User pays **Hey, Blue!** at checkout via a trusted Payment Provider (e.g. PayPal, Credit Card) (2). **Hey, Blue!** then places the order at the corresponding Business (3). If the Business accepts the order it sends a confirmation to **Hey, Blue!** (4) und the products directly to the User (5). The Business is then paid by **Hey, Blue!** (e.g. directly, monthly).

![](./resources/hey-blue-redeem-points-modelling.drawio.svg)

## Consequences
Not ordered by importance:
- Hey Blue hosts a [PA-DSS](https://listings.pcisecuritystandards.org/minisite/en/docs/PA-DSS_v3.pdf) compliant web shop with offerings from participating Businesses.
- Businesses can easily participate in Hey Blue. They do not need to integrate special software. After registering with Hey Blue they can create offerings in the Hey Blue web shop. Offerings can be of any type (e.g. one product, vouchers, services).
- Users can only redeem Points at the Hey Blue webshop. If they want to make an on-site purchase they can pay with a voucher from the webshop.