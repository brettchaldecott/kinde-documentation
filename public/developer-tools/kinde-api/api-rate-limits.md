
Kinde may rate limit incoming traffic to help maximise API stability and prevent bursts of requests from destabilizing API functions.

If you send a lot of requests in quick succession, you might see error responses with **code `429`**.

For advice on handling these errors, see [Handle limiting gracefully](#handle-limiting-gracefully), below. If you suddenly see a rising number of rate-limited requests, contact Kinde support.

## API limiters

Kinde has several limiters in the API, including a rate limiter and a concurrency limiter.

### Rate limiter

The basic rate limiter restricts the number of API requests per minute as follows:

- maximum page size of 500 per request to API GET endpoints that use the `page_size` parameter, additional results can be requested using the `page_size` and `next_token` parameters (e.g. GET `/api/v1/subscribers`)
- maximum 100 objects can be updated in a single request when sending `POST/PATCH` requests to bulk endpoints (e.g. PATCH `/api/v1/organizations/{org_code}/users`)

If this affects your integrations and you require an extended period with a higher limit please get in touch.

## Common causes

Rate limiting can occur under a variety of conditions, but it’s most common in these scenarios:

- Running a large volume of closely-spaced requests can lead to rate limiting. Often this is part of an analytical or migration operation. When engaging in these activities, you should try to control the request rate on the client-side.
- Issuing many long-lived requests can trigger limiting. Requests vary in the amount of Kinde’s server resources they use, and more resource-intensive requests tend to take longer and run the risk of causing new requests to be shed by the concurrency limiter.
- Resource requirements vary, but list requests and requests that include expansions generally use more resources and take longer to run. We suggest profiling the duration of Kinde API requests, and watch for timeouts to try and spot those that are unexpectedly slow.
- A sudden increase in volume like the bulk addition of new users can result in rate limiting. We try to set our rates high enough that legitimate user traffic never exceeds the limits, but if you suspect that an upcoming event might push you over the limits listed above, contact us to increase limits for you.

## Handle limiting gracefully

Graceful handling involves monitoring for `429` status codes and triggering a retry method.  The header `RateLimit-Reset` will return the number of seconds until the rate limit is reset.

Your method should follow an exponential back-off schedule to reduce request volume as needed. We also recommend building randomness into the back-off schedule to avoid a ‘thundering herd’ effect.

Another method is to manage traffic at a global level, and throttle it back if you detect substantial rate limiting. A common technique for controlling rate is to implement something like a token bucket rate limiting algorithm on the client-side. Ready-made and mature implementations for token bucket can be found in most programming languages.
