---
title: MobileFirst Foundation iFix 8.0.0.0-MFPF-IF201801301424 released
date: 2018-02-01
version:
- 8.0
tags:
- MobileFirst_Foundation
- Announcement
- iFix_8.0
- iFix
author:
  name: Sreelatha Sankaranarayanan
---
A new iFix has been released for MobileFirst Foundation 8.0, dated **January 30th, 2018**.

## Changes in this iFix
*For a cumulative list of all previous fixes, see the [iFix download page on IBM Fix Central](http://www.ibm.com/support/fixcentral/swg/quickorder?parent=ibm%7EOther%2Bsoftware&product=ibm/Other+software/IBM+MobileFirst+Platform+Foundation&release=8.0.0.0&platform=All&function=all&source=fc).*

### APARs Fixed

**PI92805** EXPORTING NETWORK TRANSACTION DATA FAILED TO LIST ALL ADAPTERCALLS<br/>
**PI92803** TTL MANAGER FAILED TO PURGE DATA IF TTL SET WITH 30D<br/>
**PI92303** CROSS-SITE SCRIPTING (XSS) AND UNAUTHENTICATED ACCESS TO BUSINESS APPLICATION FILES<br/>
**PI92168** UPLOADING APK FILE TO APPLICATION CENTER FAILS WITH NULLPOINTEREXCEPTION IN LOG<br/>
**PI92043** LOGIN_IN_PROGRESS ERROR IS THROWN WHEN THE APP IS UNREGISTERED AND LOGOUT API CALLED BEFORE LOGIN<br/>
**PI91471** HANDLEOPENURL() NOT WORK PROPERLY IN IOS<br/>
**PI90760** THE ANDROID:MAXSDKVERSION VALUE IS OVERWRITTEN WHEN AN ANDROID APPLICATION IS BUILT<br/>
**PI90122** HTTPS CLIENT CONNECT REQUEST TO SERVER RETURNS OAUTH COMPLIANT 302, BUT WITH HTTP URL<br/>
**PI89372** The performance of JSONStore find has become severely degraded after applying an ifix.<br/>
**PI86996** SET OFFSET IN PAGINATION IN ANDROID THROWING ERRORS<br/>

## How to upgrade
**Server**  
To upgrade, download &amp; install the [Developer Kit for evaluators]({{site.baseurl}}/downloads/), [Developer Kit for customers / iFix package for on-prem production environment](http://www.ibm.com/support/fixcentral/swg/quickorder?parent=ibm%7EOther%2Bsoftware&product=ibm/Other+software/IBM+MobileFirst+Platform+Foundation&release=8.0.0.0&platform=All&function=all&source=fc) (requires login to IBM Fix Central), or refresh your Mobile Foundation service from your service Dashboard.

**Client SDKs**  
To upgrade, [run the upgrade commands for your platform]({{site.baseurl}}/tutorials/en/foundation/8.0/application-development/sdk/).


## Individual artifact build numbers in this iFix
*The artifacts updated in the iFix are emphasized.*

<div class="panel-group accordion" id="mfp-component-builds" role="tablist">
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="mfp-devkit">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-mfp-devkit" aria-expanded="true" aria-controls="collapse-mfp-devkit"><b>MobileFirst DevKit</b></a>
            </h4>
        </div>
        <div id="collapse-mfp-devkit" class="panel-collapse collapse" role="tabpanel" aria-labelledby="mfp-devkit">
            <div class="panel-body">
                  <b>8.0.0.0-MFPF-DevKit-Linux-IF201801301424.bin</b><br/>
                  <b>8.0.0.0-MFPF-DevKit-MacOSX-IF201801301424.zip</b><br/>
                  <b>8.0.0.0-MFPF-DevKit-Windows-IF201801301424.exe</b><br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="cordova-plugins">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-cordova-plugins" aria-expanded="true" aria-controls="collapse-cordova-plugins"><b>Cordova plugins</b></a>
            </h4>
        </div>
        <div id="collapse-cordova-plugins" class="panel-collapse collapse" role="tabpanel" aria-labelledby="cordova-plugins">
            <div class="panel-body">
                  <b>cordova-plugin-mfp              8.0.2018012409</b><br/>
                  cordova-plugin-mfp-encrypt-utils   8.0.2017021815<br/>
                  cordova-plugin-mfp-fips            8.0.2017090705<br/>
                  cordova-plugin-mfp-jsonstore       8.0.2017090705<br/>
                  cordova-plugin-mfp-push            8.0.2017082110<br/>
                  cordova-template-mfp               8.0.2017060206<br/>
                  ibm-mfp-web-sdk                    8.0.2017082310<br/>
                  passport-mfp-token-validation      8.0.2017010917<br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="tools">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-tools" aria-expanded="true" aria-controls="collapse-tools">Tools</a>
            </h4>
        </div>
        <div id="collapse-tools" class="panel-collapse collapse" role="tabpanel" aria-labelledby="tools">
            <div class="panel-body">
                  mfpdev-cli 8.0.2017102406<br/>
                  mfpmigrate-cli 8.0.20171211072611<br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="ios-sdk">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-ios-sdk" aria-expanded="true" aria-controls="collapse-ios-sdk">iOS SDK</a>
            </h4>
        </div>
        <div id="collapse-ios-sdk" class="panel-collapse collapse" role="tabpanel" aria-labelledby="ios-sdk">
            <div class="panel-body">
                    IBMMobileFirstPlatformFoundation              8.0.2017121910<br/>
                    IBMMobileFirstPlatformFoundationOpenSSLUtils  8.0.2017121910<br/>
                    IBMMobileFirstPlatformFoundationPush         8.0.2017061612<br/>
                    IBMMobileFirstPlatformFoundationJSONStore    8.0.2017053010<br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="android-sdk">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-android-sdk" aria-expanded="true" aria-controls="collapse-android-sdk"><b>Android SDK</b></a>
            </h4>
        </div>
        <div id="collapse-android-sdk" class="panel-collapse collapse" role="tabpanel" aria-labelledby="android-sdk">
            <div class="panel-body">
                    <b>ibmmobilefirstplatformfoundation 8.0.2018011011</b><br/>
                    ibmmobilefirstplatformfoundationpush            8.0.2017011813<br/>
                    ibmmobilefirstplatformfoundationjsonstore       8.0.2017092509<br/>
                    adapter-maven-plugin              8.0.2017021701<br/>
                    adapter-maven-archetype-sql       8.0.2017021701<br/>
                    adapter-maven-archetype-java      8.0.2017021701<br/>
                    adapter-maven-archetype-http      8.0.2017021701<br/>
                    adapter-maven-api                 8.0.2017021701<br/>
                    mfp-security-checks-base          8.0.2017020112<br/>
                    mfp-java-token-validator          8.0.2017020112<br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="win-sdk">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-win-sdk" aria-expanded="true" aria-controls="collapse-win-sdk">Windows SDK</a>
            </h4>
        </div>
        <div id="collapse-win-sdk" class="panel-collapse collapse" role="tabpanel" aria-labelledby="win-sdk">
            <div class="panel-body">
                    IBMMobileFirstPlatform Foundation 8.0.2017092012<br/>
                    IBM MobileFirstPlatform Push SDK  8.0.2017012419<br/>
            </div>
        </div>      
    </div>
    <div class="panel panel-default">
        <div class="panel-heading" role="tab" id="xamarin-sdk">
            <h4 class="panel-title">
                <a role="button" data-toggle="collapse" data-parent="#mfp-component-builds" href="#collapse-xamarin-sdk" aria-expanded="true" aria-controls="collapse-xamarin-sdk">Xamarin SDK</a>
            </h4>
        </div>
        <div id="collapse-xamarin-sdk" class="panel-collapse collapse" role="tabpanel" aria-labelledby="xamarin-sdk">
            <div class="panel-body">
                    IBMMobileFirstPlatform SDK 8.0.2017051208<br/>
            </div>
        </div>      
    </div>
</div>        
