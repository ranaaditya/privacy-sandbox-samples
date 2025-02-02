## Overview

The FLEDGE sample app demonstrates how to initialize and call the Custom Audience 
and Ad Selection APIs in FLEDGE.

The sample app has two client classes, `AdSelectionClient` and
`CustomAudienceClient` that interact with the two FLEDGE APIs. CustomAudienceClient
wraps the API calls that allow apps to join and leave custom audiences (CAs) while
`AdSelectionClient` wraps the functionality that allows apps to run ad auctions
and report impressions based on the results of those auctions.

## About the FLEDGE API

For a full overview of how FLEDGE works, read the [design proposal]. Review the 
[developer guide] for details on API usage and systems integration.

## Set up development environment

To test functionality of the FLEDGE API, you need to [set up your development
environment].

## Set up bidding code and impression reporting endpoints

In order for ad selection components of the app to work, you must follow the 
directions for a setting up a server located in the [FledgeServerSpec]
folder.

## Launching the app 
First install the app on your device by running 
```shell
./gradlew installDebug
```
Once the app is installed on your device, you must launch it with the command
```shell
adb shell am start -n com.example.adservices.samples.fledge.sampleapp/.MainActivity -e biddingUrl [bidding endpoint] -e scoringUrl [scoring endpoint]
```
in order to inform the app where your server endpoints are running.

## Manage custom audiences and run ad selection

To view the end-to-end functionality of FLEDGE, you can use the four buttons at
the bottom of the app to join and leave the "shirts" CA and the "shoes" CA which
represent custom audiences for advertising shirts and shoes respectively.

Once you have joined one or both of the custom audiences, you can run ad selection
by pressing the "Run Ad Selection" button. This will trigger an ad auction. The
ad auction will pull the bidding and scoring logic, in the form of javascript files,
from two web endpoints. With the way the javascript is set up,
if the phone is part of both CAs the shoes CA will outbid the shirts CA and win
the auction.

Once the ad auction is complete, the app with show the URL that would normally
be used the render the ad in the top purple Textview. The app will then trigger
impression reporting which will run the reportWin and reportResults functions from the
javascript files pulled down during the ad auction. These javascript files will
instruct FLEDGE to send some data back to two additional endpoints
which will then display the HTTPS calls they received in the second and third
TextViews.

[design proposal]: https://developer.android.com/privacy-sandbox/fledge
[set up your development environment]: https://developer.android.com/design-for-safety/privacy-sandbox/setup
[developer guide]: https://developer.android.com/design-for-safety/privacy-sandbox/guides/fledge
[FledgeServerSpec]: ../FledgeServerSpec
