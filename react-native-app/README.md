# React Native Auth Kit

This is a React Native Auth Boilerplate using [Expo](https://docs.expo.io) that is equipped with the following authentication methods:

- Username/Password
- Email
- Mobile with OTP
- Google
- Facebook

> If you do not want to use the Expo SDK, [click here](https://github.com/hasura/react-native-auth-boilerplate/blob/master/expo).

## Contents

- [Getting started](#getting-started)
- [Adding your own source code](#adding-your-own-source-code)
  - [Logging out](#logging-out)
  - [Getting user info](#getting-user-information)
- [Login Methods](#login-methods)
  - [Username/Password](#username)
  - [Email](#email)
  - [Mobile-OTP](#mobile-otp)
  - [Google](#google)
  - [Facebook](#facebook)

## Getting started

### With Hasura APIs

**Note**: You need to install [hasura CLI](https://docs.hasura.io/0.15/manual/install-hasura-cli.html) to use the Hasura APIs

1. Clone the repo and cd into the expo directory. This will be referred to as the root directory from now on.

```bash
$ git clone https://github.com/hasura/react-native-auth-boilerplate && cd react-native-auth-boilerplate/expo
```

2. Quickstart the base Hasura project and apply the configurations of the project to the newly created [Hasura cluster]. Run the following commands.

> The `hasura quickstart` command clones a base Hasura project with basic configuration and creates a free tier [Hasura cluster](https://docs.hasura.io/0.15/manual/cluster/index.html). Running a `git push` to the `hasura` remote applies the configuration from the project (here base) to the cluster.

```bash
$ hasura quickstart base
$ cd base
$ git add . && git commit -m "Applying configuration"
$ git push hasura master
```

> The above commands need not be run from the same directory. Run these commands wherever you want to keep your backend configuration.

3. You will obtain a cluster name on running the `hasura quickstart` command. Copy this cluster name. (You can also run `hasura cluster status` from the base directory to know your cluster name)

4. Modify the clusterName in `Hasura.js`.

```javascript
const clusterName = "clusterName45";
```

5. [Run the app](#running-the-app)

(By default, only "login with username" is enabled. To configure more login methods, go to the [individual sections](#username))

### Without Hasura APIs

Clone the repo and `cd` into the expo directory. This will be referred to as the root directory from now on.

```bash
$ git clone https://github.com/hasura/react-native-auth-boilerplate && cd react-native-auth-boilerplate/expo
```

Check the following sections to see how to configure the login methods without using Hasura APIs.

- [Username/Password](#username)
- [Email](#email)
- [Mobile OTP](#mobile-otp)
- [Google](#google)
- [Facebook](#facebook)

After configuring, simply [run the app](#running-the-app).

## Adding your own source code

After the authentication is complete, the following screen is rendered.

![success](https://raw.githubusercontent.com/hasura/react-native-auth-boilerplate/master/readme-assets/ios/iossuccess.jpeg)

It is present at `src/AppScreen.js`. You can modify it as you like.

### Logging out

The screen mentioned above receives the logout function as a prop called ``logoutCallback``. Execute that function to make a user logout. Example of a logout button is:

```javascript
<Button title="Logout" onPress={this.props.logoutCallback} />
```

### Getting user information

Your screen also receives a prop called `sessionInfo` which is an object like so

```javascript
sessionInfo = {
  id: <a unique user id>,
  token: "<session_token",
  type: "mobile",
  ...
  ...
}
```

> The `sessionInfo` object differs a bit for every login method. So it will be helpful to log it once so you can handle it as desired.


## Running the app

1. Install node modules

```
$ npm install
```

2. To run locally and generate a QR code in your terminal, run

```
$ npm start
```

3. To build stand-alone APKs and IPAs, run

**For Android**: `exp build:android`
**For iOS**: `exp build:ios`

Run `npm install -g exp` if you do not have `exp` installed.

## Login methods

### Username

#### With Hasura APIs

It is already configured. Just [create and configure the cluster](#getting-started) and it'll work.

#### Without Hasura APIs

1. Open `Hasura.js`
2. Set `useHasuraApis` to `false` and set the `username` field in `config` to `true`

```javascript
const clusterName = "clusterName45";
const useHasuraApis = false;

const config = {
  "username":true,
  "email":false,
  "mobileOtp":false,
  "googleArray": null,
  "facebook": null
};
```

3. Implement your own custom `tryLogin` and `trySignup` functions in ``auth/components/username/actions.js``.

4. Look at ``auth/components/username/Signup`` and ``auth/components/username/Login`` to see how the responses are handled.

### Email

#### With Hasura APIs

Note: This section assumes that you have already [created a Hasura cluster](#getting-started)

1. Run the following commands from the `base` directory:

```bash
# Get the user info
$ hasura user-info

# Copy the Token from the output of above command and run this
$ hasura secret update notify.hasura.token <Token>
```

2. Open ``conf/auth.yaml`` from the `base` directory and enable email provider by changing `defaultProviders > email > enabled` to true. (Don't change anything else unless you are sure about it).

```yaml
defaultProviders:
  email:
    enabled: "true"
    defaultRoles: []
```

3. Apply the changes you made by running a `git push` from `base` directory.

```
$ git add .
$ git commit -m "Made changes to conf"
$ git push hasura master
```

#### Without Hasura APIs

1. Open `Hasura.js`
2. Set `useHasuraApis` to `false` and set the `email` field in `config` to `true`

```javascript
const clusterName = "clusterName45";
const useHasuraApis = false;

const config = {
  "username":true,
  "email":true,
  "mobileOtp":false,
  "googleArray": null,
  "facebook": null
};
```

3. Implement your own custom `tryEmailLogin` and `tryEmailSignup`, `forgotPassword` and `resendVerification` functions in ``auth/components/email/actions.js``.

4. Look at ``auth/components/email/Signup`` and ``auth/components/email/Login`` to see how the responses are handled.

### Mobile OTP

#### With Hasura APIs

Note: This section assumes that you have already [created a Hasura cluster](#getting-started)

1. Run the following commands from the `base` directory:

```bash
# Get the user info
$ hasura user-info

# Copy the Token from the output of above command and run this
$ hasura secret update notify.hasura.token <Token>
```

2. Open ``conf/auth.yaml`` from the `base` directory and enable email provider by changing `defaultProviders > mobileOtp > enabled` to true. (Don't change anything else unless you are sure about it).

```yaml
defaultProviders:
  mobile:
    enabled: "true"
    defaultRoles: []
```

3. Apply the changes you made by running the following command from `base` directory.

```
$ git add .
$ git commit -m "Made changes to conf"
$ git push hasura master
```

#### Without Hasura APIs

1. Open `Hasura.js`
2. Set `useHasuraApis` to `false` and set the `mobileOtp` field in `config` to `true`

```javascript
const clusterName = "clusterName45";
const useHasuraApis = false;

const config = {
  "username":true,
  "email":true,
  "mobileOtp": true,
  "googleArray": null,
  "facebook": null
};
```

3. Implement your own custom `sendOtp` and `verifyOtp` functions in ``auth/components/otp/actions.js``.

4. Look at ``auth/components/otp/SendOtp`` and ``auth/components/otp/Verify`` to see how the responses are handled.


### Google

#### With Hasura APIs

Note: This section assumes that you have already [created a Hasura cluster](#getting-started)

1. Obtain Google OAuth Client ID by [following the instructions here](https://docs.expo.io/versions/latest/sdk/google.html).

2. Open ``conf/auth.yaml`` from the `base` directory and enable google login method by setting `defaultProviders > google > enabled` to `true`. (Don't change anything else unless you are sure about it).

```yaml
defaultProviders:
  google:
    enabled: "true"
    defaultRoles: []
```

3. Also enter your `android client ID` and `iOS client ID` as second and third elements of the `google > cliendIds` array respectively.

```yaml
google:
  clientIds: ["webClientId", "androidClientId", "iOSClientId"]
```

4. Apply the configuration to the cluster

```
$ git add .
$ git commit -m "Made changes to the conf"
$ git push hasura master
```

#### Without Hasura APIs

1. Open `Hasura.js`.

2. Set `useHasuraApis` to `false`. Enter your `android client ID` and `iOS client ID` as second and third elements of `config.googleArray` array respectively.

```javascript
const clusterName = "clusterName45";
const useHasuraApis = false;

const config = {
  "username":true,
  "email":true,
  "mobileOtp": true,
  "googleArray": ["webClientId", "androidClientId", "iosClientId"],
  "facebook": null
};
```

3. Implement your own implementation of handling the "auth_token" in the `tryGoogleLogin` function in ``auth/components/google/actions.js``.

### Facebook

#### With Hasura APIs

Note: This section assumes that you have already [created a Hasura cluster](#getting-started)

1. Obtain Facebook App ID by [following the instructions here](https://docs.expo.io/versions/latest/sdk/facebook.html).

2. Open ``conf/auth.yaml`` from the `base` directory and enable facebook login method by setting `defaultProviders > facebook > enabled` to `true`. (Don't change anything else unless you are sure about it).

```yaml
defaultProviders:
  facebook:
    enabled: "true"
    defaultRoles: []
```

3. Also enter your `appId` in the field `facebook > cliendId`.

```yaml
facebook:
  clientId: "appId"
```

4. Run the following command from the `base` directory to add the project to the secrets.

```
$ hasura secret update auth.facebook.client_secret <app_secret>
```

4. Apply the changes to the cluster by running these commands from the `base`

```
$ git add .
$ git commit -m "Made changes to conf"
$ git push hasura master
```

#### Without Hasura APIs

1. Open `Hasura.js`.

2. Set `useHasuraApis` to `false`. Enter your `appID` in the `config.facebook` field.

```javascript
const clusterName = "clusterName45";
const useHasuraApis = false;

const config = {
  "username":true,
  "email":true,
  "mobileOtp": true,
  "googleArray": null,
  "facebook": "facebookAppId"
};
```

3. Implement your own implementation of handling the "auth_token" in the `tryFacebookLogin` function in ``auth/components/facebook/actions.js``.
