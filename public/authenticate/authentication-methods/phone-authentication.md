
You can allow users to use their phone as a primary method for authentication. This is a passwordless method, where the user is sent a verification code via SMS. SMS can also be included as a secondary factor if you have [multi-factor authentication](/authenticate/multi-factor-auth/about-multi-factor-authentication/) set up. 

<Aside>

This feature requires paid third-party services to use. Rates and limitations apply.

</Aside>

## (Existing phone auth Twilio users only) Switch on SMS for MFA

1. In Kinde, go to **Settings > Environment > SMS**.
2. Scroll to the bottom and switch on the **Use this service for SMS MFA** option.
3. Select **Save**.

## Benefits of using a third-party SMS service instead of Kinde

- Gives you full control over the SMS delivery nuances, such as SenderID, country registrations, and detailed delivery metrics. 
- You can register dedicated short codes or sender IDs in countries that have strict SMS sending regulations like Ireland, NZ and Canada, which will greatly improve deliverability.
- Access to delivery logs and other service quality details for troubleshooting.

## SMS provider requirements (Twilio)

SMS authentication requires the services of a messaging provider, in this case, [Twilio](https://www.twilio.com/en-us).

You need a [Twilio](https://www.twilio.com/en-us) business account to ensure messaging works for local and overseas phone numbers.

Phone authentication interactions are also known as [A2P (Application to Person)](https://www.twilio.com/docs/glossary/what-a2p-sms-application-person-messaging) messaging. Before you implement A2P, check if you need to register your business for 10DLC (10 Digit Long Code) support to be able to send messages, as this is required in some locations.

We also recommend you check [Twilio’s guidelines for setting up messaging](https://www.twilio.com/en-us/guidelines/sms), and carefully follow procedures for registration, and SMS policies for all relevant countries.

## What you need

<Aside>

If you just want to test this feature first, Kinde allows you to send 10 SMS messages per month without setting up Twilio. If you want the feature to be live for your users, you must implement the full Twilio setup.

</Aside>

You’ll need the following details that are in the dashboard of your [Twilio account](https://www.twilio.com/en-us).

- The SID of your Twilio account
- The Auth Token for your Twilio account
- Your Twilio phone number or the Messaging Service SID (if you set one up)

![Twilio account info](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/1da93a0a-9fd3-437f-5357-be90f3f3c200/public)

Refer to the [Twilio documentation](https://www.twilio.com/docs/messaging/services/tutorials/send-messages-with-messaging-services) for assistance setting up.

## Configure phone SMS auth in Kinde

After you set this up, you can use SMS for both phone authentication and SMS MFA.

1. In Kinde, go to **Settings > Environment > SMS**.
2. Select the **Default country** that you want to show on the authentication screen when users sign in.
3. Enter the Twilio details from your Twilio account (see above) in the relevant fields.

   ![twilio details](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/4c857ff9-ff87-44ea-a488-3e2b511caf00/public)
 
4. In the **SMS source** field, select either the **Use** **Messaging service** or **Use phone number**. Verification codes will be sent from whichever you choose.

   <Aside>

   Note that the Twilio messaging service is more suitable for global applications as it detects where the sign in comes from and sends from an appropriate number.

   </Aside>

5. Depending on your selection in the previous step, enter either the **Messaging service SID** or Twilio **Phone number** in the relevant field.

   ![Twilio config](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/749a80bc-d6b7-40b0-950a-650c7775b900/public)

6. Select if you want to use a fallback service if the provider service is interrupted.

   ![option to use kinde sms as fallback](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/9bdd2ef1-c308-4307-c84e-bc8ffdbfe200/public)

7. Select **Save**.

## Switch on phone authentication for an application

After you have set up Twilio details, you’re ready to switch on phone or SMS auth for your applications.

1. Go to **Settings > Environment > Authentication**.
2. In the **Passwordless** section, select **Configure** on the **Phone** tile.
3. Switch on the auth method for the applications you want.
4. Select **Save**.

## Switch on SMS as a factor in MFA

If MFA is required or optional for your users, you may want to use the Twilio service for SMS MFA.

1. Go to **Settings > Environment > Multi-factor auth**.
2. Under **Additional authentication methods**, switch on **SMS**.
3. Select **Save**.

## SMS message format

You can’t customize the code message that user’s receive. We use a standard format as follows, to allow for easier translation.

**Your verification code is [xxxxxx]**

## Connection ID

When you configure phone authentication, you’ll see that a Connection ID is automatically assigned. If you’re building a [custom authentication experience](/authenticate/custom-configurations/custom-authentication-pages/), you’ll need the ID to trigger the phone authentication workflow.
