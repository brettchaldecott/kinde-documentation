
Kinde gives you several options for extracting your data, while we work toward providing a more comprehensive analytics feature.

## Google analytics

Kinde lets you integrate with [Google Analytics](/manage-users/view-activity/track-user-sign-in-with-google-analytics/), so you can track sign up patterns, rates, frequency, etc.

To start using it, enter your Google UA or G-tag in **Settings > Environment > Details > Tracking**.

## The state parameter

If GA does not give you the specific information you need, you might be able to use the `state` parameter as a solution.

The `state` parameter is used to store a unique code for each sign up flow, adding security to the authentication experience. But It can also be harnessed to correlate unique marketing IDs (such as UTM codes) along with it.

Here’s how it works:

1. Your campaign ad carries a unique ID for the campaign.
2. A user clicks through the campaign to sign up to your app or product.
3. A unique event code is generated and the unique ID from the ad is picked up and passed through the Kinde auth process in the `state` parameter.
4. A token is issued for the user by Kinde, then Kinde redirects your user back, passing the same `state` parameter. The value of the `state` can then be used to correlate back any ad-specific information or call the advertiser’s API.

Any information you need after the authentication flow, can be correlated with the passed through `state` parameter value. See the [definition of State in Using Kinde without an SDK](/developer-tools/about/using-kinde-without-an-sdk/#state).
