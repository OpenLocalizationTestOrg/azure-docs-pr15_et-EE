Pärast kirjete jaoks teie domeeni nimi on levitatud, peate need seostada oma veebirakenduse. Järgmiste juhiste abil saate lubada domeeninimede veebibrauseri abil.

> [AZURE.NOTE] See võib võtta aega jaoks loodud eelmisi juhiseid korrates levitamine süsteemis DNS-i TXT-kirjed. Domeeninime ei saa lisada oma veebirakenduse, kuni TXT-kirje on levitatud. Kui kasutate A-kirje, ei saa lisada soovitud kirje domeeninime veebirakendusse kuni eelmises etapis loodud TXT-kirje on levitatud.
>
> Saate näiteks <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> teenuse kinnitamaks, et TXT-kirje on saadaval.

1. Avage brauseris [Azure portaali](https://portal.azure.com).

2. **Veebirakenduste** menüüs klõpsake oma veebirakenduse nime ja valige **kohandatud domeenid**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. **Kohandatud domeenide** tera, nuppu **Lisa hostname (hostinimi)**.
    
4. Kasutage **Hostname** tekstiväljade domeeninimede selles veebirakenduse seostada.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klõpsake nuppu **Valideeri**.

7.  Pärast **kontrolli** Azure'i klõpsates kuvatakse võrgukoosolekuga domeeni kinnitamise töövoog. See otsida domeeni omandiõiguse kui ka Hostname kättesaadavus ja aruande edu või tõrke üksikasjalik koos asemel arendus kohta, kuidas parandada viga.    

Selles etapis peaks olema võimalus sisestage brauseri kohandatud domeeni nimi ja leiate, et see edukalt suunab teid oma veebirakenduse.
