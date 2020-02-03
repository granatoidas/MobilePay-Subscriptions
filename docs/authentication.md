## <a name="general-notes_authentication"></a>Authentication 

### <a name="openid-connect"></a>OpenID Connect
When the merchant is onboarded, he has a user in MobilePay that is able to manage which products the merchant wishes to use. 

[![](assets/images/OpenIdflowWithFIandAuthorize.png)](assets/images/OpenIdflowWithFIandAuthorize.png)

      
**The flow:**

In short - The flow is described in the following 4 steps:

[Step 1: Call /connect/authorize to initiate user login and consent](https://developer.mobilepay.dk/developersupport/openid/authorize/) 

[Step 2: Wait for the response by listening on the redirect URI and get the authorization code](https://developer.mobilepay.dk/developersupport/openid/getcode/) 

[Step 3: Exchange the authorization code for tokens using /connect/token](https://developer.mobilepay.dk/developersupport/openid/gettokens/) 

[Step 4: Keep the session alive by using the refresh token](https://developer.mobilepay.dk/developersupport/openid/getrefreshtokens/) 

[Step 5: Follow Best Practice](https://developer.mobilepay.dk/developersupport/openid/bestpractice/) 


The merchant must grant consent to an application(__Client__). The client is the application that is attempting to get access to the user's account.  The client needs to get consent from the user before it can do so. This consent is granted through mechanism in the [OpenID Connect](http://openid.net/connect/) protocol suite. <br />
Integrators and merchants are the same as __Clients__ in the OAuth 2.0 protocol. The __Client__ must initiate the [hybrid flow](http://openid.net/specs/openid-connect-core-1_0.html#HybridFlowAuth) specified in OpenID connect. For __Subscriptions__ product the __Client__ must request consent from the merchant using the `subscriptions` scope. You also need to specify `offline_access` scope, in order to get the refresh token. The authorization server in sandbox is located [here](https://api.sandbox.mobilepay.dk/merchant-authentication-openidconnect).<br />
If the merchant grants consent, an authorization code is returned which the __Client__ must exchange for an id token, an access token and a refresh token. The refresh token is used to refresh ended sessions without asking for merchant consent again. This means that if the __Client__ receives an answer from the api gateway saying that the access token is invalid, the refresh token is exchanged for a new access token and refresh token. <br /> <br />
An example of how to use OpenID connect in C# can be found [here](https://github.com/MobilePayDev/MobilePay-Invoice/tree/master/ClientExamples).

When user clicks on this button, merchant must do back-end call to   
[`"/authorize"`](https://developer.mobilepay.dk/developersupport/openid/authorize/) endpoint for initiating  authentication flow. You need to wait for the response by listening on the redirect URI and get the Authorization Code. Our system will re-direct the merchant back to your system also using the redirect URL. 
 

### OpenID configuration endpoints 
Find the configuration links below:

|Environment | Links |
|------------|-------|
|Sandbox    | Denmark <a href="https://sandprod-admin.mobilepay.dk/account/.well-known/openid-configuration">https://sandprod-admin.mobilepay.dk/account/.well-known/openid-configuration</a> <br> Finland <a href="https://sandprod-admin.mobilepay.fi/account/.well-known/openid-configuration">https://sandprod-admin.mobilepay.fi/account/.well-known/openid-configuration</a> |
|Production  | Denmark <a href="https://admin.mobilepay.dk/account/.well-known/openid-configuration">https://admin.mobilepay.dk/account/.well-known/openid-configuration</a> <br> Finland <a href="https://admin.mobilepay.fi/account/.well-known/openid-configuration">https://admin.mobilepay.fi/account/.well-known/openid-configuration</a>|


![](assets/images/OpenIdFlowWithFiandAuthorize.png)
      
### QuickStart: follow our QuickStart to start building your integration

- More information about integration steps are  [here](https://developer.mobilepay.dk/subscriptions-main)
- Pick an OpenID Connect library: we recommend <a href="https://github.com/IdentityModel/IdentityModel.OidcClient2">Certified C#/NetStandard OpenID Connect Client Library for native mobile/desktop Applications</a> 
- FAQ's for OpenID Connect <a href="https://developer.mobilepay.dk/faq/oidc">here</a>
- Integration is based on common standard OpenID Connect. You can find more [here](https://developer.mobilepay.dk/developersupport/openid/). 
- Video tutorial [here](https://developer.mobilepay.dk/developersupport/openid/tutorial)

### <a name="openid-connect-libraries"></a>Implementing OpenID Connect protocol
There are many OpenID Connect certified libraries for different platforms, so you just have to chose the one, that suits you best [from this list](http://openid.net/developers/certified/#RPLibs).
To be able to use and connect to the API there are few requirements. In order to authenticate to the API, all requests to the API must contain at least three authentication headers:
1. `x-ibm-client-id`
2. `x-ibm-client-secret`  
3. `Authorization` 

Creating an app in MobilePay Developer Portal will create a `x-ibm-client-id` and `x-ibm-client-secret` that should be used in all calls to the MobilePay Subscriptions API  


```console
$ curl --header "Authorization: Bearer <token>" --header 'x-ibm-client-id: client-id' --header 'x-ibm-client-secret: client-secret' --url https://<mobile-pay-root>/api/merchants/me/resource
```
[![](assets/images/Preview-MP-logo-and-type-horizontal-blue.png)](assets/images/Preview-MP-logo-and-type-horizontal-blue.png)