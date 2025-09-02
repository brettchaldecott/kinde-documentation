
Feature flags, along with other kinds of development flags, are an essential part of building and managing digital products. Feature flags (sometimes called feature toggles) control who can see features in specific environments for your app or site.

Feature flags are defined by a `key` identifier, flag `type`, and flag `value`, which gets included in your code.

## What to use feature flags for

Feature flags let you control user access to specific features as they are being built. They keep untested features out of the hands of your customers and safe from accidental discovery.

A simple application of a feature flag is to create a flag when you start working on a feature, then remove the flag when the feature is ready for release.

There’s many benefits to using feature flags.

- Engineers can keep their in-progress work confined to a development environment
- Allows for more secure feature testing prior to release
- Enables more granular release control, e.g. for continuous delivery
- Gives you the ability to target users with specific features

## Types of feature flags in Kinde

There are several types of feature flags that can be used for different reasons. Kinde currently offers:

- **Boolean** - operate like an on/off switch using `true | false` values. Ideal for showing and hiding features or switching functions on and off.
- **String** - to pass configuration values, e.g. colour, and content such as label text, etc. Say if you want to test if a green or a red button gets more clicks.
- **Integer** - for updating numeric values using whole numbers only. Decimal numbers will be trimmed after the decimal point. The integer range is -2147483648 to +2147483647.
- **JSON** - allow you to define a set of key-value pairs in a JSON file, where each key represents a feature or functionality in your application.

## Overriding feature flags

Feature flag values cascade downward from the business level setting. You can also allow overrides to be applied in environments, organizations, or at the user level, as part of the flag definition.

See [override feature flag values](/releases/feature-flags/edit-feature-flag/).

| When the value is set or changed…         | then the value is defined for…                                 |
| ----------------------------------------- | -------------------------------------------------------------- |
| at the business level where it is created | all users in the environment and organizations in the business |
| in an environment                         | all users in the environment                                   |
| for an organization                       | all users in the organization                                  |
| for a user                                | that user in every organization they belong to                 |

## Feature flag use cases

### Boolean

You’re building a new major feature such as ‘notifications’ and you want to make sure that nobody can use it until development is complete. You set the feature value to `false` for everyone, to ensure it is hidden. But because you want to test it later, you also make sure that the value can be overriden for your testing environment.

Once you’ve finished building and you’re ready to test, you change the value to `true` for the testing environment to enable your internal QA team to look at it.

When it’s finished and bug-free, you can go back and change the value to `true` so all environments and organizations can access it. Released!

### String

You want to test out some new button names in your app. You create a string type flag called `buy-button` where the value can be overridden for different environments and organizations.

Instead of making changes in production, test them behind a flag to select organizations or cohorts before you decide what works best. For example, you could test if `buy it now` or `add to cart` is a more effective call to action.

Another use for string flags is to apply a unique color scheme for each different environment you work in. Developers frequently switch between production, staging, and local environments as they work, and with string flags it’s easy to make these visually distinct.

In his own project, one of Kinde’s founders created the `heading-color` string flag to distinguish each of his three environments: `yellow` for local, `orange` for staging, and `pink` for production.

### Integer

Less used for releasing features than for controlling various elements in your app or site, integer flags enable more granular control in certain situations.

For example, say you want to set the number of items you display on a page in your online store, you can use an integer flag to create a page limit. You might set the display limit to 12 for mobile devices, and to 24 for desktop.

Integer flags can also be used to set other limits, such as limiting the number of concurrent requests to an API endpoint, or to limit the number of password attempts before locking a user out. The advantage of using a flag for this is being able to edit the value and deploy changes quickly and easily, without major code updates.

### JSON

With JSON feature flags, you can define a set of key-value pairs in a JSON file, where each key represents a feature or functionality in your application.

For example:

```json
{
  "is_dark_mode": true,
  "font_size": "large",
  "employee_count": 5
}
```

JSON feature flags might suit you if you use a separate feature flag management system or library for your application.
