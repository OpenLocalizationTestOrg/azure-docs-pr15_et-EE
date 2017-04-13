<properties 
    pageTitle="Voo Analytics: Pööramine sisselogimise identimisteave sisendi ja väljundi jaoks | Microsoft Azure'i" 
    description="Saate teada, kuidas värskendada voo Analytics sisendi ja väljundi korral kasutatav mandaat."
    keywords="sisselogimisteave"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Pööramine sisselogimisteave jaoks sisendi ja väljundi voo Analytics tööde haldamine

##<a name="abstract"></a>Ülevaade
Azure'i voo Analytics täna ei luba, asendades identimisteabe sisendit/tulemustele töö töötamise.

Vaatamata Azure'i voo Analytics viimase väljund tööd jätkata, me kalendriüksusi jagamine kogu protsessi vaheline soovitud lõpetamist minimeerimine ja töö käivitamiseks ja sisselogimise pöörata.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Osa 1 - uue mandaadikomplekt ettevalmistamine:
See osa on rakendatav sisendeid/väljundeid:

* Bloobimälu
* Sündmuse jaoturi
* SQL-andmebaas
* Tabelimälu

Muud sisendina-väljundid, jätkake osa 2.

###<a name="blob-storagetable-storage"></a>Salvestusruumi/tabeli bloobimälu
1.  Avage salvestusruumi pikendamine Azure haldusportaali kohta.  
![graphic1][graphic1]
2.  Otsige üles oma töö kasutatav salvestusruum ja selle minema:  
![graphic2][graphic2]
3.  Klõpsake käsku Halda kiirklahvide:  
![graphic3][graphic3]
4.  Accessi primaarvõtme ja teisene kiirklahv, **Valige soovitud variant ei kasutata oma töö**.
5.  Tulemus taastada:  
![graphic4][graphic4]
6.  Kopeerige äsja loodud võti.  
![graphic5][graphic5]
7.  Jätkake osa 2.

###<a name="event-hubs"></a>Sündmuse jaoturi
1.  Avage teenus siini laiend Azure haldusportaali:  
![graphic6][graphic6]
2.  Leidke teenuse siini Namespace kasutavad oma töö ja avage see.  
![graphic7][graphic7]
3.  Kui teie töö kasutab teenuse siini Namespace ühiskasutusega juurdepääsu poliitika, liikuda juhist 6  
4.  Avage menüü sündmus jaoturi.  
![graphic8][graphic8]
5.  Leidke sündmuse jaoturi kasutavad oma töö ja minna sinna.  
![graphic9][graphic9]
6.  Avage menüü konfigureerimine.  
![graphic10][graphic10]
7.  Poliitika nimi rippmenüüst otsige üles ühiskasutuses Accessi poliitika kasutavad oma töö:  
![graphic11][graphic11]
8.  Primaarvõtme ja teisese võtme, **Valige soovitud variant ei kasutata oma töö**.  
9.  Tulemus taastada:  
![graphic12][graphic12]
10. Kopeerige äsja loodud võti.  
![graphic13][graphic13]
11. Jätkake osa 2.  

###<a name="sql-database"></a>SQL-andmebaas

>[AZURE.NOTE] Märkus: peate SQL-i andmebaasi teenusega ühenduse loomiseks. Me näitab, kuidas seda teha teenuse haldamine Azure haldusportaali abil, kuid otsustate kasutada mõne kliendipoolne tööriista nagu SQL Server Management Studio ka.

1.  Avage SQL andmebaase laiend Azure haldusportaali:  
![graphic14][graphic14]
2.  Otsige üles oma töö ja **klõpsake nuppu serveris** link samal real kasutatud SQL-andmebaasi:  
![graphic15][graphic15]
3.  Klõpsake käsku Halda.  
![graphic16][graphic16]
4.  Tippige andmebaasi juhtslaidi:  
![graphic17][graphic17]
5.  Tippige oma kasutajanimi, parool ja klõpsake nuppu Logi sisse.  
![graphic18][graphic18]
6.  Klõpsake nuppu Uus päring.  
![graphic19][graphic19]
7.  Tippige järgmine päring < login_name > asendamine oma kasutajanimi ja asendamine <enterStrongPasswordHere> uue parooliga:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Klõpsake nuppu Käivita:  
![graphic20][graphic20]
9.  Minge tagasi etappi 2 ja klõpsake käsku andmebaasi.  
![graphic21][graphic21]
10. Klõpsake käsku Halda.  
![graphic22][graphic22]
11. Tippige oma kasutajanimi, parool ja klõpsake nuppu sisselogimine.  
![graphic23][graphic23]
12. Klõpsake nuppu Uus päring.  
![graphic24][graphic24]
13. Tippige järgmine päring < user_name > asendamine, mille alusel soovite tuvastada see sisselogimine kontekstis (saate sisestada andsite < login_name >, näiteks sama väärtuse) andmebaasi nimi ja asendades < login_name > uus kasutajanimi:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klõpsake nuppu Käivita:  
![graphic25][graphic25]
15. Nüüd peaks teie uus kasutaja pakkuda sama rollid ja õigused oma algse kasutaja oli.
16. Jätkake osa 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Osa 2: Peatamine voo Analytics töö
1.  Avage voo Analytics laiend Azure haldusportaali:  
![graphic26][graphic26]
2.  Leidke oma töö ja avage see.  
![graphic27][graphic27]
3.  Kas teil on pöörata sisend või väljund mandaadi põhjal sisendina vahekaardil või väljundeid minek  
![graphic28][graphic28]
4.  Klõpsake käsku Peata ja kinnitage on töö lõpetanud.  
![graphic29][graphic29] oodake töö lõpetada.
5.  Leidke sisend/väljund soovite identimisteabe pööramiseks klõpsake ja avage see.  
![graphic30][graphic30]
6.  Nüüd saate osa 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Osa 3: Redigeerimise identimisteabe voo Analytics töö

