# Stripe PaymentIntent To Donation Flow

## Prerequesites:
- Install [Stripe Universal Connector](https://appexchange.salesforce.com/appxListingDetail?listingId=4dff0f8e-0b10-47c2-a3a3-f3905e7f7927)
- Authorize Connector and activate paymentintent.succeeded Event
- Brandon's [Find or Create Contact Flow](https://github.com/EncludeLtd-Donor-Import-Configurations/Find-or-Create-Contact-Flow)


[Stripe UC Documentation](https://docs.stripe.com/connectors/stripe-connector-for-salesforce/overview)

## Considerations
- Opportunity Process Stage options may differ. Default in flow closes Opportunities to 'Closed Won' but depending on the Org you may need to change this in the imported flow
- Formulae are used to separate Customer Name field into First and Last names, but this may not play well with 3 or more names.
- A Name generation formula is available for defining the name of created Recurring Donations
- Close Date formulae are available for defining an acceptable close date period to match within
- Stripe allows for Subscription values to be updated in later payments. This flow alone will not match changed value payments, and needs a second subscription.updated event flow to ensure this is caught.
- This flow alone does not close Recurring Payments if a Sub is canceled. This will need a subscription.deleted event flow to handle this.
- The flow creates barebones Contacts with just a name and email address. Needs some config to also bring in addresses, define record types, or pull in metadata entered in Stripe as per Org needs. How this is entered can vary by what is triggering Stripe.

## Included
### Custom Fields
 - Creates a Stripe Field on Recurring Donation to carry Subscription ID (Default name in flow 'Stripe_Sub_ID__c')
 - Creates a Stripe Field on Opportunity/Donation to carry Payment Intent ID (Default name in flow 'Stripe_ID__c')
### Flows
 - Creates a record triggered Flow called StripePI_toDonation_toRecurringDonation to handle incoming Donations
 - Todo: Record triggered flow to handle subscription amount changes
 - Todo: Record triggered flow to cleanly close Recurring Donations when a Subscription is deleted

## Actions
- This flow generates Donation and Recurring Donation records from Stripe Payment Intent Succeeded Events.
- This flow makes up to two GETs from Stripe API for additional data, depending on the payment type.
  - GET Customer for name data to use for Contact matching
  - GET Invoice for Subscription ID to use for Recurring Donation matching

## Deploy
<a href="https://githubsfdeploy.herokuapp.com?owner=CiaraEnclude&repo=StripePIEvent_ToDonation_ToRecurringDonation&ref=main">
  <img alt="Deploy to Salesforce"
       src="https://raw.githubusercontent.com/afawcett/githubsfdeploy/master/deploy.png">
</a>
