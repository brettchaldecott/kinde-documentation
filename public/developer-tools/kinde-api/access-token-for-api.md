
Before you can get an access token and call the Kinde Management API, follow the steps to [create and authorize a machine-to-machine (M2M) application, with scopes](/developer-tools/kinde-api/connect-to-kinde-api/).

## Get access token

There are two main methods for getting an access token for the Kinde Management API.

### Method 1: Get a test access token in the Kinde Admin

1. Open the M2M application you created for API access. 
2. Go to **Test**.
3. Select **Get token**.
4. Select **Use test token**. You'll be taken to the API docs with the access token prepopulated. You can now test any endpoint.

For full details, see [generate a test access token for the Kinde Management API](/developer-tools/kinde-api/kinde-api-test-token/).

### Method 2: Perform a POST request to get an access token

To ask Kinde for an access token for calling the management API, perform a POST request to the `https://<your_subdomain>.kinde.com/oauth2/token` endpoint, using the credentials of the M2M application you created in the prerequisite step.

The payload should look as follows:

<Tabs>

  <TabItem label="cURL">

```shellscript
curl --request POST \
  --url 'https://<your_subdomain>.kinde.com/oauth2/token' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data grant_type=client_credentials \
  --data 'client_id=<your_m2m_client_id>' \
  --data 'client_secret=<your_m2m_client_secret>' \
  --data 'audience=https://<your_subdomain>.kinde.com/api'
```

  </TabItem>

  <TabItem label="C#">

```c#
var client = new RestClient("https://<your_subdomain>.kinde.com/oauth2/token");
var request = new RestRequest(Method.POST);
request.AddHeader("content-type", "application/x-www-form-urlencoded");
request.AddParameter("application/x-www-form-urlencoded", "grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi", ParameterType.RequestBody);
IRestResponse response = client.Execute(request);
```

  </TabItem>

  <TabItem label="Go">

```go
package main

import (
	"fmt
	"strings
	"net/http
	"io/ioutil
)

func main() {

	url := "https://<your_subdomain>.kinde.com/oauth2/token

	payload := strings.NewReader("grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/x-www-form-urlencoded")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

  </TabItem>

  <TabItem label="Java">

```java
HttpResponse<String> response = Unirest.post("https://<your_subdomain>.kinde.com/oauth2/token")
  .header("content-type", "application/x-www-form-urlencoded")
  .body("grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi")
  .asString();
```

  </TabItem>

  <TabItem label="Node.js">

```js
async function getToken() {
  try {
    const response = await fetch(`https://<your_subdomain>.kinde.com/oauth2/token`, {
      method: "POST",
      headers: {
        "content-type": "application/x-www-form-urlencoded"
      },
      body: new URLSearchParams({
        audience: "https://<your_subdomain>.kinde.com/api",
        grant_type: "client_credentials",
        client_id: "<your_m2m_client_id>",
        client_secret: "<your_m2m_client_secret>"
      })
    });

    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error.message);
  }
}

getToken();
```

  </TabItem>

  <TabItem label="Obj-C">

```objc
#import <Foundation/Foundation.h>

NSDictionary *headers = @{ @"content-type": @"application/x-www-form-urlencoded" };

NSMutableData *postData = [[NSMutableData alloc] initWithData:[@"grant_type=client_credentials" dataUsingEncoding:NSUTF8StringEncoding]];
[postData appendData:[@"&client_id=<your_m2m_client_id>" dataUsingEncoding:NSUTF8StringEncoding]];
[postData appendData:[@"&client_secret=<your_m2m_client_secret>" dataUsingEncoding:NSUTF8StringEncoding]];
[postData appendData:[@"&audience=https://<your_subdomain>.kinde.com/api" dataUsingEncoding:NSUTF8StringEncoding]];

NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://<your_subdomain>.kinde.com/oauth2/token"]
                                                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                   timeoutInterval:10.0];
[request setHTTPMethod:@"POST"];
[request setAllHTTPHeaderFields:headers];
[request setHTTPBody:postData];

NSURLSession *session = [NSURLSession sharedSession];
NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                if (error) {
                                                    NSLog(@"%@", error);
                                                } else {
                                                    NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                    NSLog(@"%@", httpResponse);
                                                }
                                            }];
[dataTask resume];
```

  </TabItem>

    <TabItem label="PHP">

```php
$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://<your_subdomain>.kinde.com/oauth2/token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi",
  CURLOPT_HTTPHEADER => [
    "content-type: application/x-www-form-urlencoded
  ],
]);

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

    </TabItem>

    <TabItem label="Python">

