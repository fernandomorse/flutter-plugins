# login_with_amazon

A flutter plugin to use Amazon's native platform implementations of [Login With Amazon](https://developer.amazon.com/docs/login-with-amazon/minitoc-lwa-overview.html). Like the Amazon platform packages it will only ask to authorize if already logged in on the device (single sign on). Alternatively it will ask to login.

## Getting Started

The API key must be changed on a per platform basis in accordance with the following instructions.

- [Android](https://developer.amazon.com/docs/login-with-amazon/create-android-project.html#add-api-key)
  - in brief, after generating the API key on amazon, you must add it to your android project in the file app/src/main/assets/api_key.txt
  - you will likely need multiple keys, one for your debug application, and one for the live one as noted by amazon in the above documentation
- [iOS](https://developer.amazon.com/docs/login-with-amazon/register-ios.html#ios-bundle-id-and-api-keys)

- Alexa or Dash Device authorization code generation
  - [Get Authorization Code](https://developer.amazon.com/docs/dash/lwa-mobile-sdk.html#prerequisites)

### Methods

`import 'package:login_with_amazon/login_with_amazon.dart';`

- `LoginWithAmazon`
  - `Future<Map> login(Map<String, dynamic> scopes)`
  - `Future<void> logout()`
  - `Future<String> getAccessToken(Map<String, dynamic> scopes)`
  - `Future<Map> getAuthCode(String codeChallenge, String codeChallengeMethod, Map<String, dynamic> scopes)`

#### Scopes should match amazon scopes.

```
{
  'profile': null,
  'alexa:all': {
    'productID': 'SomeProductId',
    'productInstanceAttributes': {
      'deviceSerialNumber': 'serialNumberHere',
    },
  },
}
```

###### If a scope has no scopeData like the profile scope it should map to null.

`{'profile': null}`

###### Multiple scopes can be also be passed in all at once. _Do Not Do This_ amazon doesn't always work as expected

```
{
  'profile': null,
  'alexa:all': {
    'productID': 'SomeProductId',
    'productInstanceAttributes': {
      'deviceSerialNumber': 'serialNumberHere',
    },
  },
  'dash:replenish': {
    'device_model': 'SomeDeviceModel',
    'serial': 'serialNumberHere',
    'is_test_device': true
  },
}
```

#### Login Response

The response from using the login method is a map of the AuthorizeResult. It is dependent on the scope.

```
{
    'accessToken': 'access token result',
    'user': {
        'userEmail': 'user email result',
        'userId': 'user id result',
        'userName': 'user name result',
        'userPostalCode': 'user postal code result',
        'user info': 'user info as a Map<String,String> result',
    },
}
```

#### Authorization Code Response

The response from using the login method is a map of the AuthorizeResult. It is dependent on the scope.

```
{
    'authorizationCode': 'access token result',
    'clientId': 'amazon client Id',
    'redirectUri': 'amazon redirect Uri'
}
```

## Functionality

- [x] Sign on with varying scopes
- [x] Login With Amazon on Android
- [x] Single Sign On on Android
- [x] Get current Access Token on Android
- [x] Retrieve full amazon response
- [x] Simple setup with drop-in replacement of API key
- [x] Get LWA Authorization Code for Dash replenishment device
- [ ] Login With Amazon on iOS
- [ ] Get LWA Authorization Code for Dash replenishment device on iOS
- [ ] Logout on iOS
- [ ] Get current Access Token on iOS
