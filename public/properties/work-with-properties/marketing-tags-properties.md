
Marketing analytics relies on data about customer behaviour. This can include how a customer finds you, e.g. through a campaign, and what a customer does when they interact. Kinde allows you to pass marketing tag data in tokens, and store it as [properties](/properties/about-properties/) in Kinde. From here, you can extrapolate campaign success and other metrics you wish to track.

## What tracking tags are supported?

Out of the box Kinde gives you 3 categories containing marketing properties which are available at both an organization and user level:

### UTM tags category

Contains the following properties:

* UTM source `utm_source`
* UTM medium `utm_medium`
* UTM campaign `utm_campaign`
* UTM content `utm_content`
* UTM term `utm_term`

### Google Ads smart campaign tracking

Contains the following properties:

* Google ID `gclid`
* Google click ID `click_id`
* HSA account `hsa_acc`
* HSA campaign `hsa_cam`
* HSA group `hsa_grp`
* HSA ad `hsa_ad`
* HSA source `hsa_src`
* HSA target `hsa_tgt`
* HSA keyword `hsa_kw`
* HSA match type `hsa_mt`
* HSA network `hsa_net`
* HSA version `hsa_ver`

### Marketing category

Contains the following properties:

* Match type `match_type`
* Keyword `keyword`
* Device `device`
* Ad group `ad_group_id`
* Campaign ID `campaign_id`
* Creative `creative`
* Network `network`
* Ad position `ad_position`
* Facebook ID `fbclid`
* LinkedIn ID `li_fat_id`
* Microsoft ID `msclkid`
* X ID `twclid`
* TikTok ID `ttclid`

## Getting marketing tags into Kinde

There are a few mechanisms for updating these property values in Kinde.

### Pass the tracking tags via the auth flow

This is the most common method. For example, if you want to pass in the UTM source, you just need to forward the `utm_source` query parameter as part of the sign up flow authentication url. Most of our SDKs allow you to pass this in via additional URL params. 

For example in the React SDK:

```jsx
<RegisterLink
  className="btn btn-dark
  properties={{
    utm_source: "my source",
    utm_medium: "some medium",
    utm_campaign: "awesome campaign",
    utm_content: "something else",
    utm_term: "my terms",
    click_id: "1234"
  }}
>Register</RegisterLink>
```

We automatically map the standard query parameters to the specific Kinde properties for you. For example `utm_source` URL param will be mapped to the `kp_usr_utm_source` Kinde parameter for a user and `kp_org_utm_source` for an organization. We map these automatically so that you do not need to make changes to the parameters in your code.

The property values are set during the sign up flow for new users and at the point of organization creation for new organizations. 

### Update the tracking tags manually

* Via dashboard - Open a user or organization in Kinde, then select *Properties* from the side navigation. Update the relevant property.
* Via the Kinde Management API - 
    * [Update user property](https://docs.kinde.com/kinde-apis/management/#tag/users/put/api/v1/users/{user_id}/properties/{property_key}) - make sure to add `kp_usr_` as a prefix to the standard tag key.
    * [Update organization property](https://docs.kinde.com/kinde-apis/management/#tag/organizations/put/api/v1/organizations/{org_code}/properties/{property_key}) - make sure to add `kp_org_` as a prefix to the standard tag key.

## Accessing marketing tags data

* Via dashboard - Open a user or organization in Kinde, then select *Properties* from the side navigation.
* Via the Kinde Management API -
    * [View user properties](https://docs.kinde.com/kinde-apis/management/#tag/users/get/api/v1/users/{user_id}/properties)
    * [View organization properties](https://docs.kinde.com/kinde-apis/management/#tag/organizations/get/api/v1/organizations/{org_code}/properties) 

## Check how specific marketing tags are performing
    
Likely you will want to see which tags are converting into sign ups and are performing well. For this, you can use the User search API to return all users that match a specific property value. For example, if you wanted to see all users who signed up with a UTM source of "Hello" and a UTM campaign of "World" you could use this query:

```
/api/v1/search/users?query=properties[kp_usr_utm_source]=Hello&properties[kp_usr_campaign]=World
```

The search organizations API is coming soon.