###<a name="blob-storagetable-storage"></a>Salvestusruumi/tabeli bloobimälu
1.  Leidke väli salvestusruumi konto võti ja kleepige see oma äsja loodud võti:  
![graphic31][graphic31]
2.  Klõpsake käsku Salvesta ja kinnitage, salvestades teie tehtud muudatused.  
![graphic32][graphic32]
3.  Ühenduse test käivitub automaatselt muudatuste salvestamisel veenduge, et see on edukalt on möödas.
4.  Jätkake osa 4.

###<a name="event-hubs"></a>Sündmuse jaoturi
1.  Leidke väli sündmuse jaoturi poliitika võti ja kleepige see oma äsja loodud võti:  
![graphic33][graphic33]
2.  Klõpsake käsku Salvesta ja kinnitage, salvestades teie tehtud muudatused.  
![graphic34][graphic34]
3.  Ühenduse test käivitub automaatselt muudatuste salvestamisel veenduge, et see on edukalt möödas.
4.  Jätkake osa 4.

###<a name="power-bi"></a>Power BI
1.  Klõpsake käsku pikenda luba:  
* ![graphic35][graphic35]
* Saate pärast kinnitust:  
* ![graphic36][graphic36]
2.  Klõpsake käsku Salvesta ja kinnitage, salvestades teie tehtud muudatused.  
![graphic37][graphic37]
3.  Ühenduse testi käivitub automaatselt muudatuste salvestamisel, veenduge, et see on edukalt möödas.
4.  Jätkake osa 4.

###<a name="sql-database"></a>SQL-andmebaas
1.  Otsimine väljad kasutajanimi ja parool ja kleepige need oma äsja loodud mandaadikomplekt:  
![graphic38][graphic38]
2.  Klõpsake käsku Salvesta ja kinnitage, salvestades teie tehtud muudatused.  
![graphic39][graphic39]
3.  Ühenduse testi käivitub automaatselt muudatuste salvestamisel veenduge, et see on edukalt möödas.  
4.  Jätkake osa 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Osa 4: Oma töö alates viimast peatatud
1.  Liikuge sisend/väljund eemale.  
![graphic40][graphic40]
2.  Klõpsake käsku Käivita.  
![graphic41][graphic41]
3.  Valige viimast lõpetanud ja klõpsake nuppu OK.  
 ![graphic42][graphic42]
4.  Jätkake osa 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Osa 5: Eemaldamine vana mandaadikomplekt
See osa on rakendatav sisendeid/väljundeid:
* Bloobimälu
* Sündmuse jaoturi
* SQL-andmebaas
* Tabelimälu

###<a name="blob-storagetable-storage"></a>Salvestusruumi/tabeli bloobimälu
Mis on varem kasutanud oma töö pikendamiseks nüüd kasutamata kiirklahv Korrake 1 osa.

###<a name="event-hubs"></a>Sündmuse jaoturi
Korrake 1 osa varem kasutatud tööpäevad pikendamiseks nüüd kasutamata võti võti.

###<a name="sql-database"></a>SQL-andmebaas
1.  Minge tagasi päringuakna osa 1 juhisega 7 ja tippige järgmine päring, asendades < previous_login_name > Kasutaja nimi, mida teie töö varem kasutatud kaudu:  
`DROP LOGIN <previous_login_name>`  
2.  Klõpsake nuppu Käivita:  
    ![graphic43][graphic43]  

Saate tuleks järgmist kinnitust: 

    Command(s) completed successfully.

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
