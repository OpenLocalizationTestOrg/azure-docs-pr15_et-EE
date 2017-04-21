Azure'i turuplatsi tehakse kättesaadavaks mitmesuguseid populaarsed veebirakenduste töötanud Microsoft, kolmanda osapoole ettevõtetele ja avatud allika tarkvara algatused. Azure'i turuplatsilt loodud veebirakenduste vaja installi peale ühendamiseks [Azure eelvaade portaali](http://go.microsoft.com/fwlink/?LinkId=529715)brauseri tarkvara. 

Selles õppetükis saate teada:

- Kuidas luua uus web appi kaudu Azure'i turuplatsi.

- Kuidas kasutada web app Azure'i eelvaade portaali kaudu.
 
Saate suurendada WordPress ajaveebi, mis kasutab vaikemalli. Järgmisel joonisel on täidetud:


![WordPress ajaveeb][13]

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="create-a-web-app-in-the-portal"></a>Luua veebirakenduse portaalis

1. Azure'i eelvaade portaali sisse logida.

2. Avage Azure'i turuplatsi kas **turuplatsi** ikooni.

    ![Turuplatsi ikoon][marketplace]

    Või klõpsake ikooni **Uus** paremas ülanurgas armatuurlaud ning valides **turuplatsi** veebisaidil bottow loendi.
    
    ![Loo uus][5]
    
3. Valige **Web + Mobile**. Otsige **WordPress** ja **WordPress** ikooni.

    ![WordPress loendist][7]
    
5. Pärast lugemist WordPress rakenduse kirjeldus, valige **Loo**.

6. Klõpsake **Web**Appile ja sisestage nõutav väärtused konfigureerida oma veebirakenduse.
    
    ![rakenduse konfigureerimine][8]

7. Klõpsake **andmebaasi**ja sisestage nõutav väärtused konfigureerida MySQL-andmebaasiga. 

    ![andmebaasi konfigureerimine][database]

8. Sisestage nimi uue ressursirühma.

    ![Ressursirühm määramine][groupname]

8. Vajaduse korral käsku **tellimus**ja määrake tellimuse kasutada. 

7. Kui olete lõpetanud määratlemine veebirakenduse, klõpsake nuppu **Loo**ja oodake, kuni on loodud uue veebirakenduse.

   Rakenduse loomisel kuvatakse ressursirühm, mis sisaldab web app ja andmebaasi.

   ![Kuva rühm][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Käivitada ja hallata oma WordPress web app
    
1. Uue veebirakenduse, rakenduse üksikasjade kuvamiseks klõpsake nuppu.

    ![Käivitage armatuurlaud][10]

2. **Essentialsi** lehel nuppu **Sirvi** või lingi **URL-i** web appi Tervituslehe avamiseks klõpsake jaotises.

    ![saidi URL-i][browse]

3. Kui teil on installitud WordPress, sisestage nõutud WordPress vastav konfiguratsiooniteavet ja **Installida WordPress** konfiguratsiooni viimistlemine ja web appi sisselogimislehe avamiseks klõpsake.

4. Klõpsake nuppu **Logi sisse** ja sisestage oma kasutajanimi ja parool.  

5. Peate uue WordPress veebirakenduse, mis sarnaneb allpool veebirakenduse.    

    ![saidi WordPress][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