```python
import http.client

conn = http.client.HTTPSConnection("")

payload = "grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi

headers = { 'content-type': "application/x-www-form-urlencoded" }

conn.request("POST", "https://<your_subdomain>.kinde.com/oauth2/token", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

    </TabItem>

     <TabItem label="Ruby">

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://<your_subdomain>.kinde.com/oauth2/token")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/x-www-form-urlencoded'
request.body = "grant_type=client_credentials&client_id=<your_m2m_client_id>&client_secret=<your_m2m_client_secret>&audience=https%3A%2F%2F<your_subdomain>.kinde.com%2Fapi

response = http.request(request)
puts response.read_body
```

    </TabItem>

    <TabItem label="Swift">

```swift
import Foundation

let headers = ["content-type": "application/x-www-form-urlencoded"]

let postData = NSMutableData(data: "grant_type=client_credentials".data(using: String.Encoding.utf8)!)
postData.append("&client_id=<your_m2m_client_id>".data(using: String.Encoding.utf8)!)
postData.append("&client_secret=<your_m2m_client_secret>".data(using: String.Encoding.utf8)!)
postData.append("&audience=https://<your_subdomain>.kinde.com/api".data(using: String.Encoding.utf8)!)

let request = NSMutableURLRequest(url: NSURL(string: "https://<your_subdomain>.kinde.com/oauth2/token")! as URL,
                                        cachePolicy: .useProtocolCachePolicy,
                                    timeoutInterval: 10.0)
request.httpMethod = "POST
request.allHTTPHeaderFields = headers
request.httpBody = postData as Data

let session = URLSession.shared
let dataTask = session.dataTask(with: request as URLRequest, completionHandler: { (data, response, error) -> Void in
  if (error != nil) {
    print(error)
  } else {
    let httpResponse = response as? HTTPURLResponse
    print(httpResponse)
  }
})

dataTask.resume()
```

    </TabItem>

</Tabs>

Make sure to replace `<your_subdomain>`, `<your_m2m_client_id>` and `<your_m2m_client_secret>` with your own details.

The response will contain a signed JWT containing claims including the scopes the token is allowed to access and the expiry time. Here is an example token:

```json

{
  "aud": [
    "https://example.kinde.com/api
  ],
  "azp": "bd69bb9fe5db44a38b6b2dacd1f4b451",
  "exp": 1729812040,
  "gty": [
    "client_credentials
  ],
  "iat": 1729725640,
  "iss": "https://example.kinde.com",
  "jti": "6f091ebe-44ba-4afc-bd2f-05fcccafc89e",
  "scope": "read:users update:users"
}

```

## Use the access token

To use this token, include it in the Authorization header of your request. For example to get all users you would call:

<Tabs>

  <TabItem label="cURL">

```shellscript
curl --request GET \
  --url 'https://<your_subdomain>.kinde.com/api/v1/users' \
  --header 'authorization: Bearer <m2m_access_token>' \
  --header 'content-type: application/json'
```

  </TabItem>

  <TabItem label="C#">

```c#
var client = new RestClient("https://<your_subdomain>.kinde.com/api/v1/users");
var request = new RestRequest(Method.GET);
request.AddHeader("content-type", "application/json");
request.AddHeader("authorization", "Bearer <m2m_access_token>");
IRestResponse response = client.Execute(request);
```

  </TabItem>

  <TabItem label="Go">

```go
package main

import (
	"fmt
	"net/http
	"io/ioutil
)

func main() {

	url := "https://<your_subdomain>.kinde.com/api/v1/users

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("authorization", "Bearer <m2m_access_token>")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

  </TabItem>

  <TabItem label="Java">

```java
HttpResponse<String> response = Unirest.get("https://<your_subdomain>.kinde.com/api/v1/users")
  .header("content-type", "application/json")
  .header("authorization", "Bearer <m2m_access_token>")
  .asString();
```

  </TabItem>

  <TabItem label="Node.js">

```js
async function getUsers() {
  try {
    const response = await fetch(`https://<your_subdomain>.kinde.com/api/v1/users`, {
      method: "GET",
      headers: {
        "content-type": "application/json",
        authorization: "Bearer <m2m_access_token>"
      }
    });

    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error.message);
  }
}

getUsers();
```

  </TabItem>

  <TabItem label="Obj-C">

```objc
#import <Foundation/Foundation.h>

NSDictionary *headers = @{ @"content-type": @"application/json",
                           @"authorization": @"Bearer <m2m_access_token>" };

NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://<your_subdomain>.kinde.com/api/v1/users"]
                                                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                   timeoutInterval:10.0];
[request setHTTPMethod:@"GET"];
[request setAllHTTPHeaderFields:headers];

NSURLSession *session = [NSURLSession sharedSession];
NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                if (error) {
                                                    NSLog(@"%@", error);
                                                } else {
                                                    NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                    NSLog(@"%@", httpResponse);
                                                }
                                            }];
[dataTask resume];
```

  </TabItem>

    <TabItem label="PHP">

```php
$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://<your_subdomain>.kinde.com/api/v1/users",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "authorization: Bearer <m2m_access_token>",
    "content-type: application/json
  ],
]);

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

    </TabItem>

    <TabItem label="Python">

