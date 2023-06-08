# ICD API Authentication 

In order to be able to use the ICD APIs you need to follow these steps:

- Register to the ICD API site (https://icd.who.int/icdapi). 
  - Click on the **[Register]** link and follow the instructions
  - Once you register and login to the portal with your account, you may now click the **[View API access key]** to get your clientid and clientsecret. These are necessary for using the APIs.

	The system allows setting up multiple clients per user as well as removing clients. However, in a standard use, just having one client Id is sufficient.

  ICD APIs use OAUTH2 authentication with client credentials grant type which makes use of the Client Id and Client Secret for authentication. Below you will find the details on how to use them. You may also find more information on OAUTH 2.0 Authentication Framework [here](https://tools.ietf.org/html/rfc6749)

- The next step is getting an OAUTH 2.0 token from the server. 
You will make a request to the token endpoint with the following information.
  - Use Basic Authentication with your client Id and secret
  - grant_type = client_credentials
  - scope = icdapi_access
  
  **Our token endpoint:** https://icdaccessmanagement.who.int/connect/token

- Then the server returns back a token which you add to your future requests to the ICD API. Please note that the tokens are valid for about 1 hour and after that one has to get a new one.

Many programming languages have APIs that you could use to make the process easier. Below you will find and example in C# using .NET. You may find more examples in other programming languages at our [ICD API Samples GitHub repository](https://github.com/icd-api)



```csharp

using System.Net.Http;
using System.Net.Http.Headers;

//...

var tokenEndpoint = "https://icdaccessmanagement.who.int/connect/token";
var clientId = "..."; //of course not a good idea to put id and secret in the source code
var clientSecret = "..."; //you could read from an encyrpted source in the production
var scope = "icdapi_access";

tokenClient = new TokenClient(tokenEndpoint,
				clientId,
				clientSecret);

//We request the for an access token
tokenResponse = await tokenClient.RequestClientCredentialsAsync(scope);

var client = new HttpClient();
//Place the token at the httpclient that we are going to use at the future requests
client.SetBearerToken(tokenResponse.AccessToken);

//prepare a request to the API
request = new HttpRequestMessage(HttpMethod.Get, "https://id.who.int/icd/entity");

//put an accept header for application/json
request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
request.Headers.AcceptLanguage.Add(new StringWithQualityHeaderValue("en"));
request.Headers.Add("API-Version", "v2");

//call the API
var response = await client.SendAsync(request);
if (!response.IsSuccessStatusCode)
{
      Console.WriteLine(response.StatusCode);
}

var content = response.Content.ReadAsStringAsync().Result;
Console.WriteLine(content);

```
  