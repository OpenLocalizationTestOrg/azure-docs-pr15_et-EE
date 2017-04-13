<properties 
    pageTitle="Tuntud võrke | Microsoft Azure'i" 
    description="Teadaolevad võrkude konfigureerides saate vältida IP-aadressid, mis kuuluvad ettevõtte Logi lisandmoodulid: mitme kaugemad ja IP-aadresside kahtlaste tegevuste aruannetega Logi lisandmoodulid." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Teadaolevad võrkude


Saate Azure Active Directory access ja kasutusaruannete saada nähtavus terviklus ja ettevõtte kataloogi turvalisus. Selle teabe directory administraator saab paremini määratleda, kus võimalikest võimalik, et nad saavad piisavalt leping neid riske leevendada.

On võimalik, et "*Logi lisandmoodulid: mitme kaugemad*" ja "*Logi lisandmoodulid IP-aadresside kahtlase tegevuse*" aruannete lipu IP-aadressid, mis kuuluvad teie organisatsiooni tegelikult valesti. 

See võib juhtuda näiteks kui: 

- Teie office on sisse logitud kaugühenduse teel oma andmekeskuse San Francisco Boston kasutaja käivitab "Logige lisandmoodulid: mitme kaugemad" aruanne 

- Teie ettevõtte kasutaja proovib sisselogimine mitu korda on vale parooliga päästikute "Logige lisandmoodulid IP-aadresside kahtlase tegevuse" aruanne 

Sellisel juhul eksitavat turvalisus aruannete loomiseks vältimiseks tuleks lisada teadaolevad IP-aadresside vahemikud loendisse ettevõtte avaliku IP-aadress.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Kui soovite lisada oma ettevõtte avaliku IP-aadresside vahemikud, tehke järgmist: 

1.  Sisselogimise [Azure haldusportaali](https://manage.windowsazure.com).

2.  Klõpsake vasakpoolsel paanil nuppu **Active Directory**. <br><br>![Pilveteenuse rakenduse Discovery tööpõhimõtted](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Valige vahekaardil **Directory** kataloogi.

4.  Menüü üleval, klõpsake nuppu **Konfigureeri**. <br><br>![Pilveteenuse rakenduse Discovery tööpõhimõtted](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Avage menüü konfigureerimine **oma ettevõtted avaliku IP-aadresside vahemikud** <br><br>![Pilveteenuse rakenduse Discovery tööpõhimõtted](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Klõpsake **Lisa teadaolevad IP-aadresside vahemikud**.

7.  Kuvatavas dialoogiboksis oma aadresside vahemikud lisamiseks ja seejärel klõpsake nuppu kontrolli, kui olete valmis. <br><br>![Pilveteenuse rakenduse Discovery tööpõhimõtted](./media/active-directory-known-networks/known-netwoks-04.png)


**Lisaressursid**


* [Saate vaadata oma aruandeid juurdepääs ja kasutamine](active-directory-view-access-usage-reports.md)
* [IP-aadresside kahtlaste tegevuste Logi lisandmoodulid](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Logige lisandmoodulid: mitme kaugemad](active-directory-reporting-sign-ins-from-multiple-geographies.md)


