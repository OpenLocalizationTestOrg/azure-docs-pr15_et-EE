
1. Leiate [Azure'i portaalis]. Klõpsake nuppu **Sirvi kõik** > **Mobile'i rakendused** > äsja loodud taustväärtus. Mobiilirakenduse sätted, klõpsake **Kiirjuhend** > **Cordova**. **Konfigureerimine klientrakenduse**, valige **Loo uus rakendus**ja seejärel klõpsake nuppu **Laadi alla**. See allalaadimist Cordova projekti lõpuleviimist rakenduse eelnevalt konfigureeritud ühenduse loomiseks oma taustväärtus.

2. Lahti allalaaditud ZIP faili kausta, kõvakettale, liikuge lahenduse faili (.sln) ja avage see Visual Studio abil.

5. Visual Studios, valige lahenduse platvormi (Androidi, iOS-i või Windowsi) rippmenüü noolt Alusta kõrval ja seejärel valige emulaator või konkreetse juurutamise seadme, klõpsates rippmenüüd rohelist noolt. Pange tähele, et saate kasutada vaikimisi Androidi platvormi ja sulin emulaator. Täpsemate õpetused vajavad valida toetatud seade või emulaator. 

6. Vajutage klahvi F5 või klõpsake rohelist noolt koostamiseks ja ja Cordova rakenduse käivitamine. Kui näete turvalisuse dialoog emulaator taotlemise juurdepääs võrku, selle vastu.   

7. Pärast soovitud rakendus käivitatakse seadmes või emulaator, tippige **Sisestage uus tekst**, nt _lõpuleviimine õpetuse_ selgitav tekst ja seejärel klõpsake nuppu **Lisa** .  
See saadab POST taotlus Azure kirjutamata, saate juurutada varasemas versioonis. Taotluse taustväärtus lisab andmed on SQL-andmebaasi tabelisse TodoItem ja tagastab äsja salvestatud üksuste kohta teavet naasmiseks mobiilirakenduse kaudu. Mobiilirakenduse kaudu, kuvatakse loendis selle andmed.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Korrake eelmisi toiminguid kolme iga seadme platvormi, mis on kavas toetada.

[Azure'i portaal]: https://portal.azure.com/
