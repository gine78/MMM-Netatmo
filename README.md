[![Build Status](https://travis-ci.org/CFenner/MMM-Netatmo.svg?branch=master)](https://travis-ci.org/CFenner/MMM-Netatmo)
[![Known Vulnerabilities](https://snyk.io/test/github/cfenner/magicmirror-netatmo-module/badge.svg)](https://snyk.io/test/github/cfenner/magicmirror-netatmo-module)
[![code climate](https://codeclimate.com/github/CFenner/MMM-Netatmo/badges/gpa.svg)](https://codeclimate.com/github/CFenner/MMM-Netatmo)
[![api](https://img.shields.io/badge/api-Netatmo-orange.svg)](https://dev.netatmo.com/doc)
[![License](https://img.shields.io/github/license/mashape/apistatus.svg)](https://choosealicense.com/licenses/mit/)

# MagicMirror-Netatmo-Module

A module to integrale informations from a Netatmo weather station into the [MagicMirror](https://github.com/MichMich/MagicMirror).

![Netatmo visualisation](https://github.com/RaymondMolenaar/MMM-Netatmo/master/MMM-Netatmo.png)

## Usage

_Prerequisites_

- requires MagicMirror v2.0.0
- a Netatmo weather station at home or at least access to a Netatmo weather station account

To use this module, just clone this repository to your __modules__ folder of your MagicMirror: `git clone https://github.com/RaymondMolenaar/MMM-Netatmo.git MMM-Netatmo`

Now just add the module to your config.js file ([config entries](#configuration)).

### Access Your Data

To be able to access your data, you need to have an Netatmo Application and grant this application access to your data.

#### Register an App

Your can register a new app [here](https://dev.netatmo.com/dev/createapp). Afterwards you will get an APP_ID and an APP_SECRET which you will need to enter in the [config entries](#configuration).

#### Grant Access to Your Data

To allow the app to access your data, you need to send a POST request to the auth server and register the app.

##### cURL

One option is to use the command line tool [cURL](https://www.google.de/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwjqgN789KnaAhUBalAKHR-NDLoQFgg2MAE&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FCURL&usg=AOvVaw27-lfQBHvLQPR2qsddIR6U). 

```
curl --data "grant_type=password&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET&username=YOUR_NETATMO_USERNAME&password=YOUR_NETATMO_PASSWORD&scope=read_station" "https://api.netatmo.com/oauth2/token"
```

The POST request will return the following data:

```
{"access_token":"abc","refresh_token":"xyz","scope":["read_station"],"expires_in":10800,"expire_in":10800}
```

The REFRESH_TOKEN will be needed in the [config entries](#configuration).

##### Hurl.it

You can also send a POST request with [Hurl.it](https://www.hurl.it)([Git](https://github.com/defunkt/hurl)) to the Netatmo auth url: https://api.netatmo.com/oauth2/token

Also you need to provide the following data (add as parameters):

- grant_type: password
- client_id: [APP_ID]
- client_secret: [APP_SECRET]
- username: [USER_MAIL]
- password: [USER_PASSWORD]
- scope: read_station

The POST request will return the following data:

- access_token: [ACCESS_TOKEN]
- expires_in: 10800
- refresh_token: [REFRESH_TOKEN]

The REFRESH_TOKEN will be needed in the [config entries](#configuration).

### Configuration

The module needs the default configuration block in your config.js to work.

```
{
	module: 'netatmo',
	position: 'bottom_left', // the location where the module should be displayed
	config: {
		clientId: '', // your app id
		clientSecret: '', // your app secret
		refreshToken: '' // your generated refresh token
	}
}
```

The following properties can be configured:

|Option|Description|
|---|---|
|clientId|The ID of your Netatmo [application](https://dev.netatmo.com/dev/listapps).<br><br>This value is **REQUIRED**|
|clientSecret|The app secret of your Netatmo [application](https://dev.netatmo.com/dev/listapps).<br><br>This value is **REQUIRED**|
|refreshToken|The generated refresh token you got from the POST request to the auth api.<br><br>This value is **REQUIRED**|
|refreshInterval|How often does the content needs to be updated? (Minutes)<br>Data is updated by netatmo every 10 minutes.<br><br>**Default value:** `3`|
|moduleOrder|The rendering order of your weather modules, ommit a module to hide the output.<br><br>**Example:** `["Kitchen","Kid's Bedroom","Garage","Garden"]` <br>Be aware that you need to use the module names that you set in the netatmo configuration.|
|dataOrder|The rendering order of the data types of a module, ommit a data type to hide the output.<br><br>**Example:** `["Noise","Pressure","CO2","Humidity","Temperature","Rain"]`|
|showBattery| Show batterystatus<br><br>**Default value:** `true`|
|showRadio| Show Signal status <br><br>**Default value:** `true`|
|showWiFi| Show WIFI Signal  <br><br>**Default value:** `true`|
|showMinMaxTemp| Show Minimum and Maximum Temperatue of stations<br><br>**Default value:** `true`|
|showTrend| Way the temperature or pressure trend is diplayed. It can be not at all [None], IconStyle [Icon] and or [text]. <br><br>**Example value:** `["Icon", "Text"]`<br><br>**Default value:** `["Icon"]`|
|showReachable| Show station reachable <br><br>**Default value:** `true`|

## About this Fork
The most important change of this fork, is that even when not all of your Netatmo Modules are in reach (due to empty batteries or whatever), this MM-Module will still display your module, but with empty values. It doesn't hang on the loading circle.