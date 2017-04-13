Pärast kirjete jaoks teie domeeni nimi on levitatud, peaks saama kasutada brauseri kinnitamaks, et juurde pääseda oma veebirakenduse teenuses Azure rakenduse saab kasutada oma kohandatud domeeni nime.

> [AZURE.NOTE] See võib võtta aega jaoks oma DNS-i süsteemis levitada CNAME. Näiteks <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> teenuse abil saate CNAME-i saadavuse kontrollimiseks.

Kui te pole veel lisanud oma veebirakenduse nimega liikluse haldur endpoint, peate tegema enne nimelahendus töötavad, nagu kohandatud domeeni nime marsruudib liikluse haldur. Seejärel marsruudib liikluse haldur oma veebirakenduse. Kasutage teabe lisamiseks oma veebirakenduse lõpp liikluse haldur profiili [lisamine või kustutamine lõpp-punktid](../articles/traffic-manager/traffic-manager-endpoints.md) .

> [AZURE.NOTE] Kui lõpp lisamisel oma veebirakenduse pole loendis, veenduge, et see on konfigureeritud **Standard** rakenduse teenuse leping režiimi. Kasutage selleks, et töötada liikluse haldur **standardrežiimis veebirakenduse jaoks** .

1. Avage brauseris [Azure portaali](https://portal.azure.com).

1. **Veebirakenduste** menüüs klõpsake oma veebirakenduse nime, klõpsake nuppu **sätted**ja valige **kohandatud domeenide**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. **Kohandatud domeenide** tera, nuppu **Lisa hostname (hostinimi)**.
    
1. Kasutage **Hostname** tekstiväljade liikluse haldur domeeninime seostamiseks see web app.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klõpsake nuppu **Valideeri** salvestada konfiguratsiooni domeeni nimi.

7.  Pärast **kontrolli** Azure'i klõpsates kuvatakse võrgukoosolekuga domeeni kinnitamise töövoog. See otsida domeeni omandiõiguse kui ka Hostname kättesaadavus ja aruande edu või tõrke üksikasjalik koos asemel arendus kohta, kuidas parandada viga.    

8.  Eduka kinnitamisel **Lisa hostname (hostinimi)** muutub nupp aktiivne ja saab hostname määramine. Nüüd liikuge brauseris oma kohandatud domeeni nime. Nüüd näete oma rakendus töötab teie kohandatud domeeninime abil. 

    Kui konfigureerimine on lõppenud, loetletakse **domeeninimede** osas oma veebirakenduse kohandatud domeeni nimi.

Selles etapis peaks olema võimalus sisestage liikluse haldur domeeninime nimi brauseri ja leiate, et see edukalt suunab teid oma veebirakenduse.
