<properties 
    pageTitle="Saata isikupärastatud teatis koos Azure Mobile kaasamine" 
    description="Kuidas saata isikupärastatud teatised, sh kasutajaprofiili teabe teatistes, nt nende nimed"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Isikupärastage teatised kasutaja nimi

Teie püüdes teha teatised muudavad selle ilme meeldivaks rakenduse kasutajatele, peaksite need isikupärastamine. Üks võimas lähenemine koosneb valikuliselt saate neid rohkem isiklikke teateid rakendus kasutajate nimed abil. Sõna siin - hoiatused te peaksite lähenemisviisi lisamise kasutajanimed teatised hoolikalt Kuna kui te liig strateegia siis see võiks sattuda nii jube mõned rakenduse kasutajad. Te peate tagama, et teil on lastes anda need isikuandmete teile täielik nõusolekut mobiilirakenduses enne selle kasutamist valida kasutaja. 

Tehniliselt Azure Mobile kaasamine, kus saate täita isikupärastamine teatised, järgides allolevaid juhiseid, kus kasutame seda stsenaariumi, sh kasutajanimi teatised. Saate rakenduse-teave mõistet või sildid, mille väärtused võivad edasi kas selle SDK-d integreeritud rakenduses Mobile või API-de kaudu. Seejärel saab kasutada järgmiste rakenduse-Infos või Sildid:

1. jaoks suunamise teatised rakendus – teave väärtustel põhineva kindlatele kasutajatele või 
2. teatised, mis asendatakse väärtused seade ja kasutajale teatud ajal teatiste saatmisel selle seadme kohatäidetena. 

> [AZURE.IMPORTANT] Pange tähele, et kiiruse teatiste saatmist kuvatakse vähendamine rakenduse-teave väärtuste asendamine iga teatised täiendavad käsitlemine tõttu. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Rakenduse register Info mobiilsideseadmete kaasamine portaalis

1) Saate alustada registreerimisel rakenduse või sildid Azure'i portaalis. Avage **sätted** -> see**silt (rakenduse teave)** .  

![][1]  

2) Klõpsake **Uus silt (rakenduse teave)** ja *user_name* on nimi ja tüüp *string* ja klõpsake nuppu **Edasta**. 

![][2]

3) Lõpuks kuvatakse selle rakenduse teave registreeritud umbes selline:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Rakenduse-teave kliendi SDK saatmine

Siin on kasutusel Windows universaalne rakenduse näide, kuid samaväärsed meetodid olemas ka meie muude SDK-d. 

Eeldades, et teil on meetodi mobiilirakenduse, kust profiiliteabe oma nime nagu kasutaja tõenäoliselt pärast autentimist neid, siis kõne `SendAppInfo` meetod siin ja asustamiseks väärtus on `user_name` rakenduse teave varem registreeritud sisse Mobiil kaasamine teenuse taustväärtus. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Saata isikupärastatud teatised

Nüüd olete valmis seda **user_name**abil teateid saata. 

1) Minge Mobile kaasamine portaalis menüü **saavutamiseks** teatise loomine ja selle kohatäite saate kasutada mis tahes kohas teatise tiitel või keha järgmises vormingus. 

![][4]  

> [AZURE.NOTE] Kõigi kasutajate jaoks, mis on seatud user_name rakenduse teave, ei saa teatised. Kui käivitate teatis turunduskampaania testi režiimis ja kui teil on rakenduse-teave, siis saadame "?" märgi kohatäite asendamiseks. 

2) Kui Mobile kaasamine valib soovite teatise saata, siis vaadake selle rakenduse-teave ja asendage väärtus kohatäite seadme.  
Näiteks kui oleme seadnud `str = "Scott"` kasutaja kui seadme registreerimine saavad seostatud rakenduse teave, **user_name = SCOTT** jaoks näevad selle kasutaja ja selle kasutaja rakenduse tõuketeatised teatise järgmises vormingus kontorist väljas. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

