<properties
    pageTitle="Kuidas saata ajastatud teatised | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse Azure'i teatis jaoturi abil ajastatud teatised."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="tõuketeatised, vajutage teatis, Tõuketeatiste plaanimine"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Kuidas: Ajastatud teatiste saatmine


##<a name="overview"></a>Ülevaade

Kui teil on stsenaarium, kus soovite teatise saata mingil hetkel tulevikus, kuid on lihtne viis äratada üles oma tagaandmebaas koodi saata teate. Standardse taseme teatise jaoturi toetab funktsioon, mis võimaldab teil plaanida teatised kuni 7 päeva tulevikus.

Teatise saatmisel lihtsalt kasutada [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klassi teatis jaoturi SDK nagu on näidatud järgmises näites:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Samuti saate tühistada varem ajastatud teate, kasutades oma notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Seal on mingeid piiranguid arvu saate saata ajastatud teatised.