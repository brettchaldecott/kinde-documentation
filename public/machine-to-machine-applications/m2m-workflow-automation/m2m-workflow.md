
Kinde lets you run your own code via the [M2M token generation workflow](/workflows/example-workflows/m2m-token-generation-workflow/) whenever a machine-to-machine (M2M) token is generated. This gives you a powerful way to react to token usage in real time.

Use this to:
- Audit or log token usage
- Apply custom validation
- Configure downstream services dynamically
- Enforce limits or rate-based rules

> This trigger runs **server-side** on Kinde infrastructure. You don’t need to deploy or host anything yourself.

## Real-world use cases

Here are some examples of how teams are using this workflow trigger:

### 1. Track token usage by region
Log a metric to your analytics system based on metadata in the token (e.g. `region: eu`). Helps track activity across territories.

### 2. Enforce internal token policies
Prevent token generation if a certain property or flag is missing (e.g. `model_version` not assigned).

### 3. Apply dynamic configuration
Trigger a downstream webhook or set a cache flag when a token is generated, allowing dependent services to fetch relevant settings.

### 4. Alert on suspicious usage
Send an alert or log entry if a token is requested more than X times within a short period.

## Set up the M2M token generation workflow

The full setup process and code example is documented in the [Token generated workflow trigger](/workflows/example-workflows/m2m-token-generation-workflow/) page, and includes:

- Example JavaScript handler
- Event payload shape
- Deployment instructions
