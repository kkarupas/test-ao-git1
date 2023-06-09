---
layout: page
title: Authentication
permalink: /activities/http-request/authentication
parent: HTTP Request
grand_parent: Activities
---

# Authentication
When working with HTTP-based APIs, sometimes the hardest part is figuring out how to authenticate properly. Some APIs are simple and use an `Authorization: Basic` header with a username and password while others use OAuth or Bearer tokens.

---

## Username and Password
The simplest type of authentication is a username and password. This method usually uses an `Authorization: Basic` header containing the username and password concatenated with a colon (`:`) and then `base64` encoded. Here's an example:
`Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l`.

The good news is that you don't have to worry about the encoding part, SecureX orchestration will do that for you. To use an API with a username and password, you simply need to:
* Create an **Account Key** using the **HTTP Basic Authentication** type with your username and password.
* Configure your **Target** to use this **Account Key**.
* Use the **Target** with the `HTTP Request` activity (and orchestration will handle the `Authorization` header for you).

---

## Bearer Token
Most modern APIs use some sort of bearer token/JWT for authentication. These are essentially self-contained, signed tokens that can be validated without needing to consult an identity server ([good site about JWTs](https://jwt.io)). In orchestration, we don't have an account key type for these because they're usually only valid for 10-30 minutes before you have to request a new one.

When using bearer tokens, you usually need to make an `HTTP Request` to get a token and then use the token for subsequent requests. In orchestration, we do this by having two targets and one account key.

### Target A - with Account Key
The first target uses an account key with the credentials necessary to request a bearer token. This is usually a **HTTP Basic Authentication** account key with a client ID for the username and client secret for the password. You can use the `HTTP Request` activity with this target to request a bearer token. The token response is commonly JSON that contains the token itself as well as how long the token is valid for.

This is what a token for Threat Response looks like:

```text
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpX...",
    "token_type": "bearer",
    "expires_in": 600,
    "scope": "integration private-intel admin profile enrich:read sse registry users casebook inspect:read orbital oauth response ui-settings telemetry:write notification global-intel:read"
}
```

To extract the token, you can simply use a [JSONPath Query]({{ site.baseurl }}/activities/jsonpath-query) activity with a path of: `$.access_token`

### Target B - No Account Key
Once you have your bearer token, you can make API requests without an account key as you'll be using the token for authentication. When using this target with the `HTTP Request` activity, you need to provide the bearer token in a custom header:

![]({{ site.baseurl }}/assets/images/activities/http-request/authorization-header.png)

Check out the [sample workflow](#sample-workflows) below for an example of this process with Threat Response.

---

## Static API Key or Token
If your API uses a static key or token that doesn't change, you can store it in a `Secure String` variable and add it to the `HTTP Request` using a custom header as explained [above](#target-b---no-account-key). This variable can be [global]({{ site.baseurl }}/variables/global) or contained within the [workflow]({{ site.baseurl }}/variables/workflow) itself.

---

## OAuth
Typically, authentication with OAuth-based APIs is a two step process:
1. Using a client ID and secret, fetch an access/bearer token from a token endpoint (ex: `https://cool-api.somewhere.com/oauth2/token`). The client ID is usually used as the username for HTTP Basic authentication and the client secret is used as the password.
2. Use that access/bearer token for all subsequent requests to the API (ex: `https://cool-api.somewhere.com/api/awesome-data`). The token obtained from step 1 is usually put in an `Authorization` header with the word `Bearer` (ex: `Authorization: Bearer my-long-token-here`).
3. Refresh the access/bearer token as needed (this will depend on how the specific API is implemented).

SecureX orchestration does not support following OAuth authorization/grant flows. If your API requires you to follow a web-based authorization flow to give an API client permission to a resource, you'll need to do that yourself and then use the resulting client ID/secret/tokens within orchestration. OAuth implementations vary, so we're only able to document some generic guidelines. Please refer to the individual product's API documentation for more information about how to authenticate properly.

---

## Sample Workflows
The following sample workflows are available in our repository's workflows folder to help you get familiar with this activity. These can be imported using the instructions [here]({{ site.baseurl }}/importing) or you can view the workflow in GitHub by clicking on it.

* [Sample - HTTP Requests - Bearer Token]({{ site.github.repository_url }}/tree/Main/Workflows/Sample-HTTPRequests-BearerToken__definition_workflow_01KKTCSNCPMF56KPE3vS4dM3rG2bwHEqBQP)
