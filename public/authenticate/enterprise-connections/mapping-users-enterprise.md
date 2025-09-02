
When you use Kinde to authenticate users via an enterprise connection such as SAML, you also need a way for users to be identified in Kinde so they match the identities stored in your Identity Provider (IdP).

In general, an email address can be used to map users across systems, but because enterprise connection users can have aliases and proxy addresses, there are better ways to keep identities in sync.

Here’s how we recommend mapping user profiles and keeping them synced for enterprise connections.

## Map the Kinde user ID to your product

When users are imported or added to Kinde, a unique user ID is generated. For example, `kp:1876b10742894a0c9M8e725048e7a323`. We recommend you map each user’s Kinde ID back to your product, and use this as the primary auth identifier. This will keep profiles in sync and support a seamless authentication experience.

### Get Kinde user IDs via API

You can access user IDs [via the API](http://localhost:4321/kinde-apis/management#tag/users/post/api/v1/users/{user_id}/identities) by calling GET `/api/v1/users`. This will return a response with a users array with the following data:

```json
{
  "code": "string",
  "message": "string",
  "users": [
    {
      "id": "string",
      "provided_id": "string",
      "email": "string",
      "username": "string",
      "last_name": "string",
      "first_name": "string",
      "is_suspended": true,
      "picture": "string",
      "total_sign_ins": 0,
      "failed_sign_ins": 0,
      "last_signed_in": "string",
      "created_on": "string",
      "organizations": ["string"],
      "identities": [
        {
          "type": "string",
          "identity": "string"
        }
      ]
    }
  ],
  "next_token": "string"
}
```

Where:

- `id` is the kinde ID
- `provided_id` is the ID you may have provided when you imported your users. This ID can also be useful to match imported users to your local database records.

## Switch on profile sync

As part of your business authentication setup, we recommend switching on user profile sync to keep enterprise and social profiles up to date across providers.

1. Go to **Settings > Policies**.
2. Switch on **Sync user profiles on sign in**.
3. Select **Save**.

## Sync users with webhooks

Webhooks are a method of being notified when an event occurs in Kinde, e.g. a user is created. You can [register your own endpoint URLs](/integrate/webhooks/add-manage-webhooks/) in Kinde, and each time the event occurs, data for that event will be sent to your endpoint.

Here’s some examples of webhook events that can be used to keep your users in sync:

- `user.created` - when a user is created in Kinde either via the admin UI or registering
- `user.updated` - when a user is added to an organization, or their roles or permissions change
- `user.deleted` - when a user is deleted via the UI or via the API

Here’s an example json schema for user.updated that could be used to sync your data:

```json
{
  "id": "et_018df239698d29177684be3f5ad1266d",
  "code": "user.updated",
  "name": "User updated",
  "origin": "kinde",
  "schema": {
    "$id": "https://kinde.com/user.updated.schema.json",
    "type": "object",
    "title": "User Updated Webhook Event",
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "properties": {
      "data": {
        "type": "object",
        "properties": {
          "user": {
            "type": "object",
            "properties": {
              "id": {
                "type": "string",
                "pattern": "kp[:_][0-9a-f]{32}",
                "description": "ID of the user"
              },
              "phone": {
                "type": ["null", "string"]
              },
              "last_name": {
                "type": ["string", "null"],
                "description": "The users updated last name"
              },
              "first_name": {
                "type": ["string", "null"],
                "description": "The users updated first name"
              },
              "is_suspended": {
                "type": "boolean",
                "description": "The users updated status"
              },
              "organizations": {
                "type": ["array", "null"],
                "items": [
                  {
                    "type": "object",
                    "properties": {
                      "code": {
                        "type": "string",
                        "pattern": "org[:_][0-9a-f]{11}"
                      },
                      "roles": {
                        "type": ["array", "null"],
                        "items": [
                          {
                            "type": "object",
                            "properties": {
                              "id": {
                                "type": "string",
                                "format": "uuid"
                              },
                              "key": {
                                "type": "string"
                              }
                            }
                          }
                        ]
                      },
                      "permissions": {
                        "type": ["array", "null"],
                        "items": [
                          {
                            "type": "object",
                            "properties": {
                              "id": {
                                "type": "string",
                                "format": "uuid"
                              },
                              "key": {
                                "type": "string"
                              }
                            }
                          }
                        ]
                      }
                    }
                  }
                ]
              },
              "is_password_reset_requested": {
                "type": "boolean",
                "description": "The users updated password reset status"
              }
            },
            "description": "User event data"
          }
        },
        "description": "Webhook event data"
      },
      "type": {
        "constant": "user.updated"
      },
      "source": {
        "enum": ["api", "admin"],
        "description": "Source of the action"
      },
      "event_id": {
        "type": "string",
        "pattern": "event_[0-9a-f]{32}",
        "description": "ID of the event"
      },
      "timestamp": {
        "type": "string",
        "format": "date-time",
        "description": "Datetimestamp of the action"
      }
    },
    "description": "Webhook detail the user updated event"
  },
  "version": 1
}
```

You can see a full list of events in the Kinde UI under **Settings > Webhooks**, or by calling the [Kinde management API](/kinde-apis/management#tag/webhooks) which also provides the JSON schema GET `/api/v1/event_types`.

Read more [about webhooks](/integrate/webhooks/about-webhooks/).
