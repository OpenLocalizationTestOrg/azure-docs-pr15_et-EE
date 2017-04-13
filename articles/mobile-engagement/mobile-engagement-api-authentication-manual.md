<properties 
    pageTitle="Autentimiseks Mobile kaasamine REST API - käsitsi häälestamine"
    description="Kirjeldab, kuidas autentimise Mobile kaasamine REST API-de käsitsi häälestamine" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Autentimiseks Mobile kaasamine REST API - käsitsi häälestamine

See on mõne [autentimise abil Mobile kaasamine REST API-de](mobile-engagement-api-authentication.md)lisa dokumendid. Veenduge, et loete seda esmalt kontekstis. See kirjeldab ühekordse häälestamine oma Azure'i portaalis Mobile kaasamine REST API-de autentimise häälestamiseks tehke muid võimalusi. 

>[AZURE.NOTE] Järgmised juhised põhinevad selle [Active Directory juhend](../resource-group-create-service-principal-portal.md) ja kohandanud, mida on vaja autentimise Mobile kaasamine API-de jaoks. Et viidata kui soovite mõista üksikasjalikult allolevad juhised. 

1. [Klassikaline portaali](https://manage.windowsazure.com/)kaudu Azure'i kontosse sisselogimise.

2. Valige vasakul paanil **Active Directory** .

     ![Valige Active Directory][1]

3. Valige oma Azure portaali **Vaikimisi Active Directory** . 

     ![Valige kataloog][2]

    >[AZURE.IMPORTANT] Seda moodust toimib ainult siis, kui te töötate vaikimisi Active Directory konto ja ei tööta, kui te teete Active Directory, mis on loodud teie kontolt. 

4. Rakenduste kataloogi kuvamiseks klõpsake nuppu **rakendused**.

     ![Kuva rakendused][3]

5. Klõpsake **lisada**. 

     ![rakenduse lisamine][4]

6. Klõpsake **oma ettevõttes areneb rakenduse lisamine**

     ![Uus rakendus][5]

6. Täitke rakenduse nimi ja valige **Rakendus ja/või WEB Veebiteenuste** rakenduse tüüp ja klõpsake nuppu edasi.

     ![rakenduse nimi][6]

7. Saate sisestada fiktiivne URL-id **SIGN-ON URL-i** ja **Rakenduse ID URI**. Meie stsenaariumi puhul ei kasutata ja URL-ide ise ei ole kontrollitud.  

     ![Teenuserakenduse atribuutide][7]

8. Selle lõpus on teil rakenduse AAD nimega andsite varem umbes järgmine. See on teie **AD\_rakenduse\_nimi** ja märkige see.  

     ![rakenduse nimi][8]

9. Klõpsake rakenduse nime ja klõpsake **konfigureerimine**.

     ![rakenduse konfigureerimine][9]

10. Kirjutage kliendi ID, mida kasutatakse **Kliendi\_ID** oma API kõned. 

     ![rakenduse konfigureerimine][10]

11. Liikuge kerides jaotiseni **võtmed** ja lisage parim 2 aastat (lõppu) kestus võti ja klõpsake nuppu **Salvesta**. 

     ![rakenduse konfigureerimine][11]


12. Väärtus, mis kuvatakse võtme, nagu see kuvatakse ainult nüüd ja on talletatud, et seda ei kuvata kunagi kohe kopeerimine Kui te kaotate see siis on luua uus võti. Sellest saab **CLIENT_SECRET** API kõnede. 

     ![rakenduse konfigureerimine][12]

    >[AZURE.IMPORTANT] Seega veenduge, et seda uuendada, kui saabub aeg muidu teie API autentimine ei tööta enam määratud kestus lõpus aegub see võti. Saate kustutada ja taastada selle klahvi, kui arvate, et see on rikutud.
 
13. Klõpsake nuppu **Kuva lõpp-punktid** nüüd mis avab dialoogiboksi **Rakenduse lõpp-punktid** . 

    ![][13]

14. Kopeerige dialoogiboksis rakenduse lõpp-punktid **OAUTHI 2.0 TURBELOA lõpp-punkti**. 

    ![][14]

15. Selle lõpp-punkti saab kus GUID URL-is on teie **TENANT_ID** nii märkige see järgmisel kujul: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Nüüd jätkame õigusi selle rakenduse konfigureerimiseks. See on teil avada [Azure portaali](https://portal.azure.com). 

17. Klõpsake **Ressursi rühmad** ja otsige ressursirühm **Mobile allikaid** .  

    ![][15]

18. Klõpsake nuppu **Mobile kaasamine** ressursirühm ja liikuge **sätted** tera siin. 

    ![][16]

19. Klõpsake **kasutajate** tera sätted ja seejärel klõpsake **Lisa** kasutaja. 

    ![][17]

20. **Valige roll,** klõpsake nuppu

    ![][18]

21. Klõpsake **omanik**

    ![][19]

22. Otsige oma rakenduse nimi **AD\_rakenduse\_nimi** otsinguväljale. Te ei näe seda vaikimisi siin. Kui leiate, valige see ja klõpsake **Valige** tera allosas. 

    ![][20]

23. Enne **Lisamine Accessi** , kuvatakse see **1 kasutaja**, 0 rühmad. See blade muudatuse kinnitamiseks klõpsake nuppu **OK** . 

    ![][21]

Nüüd olete täitnud nõutav AAD konfiguratsioon ja te kõik määratakse soovitud API-d. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



