# StripeCharge To Donation & Recurring Donation Flow

## Prerequesites:
- Install [Stripe Universal Connector](https://appexchange.salesforce.com/appxListingDetail?listingId=4dff0f8e-0b10-47c2-a3a3-f3905e7f7927)
- Authorize Connector and activate paymentintent.succeeded Event
- Brandon's [Find or Create Contact Flow](https://github.com/EncludeLtd-Donor-Import-Configurations/Find-or-Create-Contact-Flow)


[Stripe UC Documentation](https://docs.stripe.com/connectors/stripe-connector-for-salesforce/overview)

## Considerations
- Opportunity Process Stage options may differ. Default in flow closes Opportunities to 'Closed Won' but depending on the Org you may need to change this in the imported flow
- Formulae are used to separate Customer Name field into First and Last names, but this may not play well with 3 or more names.
- A Name generation formula is available for defining the name of created Recurring Donations & Opportunities
- Close Date formulae are available for defining an acceptable close date period to match within
- Stripe allows for Subscription values to be updated in later payments. This flow alone will not match changed value payments, and needs a second subscription.updated event flow to ensure this is caught.
- A second flow is included to manage subscription deletion events by closing the recurring Donation - edit to set sales process close status to comply with org installed in.
- The flow creates barebones Contacts with just a name and email address. Needs some config to also bring in addresses, define record types, or pull in metadata entered in Stripe as per Org needs. How this is entered can vary by what is triggering Stripe, but is usually on the Charge>Billing or on Customer.

## Included
### Custom Fields
 - Creates a Stripe Field on Recurring Donation to carry Subscription ID (Default name in flow 'Stripe_Sub_ID__c')
 - Creates a Stripe Field on Opportunity/Donation to carry Payment Intent ID (Default name in flow 'Stripe_ID__c')
### Flows
 - Creates a record triggered Flow called StripeCharge_ToDonation_ToRecDonation to handle incoming Donations
 - Todo: Record triggered flow to handle subscription amount changes
 - Todo: Record triggered flow to cleanly close Recurring Donations when a Subscription is deleted

## Actions
- Charge flow generates Donation and Recurring Donation records from Stripe Charge Succeeded Events.
- Charge flow makes up to three GETs from Stripe API for additional data, depending on the payment type.
  - GET Customer for name data to use for Contact matching
  - GET Invoice for Subscription ID to use for Recurring Donation matching
  - GET Payment Intent for Lead Source
- Subscription Deleted flow matches a Recurring Donation and calls NPSP Action to close RD cleanly.

## Post-install
- Edit the flows to bring in your required metadata. This varies per org.
- Metadata requires you to loop over the metadata packet and assign each value to a variable based on label. These new variables can then be used further in the flow.

## Deploy
<a href="https://githubsfdeploy.herokuapp.com?owner=Enclude-Components&repo=StripeCharge_ToDonation_ToRecDonation&ref=main">
  <img alt="Deploy to Salesforce"
       src="https://raw.githubusercontent.com/afawcett/githubsfdeploy/master/deploy.png">
</a>
