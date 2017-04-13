<properties
 pageTitle="Alustamine Azure'i Scheduler Azure'i portaalis | Microsoft Azure'i"
 description="Azure'i portaalis Azure Scheduler alustamine"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Azure'i portaalis Azure Scheduler alustamine

See on lihtne Azure'i Scheduler ajastatud loomiseks. Selles õppetükis saate teada, kuidas luua tööd. Õpite Toiminguajasti jälgimise ja halduse võimaluste.

## <a name="create-a-job"></a>Töö loomine

1.  [Azure'i portaali](https://portal.azure.com/)sisse logima.  

2.  Valige **+ Uus** > _ajasti_ Tippige väljale Otsing > valige **Scheduler** tulemuste > nuppu **Loo**.

     ![][marketplace-create]

3.  Loome töö, mis lihtsalt tabab http://www.microsoft.com/ GET-päringu abil. **Töö Scheduler** ekraanil, sisestage järgmine teave:

    1.  **Nimi:**`getmicrosoft`  

    2.  **Tellimus:** Azure'i tellimuse   

    3.  **Töö saidikogumi:** Valige töö olemasolevasse kollektsiooni või klõpsake nuppu **Loo uus** > sisestage nimi.

4.  Järgmiseks **Toimingusätted**määratleb järgmised väärtused:

    1.  **Toimingu tüüp:**` HTTP`  

    2.  **Meetod:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Lõpuks vaatame määratleda ajakava. Töö võib määratletud ühekordne töö, kuid vaatame valige ajakava Korduvus.

    1. **Korduvus**:`Recurring`

    2. **Alustamine**: tänane kuupäev

    3. **Recur iga**:`12 Hours`

    4. **Lõpp**: kaks päeva, alates tänasest tänase kuupäevaga  

      ![][recurrence-schedule]

6.  Klõpsake nuppu **Loo**

## <a name="manage-and-monitor-jobs"></a>Hallata ja jälgida töö

Kui tööd on loodud, kuvatakse see peamine Azure armatuurlaud. Klõpsake töö ja uus aken avaneb järgmisi vahekaarte.

1.  Atribuudid  

2.  Toimingu sätted  

3.  Ajakava  

4.  Ajalugu

5.  Kasutajatele

    ![][job-overview]

### <a name="properties"></a>Atribuudid

Kirjutuskaitstud atribuutidest kirjeldada halduse metaandmete ajasti töö.

   ![][job-properties]


### <a name="action-settings"></a>Toimingusätete

Nupu töö **töö** kuval saate konfigureerida selle töö. See võimaldab konfigureerida Täpsemad sätted, kui te ei konfigureerinud need on kiire loomine viisardi.

Kõigi toimingu jaoks võivad muutuda, proovi uuesti poliitika ja toimingu tõrge.

HTTP ja HTTPS töö toimingu puhul, võib vahetada mis tahes lubatud HTTP tegusõna meetodit. Võib-olla lisamine, kustutamine või päised ja elementaarautentimine teabe muutmine.

Salvestusruumi järjekorda toimingu puhul, võivad muutuda, salvestusruumi konto, järjekorra nimi, SAS sõnet ja keha.

Teenuse siini toimingu puhul, võivad muutuda, nimeruumi, teema ja järjekorda tee, autentimissätted, transport tüüp, sõnumi atribuudid ja sõnumi kehasse.

   ![][job-action-settings]

### <a name="schedule"></a>Ajakava

See võimaldab teil konfigureerida ajakava, kui soovite muuta loodud ajakava on kiire loomine viisardi.

See on võimalus ehitada [keerukas ajakava ja Täpsem Korduvus oma töö](scheduler-advanced-complexity.md)

Saate muuta alguskuupäev ja aeg, Korduvus ajakava ja lõpp kuupäev ja kellaaeg (kui töö on korduv.)

   ![][job-schedule]


### <a name="history"></a>Ajalugu

Menüüs **ajalugu** kuvab valitud mõõdikud iga töö käitamise valitud töö süsteem. Need mõõdikud sisestage reaalajas väärtused oma Scheduler seisundi kohta.

1.  Olek  

2.  Üksikasjad  

3.  Uuestiproovimise katsest

4.  Esinemiskord: 1., 2, 3, jne.

5.  Täitmise alguskellaaeg  

6.  Täitmise lõppkellaaega

   ![][job-history]

Võite klõpsata juhitud vaatamiseks **Ajalugu üksikasjad**, sh iga käitamise kogu vastus. Selle dialoogiboksi võimaldab teil vastus lõikelauale kopeerida.

   ![][job-history-details]

### <a name="users"></a>Kasutajatele

Azure'i Rollipõhine juurdepääsu juhtimine (RBAC) võimaldab kohandatud juurdepääsu juhtimine Azure'i Scheduleri. Saate teada, kuidas kasutada vahekaarti kasutajad, vaadake [Azure_Role-Based juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Vt ka

 [Mis on ajasti?](scheduler-intro.md)

 [Ajasti põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Kuidas luua keerukate ajakavasid ja täpsemalt Korduvus Azure'i Scheduleri abil](scheduler-advanced-complexity.md)

 [Ajasti REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Ajasti PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Ajasti kõrge-saadavus ja usaldusväärsus](scheduler-high-availability-reliability.md)

 [Tõrkekoodid ajasti piirangud ja vaikesätted](scheduler-limits-defaults-errors.md)

 [Ajasti väljaminevat autentimist.](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
