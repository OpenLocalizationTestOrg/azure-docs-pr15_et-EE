
1. Visual Studio lahenduste Explorer, paremklõpsake Windowsi poe rakenduse project, klõpsake **poe** > **… Poe rakendus seostada**.

    ![Sidusettevõtte Windowsi poe rakendus](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. Viisardis **järgmise**, logige sisse oma Microsofti kontoga sisse, tippige oma rakenduse nimi **reservi uue rakenduse nime**, klõpsake käsku **reservi**.

3. Pärast rakenduse registreerimine on loodud, valige uue rakenduse nime, klõpsake nuppu **Järgmine**ja klõpsake **seostada**. Sellega lisatakse nõutavad andmed Windowsi poe rakenduse manifesti.

7. Korrake juhiseid 1 – 3 Windows Phone poest rakenduse project sama registreerimine varem loodud Windowsi poe rakenduse abil.  

7. Avage [Windows Arenduskeskus](https://dev.windows.com/en-us/overview)logige sisse oma Microsofti kontoga, klõpsake nuppu **minu rakendused**rakendus registreerimine ja seejärel laiendamine **teenuste** > **tõuketeatised**.

8. **Tõuketeatised** lehel klõpsake jaotises **Windows Push teatis Services (WNS) ja Microsoft Azure'i mobiilirakenduste** **Live Services saidi** ja kirjutage **Paketi SID** väärtused ja *Praegune* väärtus **Rakenduse salajane**. 

    ![Rakenduse säte on Arenduskeskus](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Rakenduse salajane ja paketi SID on oluline turvalisuse mandaat. Need väärtused teistega või levitada neid oma rakendusega.