```python
import http.client

conn = http.client.HTTPSConnection("")

headers = {
    'content-type': "application/json",
    'authorization': "Bearer <m2m_access_token>"
    }

conn.request("GET", "https://<your_subdomain>.kinde.com/api/v1/users", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

    </TabItem>

     <TabItem label="Ruby">

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://<your_subdomain>.kinde.com/api/v1/users")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["content-type"] = 'application/json'
request["authorization"] = 'Bearer <m2m_access_token>'

response = http.request(request)
puts response.read_body
```

    </TabItem>

    <TabItem label="Swift">

```swift
import Foundation

let headers = [
  "content-type": "application/json",
  "authorization": "Bearer <m2m_access_token>"
]

let request = NSMutableURLRequest(url: NSURL(string: "https://<your_subdomain>.kinde.com/api/v1/users")! as URL,
                                        cachePolicy: .useProtocolCachePolicy,
                                    timeoutInterval: 10.0)
request.httpMethod = "GET
request.allHTTPHeaderFields = headers

let session = URLSession.shared
let dataTask = session.dataTask(with: request as URLRequest, completionHandler: { (data, response, error) -> Void in
  if (error != nil) {
    print(error)
  } else {
    let httpResponse = response as? HTTPURLResponse
    print(httpResponse)
  }
})

dataTask.resume()
```

    </TabItem>

</Tabs>

Make sure to replace `<your_subdomain>` with your Kinde subdomain and `<m2m_access_token>` with the token received in the previous step.

## Use the Kinde Management API JS SDK

As an alternative to making HTTP calls, you can also use the [Kinde Management API JS](https://github.com/kinde-oss/management-api-js/) SDK.

You can use it to automatically obtain tokens for, and interact with the Kinde management API.

## Alternative - using Postman guide

### Set up Postman environment

We recommend you do this in a non-production environment first.

If you decide to use Postman, we recommend that you set up a Postman environment. Here's some [troubleshooting solutions](/developer-tools/kinde-api/troubleshoot-kinde-api/) in case you need them.

1. Add your Kinde machine to machine application keys as environment variables.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/69800dc4-2d22-468c-4300-71e4b4ee8b00/public"
  alt="Adding environment variables in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>
2. Make sure you select `Save` or the variables will not persist.

### Get the access token

1. Go to **Collections**. Create a new collection called **Kinde**.
2. In the three dots menu next to the new **Kinde** folder, select **Add request**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/ececdf0a-452f-49bc-6a96-f5343d8a8d00/public"
  alt="Adding a request in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

3. Go to the **Authorization** section and set the **Type** to **OAuth 2.0** and set the **Header Prefix** to **Bearer**.
4. In the **Configure New Token > Configuration options** section, set the **Grant Type** to **Client Credentials**.
5. Enter the **Access Token URL** using the domain variable you created above. For example, `{{business_domain}}/oauth2/token`. Note that even if you use a custom domain, the access token URL should still use your `https://<your_subdomain>.kinde.com` domain.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8149baf6-e3b7-406d-7447-390fe4bc2100/public"
  alt="Entering the access token URL"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

6. Enter the **Client ID** and **Client Secret** using the environment variables you created earlier or by copying them from the Kinde application.
7. Set the **audience** to `{{business_domain}}/api`. To do this:
   - Scroll down click **Advanced**. In the **Token request** section, select the `audience` key and enter the above URL in the **Value** field. Ensure it is being sent in the `body` of the request

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/f2c9f0dc-ef24-40e4-dae9-2d1fd6731b00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

8. Go to the **Headers** tab.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/92102061-66fb-48f8-f94a-f048b80a3f00/public"
  alt="Setting the Content-Type value in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

9. Select **Accept** and ensure the value is `application/json`.
10. In the **Authorization** section, select **Get New Access Token**. You should see a confirmation message.
11. Select **Proceed**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/b736bc91-3f30-4d48-62c6-37f131e88300/public"
  alt="Access Token in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

12. Select **Use Token**. You should now have the access token for making requests to the Kinde management API. See the [Kinde API documentation](/kinde-apis/management/) for all the available end points.

### Test the API endpoints

You can test your API access in Postman by sending a `GET` request to any Kinde API endpoint. See the [Kinde Management API library](/kinde-apis/management/) for options.

Here’s an example using the `Get users` endpoint.

1. Create a new `GET` request.
2. Enter a URL that contains the `/users` endpoint, e.g. `https://<your_subdomain>.kinde.com/api/v1/users` .

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/44dd0ac2-b96e-4a91-79dd-4a9f9dda2000/public"
  alt="Entering a Request URL in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

3. Send the request. It should return your users in the body section. If it does, the connection is successful.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8f4aae17-6f38-43e4-9bf7-55ac0bf2d300/public"
  alt="Response body in Postman"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

4. Repeat from step 1 for any other [Kinde API endpoints](/kinde-apis/management/) you want to test.
