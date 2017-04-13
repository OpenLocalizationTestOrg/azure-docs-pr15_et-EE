1. [Azure'i portaali]sisse logida.

2. Valige **+ Uus** > **Web + Mobile** > **Mobile'i rakendus**, seejärel sisestage oma mobiilirakenduse kirjutamata nimi.

3. **Ressursirühm**, valige ressursi olemasolevasse rühma või luua uue (kasutades sama nime panemine.) 
 
    Saate valida mõne muu rakenduse teenusleping või looge uus. Rakenduse teenuste kohta lisateavet lepingud ja kuidas luua uue lepingu eri hinnad taseme ja soovitud asukohta, leiate [Azure'i rakendust Service lepingute põhjalik ülevaade](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Rakenduse teenusleping**vaikimisi plaani ( [Standard taseme](https://azure.microsoft.com/pricing/details/app-service/)) on märgitud. Võite valida ka mõne muu lepingu või [looge uus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Rakenduse teenusleping sätted kindlaks [asukoht, funktsioonide maksumus ja arvutada ressursid](https://azure.microsoft.com/pricing/details/app-service/) oma rakendusega seotud. 

    Kui otsustate leping, klõpsake nuppu **Loo**. See loob Mobile'i rakendus taustväärtus. 
    
6. Klõpsake uue mobiilirakenduse kirjutamata **sätted** labale **Lühijuhend** > oma kliendi rakenduse platvorm > **andmebaasi ühendamine**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Valige **Lisa andmeühenduse** labale **SQL-andmebaasi** > **Loo uus andmebaas**, tippige andmebaasi **nimi**, valige hinnakirjad taseme ja seejärel klõpsake käsku **Server**.  Uue andmebaasi saab korduvalt kasutada. Kui teil on juba andmebaasi samasse asukohta, saate selle asemel **kasutada olemasoleva andmebaasiga**. Läbilaskevõime kulud ja suurem latentsus pole soovitatav kasutada mõnda muusse andmebaasi.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. **Uue serveri** labale kordumatu serveri nimi Tippige väljale **Serveri nimi** , sisestage kasutajanimi ja parool, märkige ruut **Luba Azure'i teenuste juurdepääsu server**ja klõpsake nuppu **OK**. See loob uue andmebaasi.

9. Tagasi labale **andmeühenduse lisamine** nuppu **ühendusstring**, tippige oma andmebaasi kasutajanime ja parooli väärtused ja klõpsake nuppu **OK**. Oodake mõni minut andmebaasi kasutusele võtta edukalt enne jätkamist.

<!-- URLs. -->
[Azure'i portaal]: https://portal.azure.com/
