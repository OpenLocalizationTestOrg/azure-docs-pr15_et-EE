<properties
pageTitle="Seadme õ asutusesisese SQL serveri andmebaasis asuvate andmete kasutamine | Azure'i"
description="Kasutage asutusesisese SQL serveri andmebaasi andmete täiustatud analüüsi Azure seadme õppimisega."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Täiustatud analüüsi täitma Azure seadme õ asutusesisese SQL serveri andmebaasis asuvate andmete kasutamine


Sageli kohapealse andmetega töötamine suurettevõtete soovite ära skaala ja agility pilveteenuste oma masina Õppekeskuse töökoormus. Kuid nad ei taha häirida oma praeguse äriprotsesside ja töövood, teisaldades oma kohapealse andmete pilve. Azure'i masina õ toetab nüüd asutusesisese SQL serveri andmebaasi andmete lugemine ja seejärel koolitus ja neid andmeid mudel hinded. Teil pole enam käsitsi kopeerimine ja pilveteenuses ja teie asutusesisese serveri vaheline andmete sünkroonimine. Selle asemel **Andmete importimine** mooduli Azure seadme õ Studios saate nüüd lugeda otse asutusesisese SQL Serveri andmebaasiga treeningu ja hinded tööde haldamine. 

Selles artiklis antakse ülevaade sellest, kuidas sissepääsu asutusesisese SQL serveri andmetega Azure seadme õ sisse. Eeldatakse, et olete tuttav Azure seadme õ mõisted nagu *tööruumid, moodulid andmekomplektide, katsete jne*.

