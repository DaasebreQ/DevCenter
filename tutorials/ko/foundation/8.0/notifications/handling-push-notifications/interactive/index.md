---
layout: tutorial
title: 대화식 알림
relevantTo: [ios, cordova]
show_in_nav: false
weight: 2
---
<!-- NLS_CHARSET=UTF-8 -->
## 개요
{: #overview }
대화식 알림을 사용하면 알림이 도착할 때 사용자가 애플리케이션을 열지 않고도 조치를 취할 수 있습니다. 대화식 알림이 도착하면 디바이스는 알림 메시지와 함께 조치 단추를 표시합니다.

대화식 알림은 iOS 버전 8 이상과 Android v7.0(API 레벨 24) 이상의 디바이스에서 지원됩니다. 이전 버전의 iOS 또는 Android 디바이스에 대화식 알림이 전송되면 알림 조치가 표시되지 않습니다.

## 대화식 푸시 알림 전송
{: #sending-interactive-push-notification }
알림을 준비하고 알림을 전송하십시오. 자세한 정보는 [푸시 알림 전송](../../sending-notifications)을 참조하십시오.

대화식 알림은 알림 조치 및 알림 조치를 카테고리로 그룹화하는 데 중점을 둡니다.  우선 애플리케이션 사용자에게 제공될 수 있는 다양한 알림 조치를 정의하십시오.  그 다음에는 각 카테고리가 하나 이상의 알림 조치를 그룹화하는 카테고리를 정의하십시오.  가장 중요한 것은 각 카테고리마다 고유 ID를 지정하는 것입니다.  그리고 대화식 푸시 알림을 iOS 디바이스에 전송하려면 **{{ site.data.keys.mf_console }} → [애플리케이션] → 푸시 → 알림 전송 → iOS 사용자 정의 설정** 및/또는 **{{ site.data.keys.mf_console }} → [애플리케이션] → 푸시 → 알림 전송 → Android 사용자 정의 설정** 아래에서 카테고리 ID를 지정하십시오. 카테고리 ID를 기반으로, 알림 조치 단추가 모바일 디바이스에 표시됩니다. 예를 들어, 다음과 같습니다.

![{{ site.data.keys.mf_console }}에서 iOS 대화식 알림의 카테고리 설정](categories-for-interactive-notifications.png)

대화식 알림을 Android 디바이스에 전송하려면 **{{ site.data.keys.mf_console }} → [애플리케이션] → 푸시 → 알림 전송 → Android 사용자 정의 설정** 아래에서 대화식 카테고리 ID를 지정하십시오.   예를 들어, 다음과 같습니다.

![ {{ site.data.keys.mf_console }}](android-settings-interactive-notifications.png)

## Cordova 애플리케이션에서 대화식 푸시 알림 처리
{: #handling-interactive-push-notifications-in-cordova-applications }
대화식 알림을 수신하려면 다음과 같은 단계를 수행하십시오.

1. 기본 JavaScript에서 대화식 알림에 대해 등록된 카테고리를 정의한 후 디바이스 등록 호출 `MFPPush.registerDevice`에 전달하십시오.

   ```javascript
   var options = {
        ios: {
            alert: true,
            badge: true,
            sound: true,     
            categories: [{
                //Category identifier, this is used while sending the notification.
                id : "poll", 

                //Optional array of actions to show the action buttons along with the message.    
                actions: [{
                    //Action identifier
                    id: "poll_ok", 

                    //Action title to be displayed as part of the notification button.
                    title: "OK", 

                    //Optional mode to run the action in foreground or background. 1-foreground. 0-background. Default is foreground.
                    mode: 1,  

                    //Optional property to mark the action button in red color. Default is false.
                    destructive: false,

                    //Optional property to set if authentication is required or not before running the action.(Screen lock).
                    //For foreground, this property is always true.
                    authenticationRequired: true
                },
                {
                    id: "poll_nok",
                    title: "NOK",
                    mode: 1,
                    destructive: false,
                    authenticationRequired: true
                }],
                    
                //Optional list of actions that is needed to show in the case alert. 
                //If it is not specified, then the first four actions will be shown.
                defaultContextActions: ['poll_ok','poll_nok'],

                //Optional list of actions that is needed to show in the notification center, lock screen. 
                //If it is not specified, then the first two actions will be shown.
                minimalContextActions: ['poll_ok','poll_nok'] 
            }]     
        }
   }
   ```

2. 푸시 알림에 대해 디바이스를 등록하는 중에 `options` 오브젝트를 전달하십시오.

   ```javascript
   MFPPush.registerDevice(options, function(successResponse) {
  		navigator.notification.alert("Successfully registered");
  		enableButtons();
   });  
   ```

## 네이티브 iOS 애플리케이션에서 대화식 푸시 알림 처리
{: #handling-interactive-push-notifications-in-native-ios-applications }
대화식 알림을 수신하려면 다음과 같은 단계를 수행하십시오.

1. 원격 알림 수신 시 백그라운드 태스크를 수행하는 애플리케이션 기능을 사용으로 설정하십시오. 일부 조치가 백그라운드 사용 조치인 경우 이 단계가 필요합니다.
2. 대화식 알림에 대해 등록된 카테고리를 정의한 후 옵션으로 `MFPPush.registerDevice`에 전달하십시오.

   ```swift
   //define categories for Interactive Push
   let acceptAction = UIMutableUserNotificationAction()
   acceptAction.identifier = "OK"
   acceptAction.title = "OK"
   acceptAction.activationMode = .Foreground

   let rejetAction = UIMutableUserNotificationAction()
   rejetAction.identifier = "Cancel"
   rejetAction.title = "Cancel"
   rejetAction.activationMode = .Foreground

   let category = UIMutableUserNotificationCategory()
   category.identifier = "poll"
   category.setActions([acceptAction, rejetAction], forContext: .Default)

   let categories:Set<UIUserNotificationCategory> = [category]

   let options = ["alert":true, "badge":true, "sound":true, "categories": categories]

   // Register device
    MFPPush.sharedInstance().registerDevice(options as [NSObject : AnyObject], completionHandler: {(response: WLResponse!, error: NSError!) -> Void in
   ```
