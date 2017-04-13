### <a name="prerequisites"></a>Eeltingimused

Teil peab olema [Teenuse siini](https://azure.microsoft.com/services/service-bus/) konto.  

Enne kasutamist konto Azure'i teenus siini loogika Appis, annate loogika rakenduse teenuse siini kontoga ühenduse loomiseks. Õnneks saate teha seda hõlpsalt: Azure'i portaalis loogika rakendusest.  

Siin on vajalikud toimingud, et lubada rakenduse loogika teie teenuse siini kontoga ühenduse loomiseks.  

1. Loogika rakenduse Designeris teenuse siini, ühenduse loomiseks valige **Kuva Microsoft hallatav API-de** rippmenüü loendi. Sisestage otsinguväljale **teenuse siini** . Valige toiming, mida soovite kasutada või käivitada.  
    ![Teenuse siini ühendus pilt 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Kui te pole loonud teenuse siini enne kõik ühendused, palutakse teil sisestada mandaat teenuse siini. Neid mandaate ühenduse ja teenuse siini konto andmetele juurde pääseda rakenduse loogika autoriseerida. Teenuse siini konnektor peab teenuse siini nimeruumi ühendusstring. Jaoks on vaja õiguste **haldamine** . Hea võimalus teada, kui teie ühendusstring on nimeruumi või konkreetse isiku on, kas see sisaldab selle `EntityPath` parameeter. Kui seda ei, ei ole loogika rakenduse paremas ühendusstring.  
    ![Teenuse siini ühendusstring](./media/connectors-create-api-servicebus/connectionstring.png)

1. Kui olete saanud ühendusstringi nimeruumi, saate seda kasutada API ühenduse loogika rakendustes.  
    ![Teenuse siini ühendus pilt 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Pange tähele, et ühendus on loodud ja olete nüüd tasuta rakenduse loogika juhiseid jätkata.  
    ![Teenuse siini ühendus pilt 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