> [AZURE.NOTE] See funktsioon pole saadaval tasuta tööruumid. Seadme õ hinnad ja astme kohta leiate lisateavet teemast [Azure seadme õ hinnad](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Installige Microsofti Andmehalduslüüs

Asutusesisese SQL serveri andmebaasis Azure seadme õ juurdepääsuks peate alla laadima ja installima Microsoft Andmehalduslüüsi. Seadme õ Studio ühenduse lüüs konfigureerimisel peate alla laadima ja installima **alla laadida ja register andmete lüüsi** dialoogiboksis allpool kirjeldatud lüüsi võimalus.

Samuti saate installida ette valmistada Andmehalduslüüsi alla laadida ja käivitada MSI installipaketi [Microsofti allalaadimiskeskusest](https://www.microsoft.com/download/details.aspx?id=39717). Valige Office'i uusim versioon, valiku 32-bitine või 64-bitine vastavalt teie arvutis. MSI saate kasutada ka olemasoleva Andmehalduslüüsi uusima versiooni täiendamine alles kõik sätted.

Lüüs on järgmine kohustuslik tarkvara.

- Toetatud Windowsi operatsioonisüsteem on Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 ja Windows Server 2012 R2.
- Soovitatav konfiguratsioon lüüsi masina on vähemalt 2 GHz 4 tuuma, 8 GB RAM-i ja 80 GB vaba.
- Kui majutusseadme talveunne ei saa lüüsi andmete vastata. Seetõttu konfigureerimine on sobiv energiarežiim arvutis enne installimist lüüsi. Lüüsi installimise kuvab teate, kui seade on konfigureeritud talveunerežiimi.
- Kuna Kopeeri toimub kindla sagedusega, järgib ressursikasutuse (CPU, mälu) seadmesse ka sama mustriga tippväärtus ja jõude korda. Ressursi kasutamine ka sõltub andmehulga ei teisaldataks. Kui mitme eksemplari töö on juba märkate ressursikasutus tippväärtus aegadel minema. Ülaltoodud minimaalne konfiguratsioon on tehniliselt piisavalt, võite on rohkem kui minimaalne konfiguratsioon, sõltuvalt teie andmete liikumine kindla laadimine ressursse.

Kui häälestamise ja kasutamise Andmehalduslüüsi järgmine tuleks arvesse võtta.

- Saate installida ainult ühe eksemplari Andmehalduslüüsi ühes arvutis.
- Saate ühe lüüsi mitme kohapealse andmeallikaga.
- Mitme lüüsi erinevates arvutites saate luua ühenduse sama kohapealse andmeallika.
- Saate konfigureerida lüüsi ainult ühte tööruumi korraga. Lüüside ei saa ühiskasutusse anda kogu tööruumide sel ajal.
- Mitme lüüsi ühe tööruumi saate konfigureerida. Näiteks võite kasutada testi andmeallikate arendamise käigus ühendatud lüüsi ja tootmise lüüsi, kui olete valmis käivitama.
- Lüüsi ei pea olema samasse arvutisse andmeallikana, kuid püsimine lähemale andmeallika vähendab andmeallikaga ühenduse lüüs kestuse. Soovitame installida seadmesse, mis on sama mis hostib kohapealse andmeallika nii, et lüüsi ja andmeallikat ei võistlema ressursid lüüsi.
- Kui teil on juba lüüsi serveeritakse Power BI või Azure'i andmed Factory stsenaariumid teie arvutisse installitud, eraldi lüüsi Azure seadme õppe mõnda teise arvutisse installida. 

    > [AZURE.NOTE] Samas arvutis ei saa käivitada Andmehalduslüüsi ja Power BI lüüsi.

- Peate kasutama Andmehalduslüüsi Azure seadme õppe, isegi juhul, kui kasutate Azure ExpressRoute muudele andmetele. Andmeallika käsitlema kohapealse andmeallika (see on tulemüüriga) isegi kui kasutate ExpressRoute, ja kasutada Andmehalduslüüsi Ühenduvus masina õppimine ja andmeallika loomiseks. 

Leiate üksikasjalikku teavet installimise eeltingimused, installimine juhiseid ja tõrkeotsingu näpunäiteid artiklist, [asutusesisestest allikatest ja pilve Andmehalduslüüsi andmete teisaldamine](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), alustades jaotises [teave kasutamise Andmehalduslüüsi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Sissepääsu andmed üheks Azure seadme õ asutusesisese SQL Serveri andmebaasiga


Ülevaate, on häälestatud Andmehalduslüüsi mõne Azure seadme õ tööruumi, seadistage see ja seejärel andmeid lugeda asutusesisese SQL serveri andmebaasi.

> [AZURE.TIP] Enne alustamist keelake brauseri hüpikakende blokeerija `studio.azureml.net`. Kui kasutate Google Chrome'i brauseris, alla laadida ja installida ühte mitu lisandmoodulid saadaval Google Chrome'i veebipoodi [Klõpsake üks kord rakenduse laiend](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Samm 1: Loo lüüsi

Esimene samm on loomine ja häälestamine lüüsi teie asutusesisese SQL-i andmebaasile juurde pääsemiseks.

1. Logi [Azure seadme õ Studio](https://studio.azureml.net/Home/) ja valige tööruum, mida soovite töötada.

2. Klõpsake **sätete** tera vasakul ja seejärel klõpsake vahekaarti **Andmed LÜÜSIDE** ülaosas.

3. Klõpsake ekraani allservas **Uute andmete LÜÜSI** .

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. **Uute andmete lüüsi** dialoogiboksis Sisestage **Lüüsi nimi** ja lisage soovi korral **Kirjeldus**. Klõpsake alumises parempoolses nurgas konfiguratsiooni järgmise juhise juurde liikumiseks klõpsake noolt.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Alla laadida ja register andmete lüüsi dialoogiboksis kopeerimine lõikelauale registreerimise LÜÜSIVÕTIT.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Kui te pole veel alla laadinud ja installinud Microsoft Andmehalduslüüsi, klõpsake nuppu **andmehalduslüüsi alla laadida**. See viib teid Microsoft Download Center, kus saate valida lüüsi versiooni, peate selle alla laadida, ja installige see. [Asutusesisestest allikatest ja pilve Andmehalduslüüsi andmete teisaldamine](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)artikli alguses jaotiste leiate üksikasjalikku teavet installimise eeltingimused, installimine juhiseid ja tõrkeotsingu näpunäiteid.

7. Pärast installimist lüüsi soovitud andmed Andmehalduslüüsi Konfigureerimishalduri avatakse ja kuvatakse dialoogiboks **lüüsi registreerida** . Kleepige lõikelauale kopeeritud **Lüüsi registreerimise võti** ja nuppu **registreerida**.

8. Kui teil on juba installitud lüüsi, käivitada soovitud andmete Andmehalduslüüsi Konfigureerimishalduri, klõpsake nuppu **Muuda tootenumber**, kleepige lõikelauale kopeeritud  **Lüüsi registreerimise võti** ja nuppu **OK**.

9. Kui installimine on lõpule jõudnud, kuvatakse dialoogiboks **lüüsi registreerida** for Microsoft andmete Andmehalduslüüsi Konfigureerimishalduri. Kleepige ülaltoodud lõikelauale kopeeritud LÜÜSIVÕTIT registreerimist ja klõpsake nuppu **Registreeri**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Lüüsi konfigureerimine on lõpule jõudnud, kui menüü **Avaleht** jaotises rakenduses Microsoft andmete Andmehalduslüüsi Konfigureerimishalduri on seatud järgmised väärtused:

    - **Lüüsi nimi** ja **eksemplari nimi** on seatud lüüsi nimi.

    - **Registreerimise** on seatud **registreeritud**.

    - **Olekuks** on seatud **käivitatud**.

    - Allosas olekuribal kuvatakse **ühendatud andmete Management Gateway pilveteenusesse** koos roheline märge.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure'i masina õ Studio ka värskendamist, kui registreerimine õnnestus.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. **Laadige alla ja andmete lüüsi registreerida** dialoogiboksis nuppu märge häälestamise lõpuleviimiseks. **Sätete** lehe kuvab lüüsi olek "Online". Paremal paanil leiate olek ja muud kasulikku teavet.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Microsoft andmete Andmehalduslüüsi Konfigureerimishalduri **serdi** vahekaardi aktiveerimine. Sellel vahekaardil määratud serdi kasutatakse krüptimine/dekrüptimine kohapealse andmesalve portaalis teie määratud mandaat. See on vaikimisi loodud sert. Microsoft soovitab, muutes selle oma sert, mida teie serdi haldus süsteemis varundamine. Klõpsake nuppu **Muuda** enda asemel kasutada.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (valikuline) Kui soovite lubada Paljusõnaline logimine lüüsi probleemide tõrkeotsinguks, klõpsake soovitud Microsoft andmete Andmehalduslüüsi Konfigureerimishalduri **diagnostika** vahekaardi aktiveerimine ja märkige ruut **Luba tõrkeotsingu eesmärgil Paljusõnaline logimine** . Logiteave leiate Windowsi Sündmusevaatur **rakenduste ja teenuste logid**  - &gt; **Andmehalduslüüsi** sõlm. Saate vahekaardi **diagnostika** abil lüüsi kohapealse andmeallikaga ühenduse testimiseks.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

See lõpetab lüüsi protsessi Azure seadme õppe häälestamine.
Nüüd oletegi valmis kasutama oma kohapealse andmed.

Saate luua ja häälestamine mitme lüüsi Studios iga tööruumi. Näiteks võib olla lüüsi, mida soovite oma testi andmeallikatega ühenduse arendamise käigus ja muu lüüsi oma tootmise andmeallikaid. Azure'i masina õ kaudu saate luua mitme lüüsi, olenevalt sellest, kas teie ettevõtte keskkonnas. Praegu ei saa lüüsi vahel tööruumide ühiskasutusse anda ja ühte arvutisse saab installida ainult üks lüüsi. Veel kaalutlused lüüsi installimisel, vaadake [teave kasutamise Andmehalduslüüsi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) artiklis [andmete asutusesisestest allikatest ja Andmehalduslüüsi pilve vahel liikumine](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Samm 2: Lüüsi abil kohapealse andmeallika põhjal andmete lugemine

Kui olete häälestanud lüüsi, saate lisada katse, mis sisestab andmeid asutusesisese SQL Serveri andmebaas on **Andmete importimine** mooduli.

1.  Seadme õ Studios, valige menüü **katsete** , valige **+ Uus** vasakus ülanurgas ja valige **Tühi katse** (või loendist mõni mitu valimi katsete saadaval).

2.  Otsimine ja lohistage **Andmete importimine** katse lõuend.

3.  Lõuendi all nuppu **Salvesta nimega** . Sisestage "Azure seadme õ asutusesisese SQL serveri õpetus" katse nimi, valige tööruumi ja klõpsake **OK** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  **Andmete importimine** mooduli selle valimiseks klõpsake ja valige lõuend paremal paanil **Atribuudid** "Asutusesisese SQL-andmebaasi" **andmeallika** rippmenüüst.

5.  Valige **andmete lüüsi** olete installinud ja registreeritud. Saate häälestada teise lüüsi, valides "(Lisa uus andmete lüüsi...)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Sisestage SQL-i **Andmebaasiserveri nimi** ja **andmebaasi nimi**, mida soovite käivitada SQL- **andmebaasi päringu** .

7.  Klõpsake jaotises **kasutajanimi ja parool** **Sisestage väärtused** ja sisestage mandaat andmebaasi. Saate kasutada Windowsi integreeritud autentimist või SQL serveri autentimine, sõltuvalt sellest, kuidas teie asutusesisese SQL Server on konfigureeritud.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Teade "väärtused, mis on nõutavad" muutub "väärtuste häälestada" roheline märge. Peate lihtsalt sisestage mandaat üks kord, kui muudab andmebaasi teave või parool. Azure'i masina õ kasutab registreerimisel installisite pilveteenuses mandaadi krüptimiseks lüüsi sert. Azure'i kunagi salvestab kohapealse identimisteabe krüptimata.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klõpsake nuppu **Käivita** katse käivitamiseks.

Kui katse lõpetab töötab teie saate visualiseerida, klõpsates väljund porti **Andmete importimine** mooduli ja valides **Visualiseeri**andmebaasist imporditud andmed.

Kui olete lõpetanud, arendada oma katse, saate juurutada ja kiireks mudelisse. Paketi täitmise teenuse kasutamise asutusesisese SQL serveri andmebaasi **Andmete importimine** mooduli konfigureeritud andmeid lugeda ja kasutada hinded. Microsoft soovitab ajal taotleda vastuse teenuse abil saate kohapealsed andmed hinded, hoopis [Exceli lisandmooduli](machine-learning-excel-add-in-for-web-services.md) abil. Praegu asutusesisese SQL serveriga kirjutamine andmebaasi **Andmete eksportimine** kuni ei toetata teie katsete või avaldatud veebiteenused.

