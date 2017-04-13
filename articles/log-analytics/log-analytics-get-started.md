<properties
    pageTitle="Alustamine Log Analytics | Microsoft Azure'i"
    description="Saate avada ja töötab Analytics Logi sisse selle Microsofti toimingute komplekti (OMS) minutites."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Log Analyticsi kasutamise alustamine

Saate avada ja töötab Analytics Logi sisse selle Microsofti toimingute komplekti (OMS) minutites. Kuidas luua mõne OMS tööruumi, mis on sarnane konto valimisel on teil kaks võimalust:

- Veebisaidi Microsoft Operations Management Suite
- Microsoft Azure'i tellimus

Saate luua mõne tasuta OMS tööruumi OMS veebisaidi kaudu. Või Microsoft Azure'i tellimuse abil saate luua mõne OMS tööruumi. Nii tööruumid võrdväärse, välja arvatud, et tasuta OMS tööruumi saate saata ainult 500 MB andmeid iga päev OMS teenusega. Kui kasutate Azure tellimuse, saate selle tellimuse muude Azure'i teenustele juurdepääsuks. Tööruumi loomiseks kasutatav meetod, olenemata saate luua tööruumi Microsofti konto või ettevõttekonto abil.

Siin on ülevaade protsessi:

![diagrammi kasutuselevõtt](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Logige Analytics eeltingimused ja juurutamise kaalutlused

- Peate kasutama täielikku Log Analytics Microsoft Azure'i tasuliseks tellimuseks. Kui teil pole Azure tellimuse, luua [tasuta konto](https://azure.microsoft.com/free/) , mis võimaldab juurdepääsu mis tahes Azure'i teenus. Või saate luua tasuta OMS konto [Operations Management Suite](http://microsoft.com/oms) veebisaidil ja klõpsake nuppu **Proovi tasuta**.
- Mõne OMS tööruumi
- Iga Windowsi arvutisse, kuhu soovite koguda andmeid tuleb käitada Windows Server 2008 hoolduspaketi SP1 või uuem
- [Tulemüüri](log-analytics-proxy-firewall.md) juurdepääsu soovitud OMS web teenuse aadressid
- Ümber suunama liikluse serverid OMS, kui Interneti-ühendus pole saadaval arvutites serveriks [OMS Log Analytics ekspediitor](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (lüüs)
- Kui kasutate Toiminguhalduri, Log Analytics toetab toimingute Manager 2012 SP1 UR6 ja kohale ja toimingute Manager 2012 R2 UR2 ja ülaltoodud. Puhverserveri tugi on lisatud toimingute Manager 2012 SP1 UR7 ja toimingute Manager 2012 R2 UR3. Määratlege, kuidas see integreeritud OMS.
- Määratleda, kas teie arvutisse on otsene Interneti-ühendus. Kui pole, nad nõuda lüüsi serveri juurdepääsu OMS veebisaitide teenus. Kõigi juurdepääs on HTTPS-i kaudu.
- Kindlaks teha, millised tehnoloogiad ja serverid saadab andmeid OMS. Näiteks domeenikontrollerid, SQL Server, jne.
- Anda õiguse OMS ja Azure kasutajad.
- Kui olete muretsema andmete kasutamine, iga lahenduse juurutamine eraldi ja jõudluse mõju testida enne lisamist täiendavaid lahendusi.
- Vaadake oma andmekasutuse ja jõudluse lisamisel Log Analytics lahenduste ja funktsioonid. See hõlmab sündmuse saidikogumi, Logi saidikogumi jõudluse andmete kogumine jne. See on parem alustada minimaalsete saidikogumi kuni andmekasutuse või jõudluse mõju on tuvastatud.
- Veenduge, et Windows agentide pole ka hallatakse Toiminguhalduri, duplikaatandmete vastasel. See kehtib ka Azure-põhiste-agentide mis on Azure diagnostika lubatud.
- Agentide installimist veenduge, et agent töötab õigesti. Kui ei, märkige ruut tagamaks, et krüptograafia API: järgmise genereerimine (CNG) klahvi eraldamise on keelatud, kasutades rühmapoliitika.
- Mõned Log Analytics lahendused on lisanõuded



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>3 sammu abil Operations Management Suite registreerumine

1. Minge veebisaidile [Operations Management Suite](http://microsoft.com/oms) ja klõpsake nuppu **Proovi tasuta**. Logige sisse oma Microsofti kontoga, nt Outlook.com või ettevõtte konto teie ettevõtte või haridusasutuse Teenusekomplektis Office 365 või muude Microsofti teenuste kasutamiseks.
2. Sisestage kordumatu tööruumi nimi. Tööruum on loogiline ümbris halduse andmete talletuskoht. See pakub teile võimalus sektsiooni andmed erinevate teie asutuse meeskondade vahel on andmed ainult selle tööruumi. Määrake e-posti aadressi ja piirkond, kuhu soovite oma talletatud andmed.  
    ![tööruumi loomine ja tellimuse link](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Järgmisena saate luua uue Azure tellimuse või lingi Azure olemasolevasse tellimusse. Kui soovite jätkata, tasuta prooviversiooni abil, klõpsake nuppu **Pole kohe**.  
  ![tööruumi loomine ja tellimuse link](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

Olete valmis alustada portaali Operations Management Suite.

Lisateavet leiate teemast oma tööruumi häälestamise ja linkimise kohta olemasoleva Azure'i kontod tööruumidesse loodud Operations Management Suite veebisaidil [Log Analytics juurdepääsu haldamine](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Registreerumine abil kiiresti Microsoft Azure'i

1. [Azure portaali](https://portal.azure.com) sisse logida, sirvige teenuste loend ja valige **Logi Analytics (OMS)**.  
    ![Azure'i portaal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klõpsake nuppu **Lisa**ja seejärel valige järgmiste üksuste võimalusi.
    - **OMS tööruumi** nimi
    - **Tellimuse** – kui teil on mitu tellimust, valige soovitud seade, mida soovite uue tööruumi seostada.
    - **Ressursirühm**
    - **Asukoht**
    - **Esimese taseme hinnad**  
        ![kiire loomine](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klõpsake nuppu **Loo** ja näete tööruumi üksikasjad Azure'i portaalis.       
    ![tööruumi üksikasjad](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Saate avada Operations Management Suite veebisaidi koos oma uue tööruumi linki **OMS portaalis** .

Olete valmis alustama Operations Management Suite portaalis.

Lugege lisateavet oma tööruumi häälestamise ja olemasoleva tööruumid, mida olete loonud Operations Management Suite Azure tellimuste [haldamine juurdepääsu Log Analytics](log-analytics-manage-access.md)veebisaidil linkimise kohta.

## <a name="get-started-with-the-operations-management-suite-portal"></a>Operations Management Suite portaali kasutamise alustamine
Valige lahenduste ja ühendada serverid, mida soovite hallata, klõpsake paani **sätted** ja järgige selles jaotises toodud juhised.  

![Alustamine](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Lahenduste lisamine** – saate vaadata oma installitud lahendusi.  
    ![lahendused](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klõpsake **Galerii külastage** lisada veel lahendusi.  
    ![lahendused](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Valige lahenduse ja seejärel klõpsake nuppu **Lisa**.
2. **Ühenduse loomine allika** - valige, kuidas soovite oma serveri keskkonnas koguda andmeid ühenduse loomiseks:
    - Ühenduse loomine mis tahes Windows Server või kliendi otse installides agent.
    - Ühendage Linux serverid OMS Agent Linuxi jaoks.
    - Kasutage konfigureeritud laiendiga Windowsi või Linuxi Azure diagnostika VM Azure storage konto.
    - System Center Operations Manager abil saate lisada oma halduse rühma või kogu Toiminguhalduri juurutamise.
    - Windowsi telemeetria kasutada Analytics versiooniuuenduse lubamine.
        ![ühendatud allikad](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Andmete kogumine** Konfigureerige tööruumi andmetega asustada vähemalt ühe andmeallika. Kui lõpetanud, klõpsake nuppu **Salvesta**.    

    ![andmete kogumine](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>Soovi korral saate ühendada serverid otse Operations Management Suite, installides agent

Järgmises näites näitab, kuidas installida Windows agent.

1. Klõpsake paani **sätted** , klõpsake vahekaarti **Ühendatud allikad** , klõpsake mõnda vahekaarti andmeallika tüüp, mille soovite lisada, ja kas alla laadida agent või teada, kuidas lubada agent. Näiteks klõpsake **Alla laadida Windows Agent (64-bitine)**. Windowsi töötajatele, saate installida ainult agent Windows Server 2008 Hoolduspaketiga SP1 või uuem versioon või Windows 7 SP1 või uuem versioon.
2. Ühe või mitme serveri agent installida. Saate installida agentide ükshaaval või [kohandatud skript](log-analytics-windows-agents.md), või rohkem automatiseeritud meetodi abil saate kasutada olemasoleva tarkvara jaotuse lahenduse, mis võib olla.
3. Kui nõustute litsentsilepinguga ja valida oma installikaust, valige **Ühenda agent Azure Log Analytics (OMS)**.   
    ![agent häälestamine](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Järgmisel lehel küsitakse teie tööruumi ID ja tööruumi võti. Kui allalaaditud faili agent ekraanil kuvatakse teie tööruumi ID ja võti.  
    ![agent võtmed](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![serverid manustamine](./media/log-analytics-get-started/oms-onboard-key.png)
5. Installimise ajal saate klõpsata **Täpsemalt** võite häälestada oma puhverserveri ja autentimise teavet. Klõpsake tööruumi teabe kuvale naasmiseks nuppu **edasi** .
6. Klõpsake nuppu **edasi** domeenist oma tööruumi ID ja võti. Kui leitakse vigu, võite klõpsata **tagasi** paranduste tegemine. Oma tööruumi ID ja võti on kinnitatud, klõpsake nuppu **installida** agent installimise lõpuleviimiseks.
7. Klõpsake juhtpaneelil käsku Microsoft Agenti jälgimine > Azure'i Log Analytics (OMS) vahekaarti. Roheline märge ikoon kuvatakse, kui agentide suhelda Operations Management Suite teenuse. Esialgu see võtab aega umbes 5 – 10 minutit.

>[AZURE.NOTE] Võimsus haldus ja konfiguratsiooni assessment lahendused ei toeta praegu ühendatud otse Operations Management Suite serverid.


Samuti saate luua ühenduse agent System Center toimingute Manager 2012 SP1 ja uuemad versioonid. Selleks valige **Ühenda System Center Operations Manager agent**. Kui valite suvand andmete teenusega ei ole vaja täiendavat riistvara või saadate koormus halduse rühm.

Lugege veebisaidil [ühenduse loomine Windows arvutite Log Analytics](log-analytics-windows-agents.md)Operations Management Suite agentide ühenduse loomise kohta.

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Soovi korral saate ühendada serverid System Center Operations Manager abil

1. Toiminguhalduri konsooli, valige **haldus**.
2. Laiendage **Töö ülevaated** ja valige **Funktsionaalseid ülevaateid ühendus**.

  >[AZURE.NOTE] Sõltuvalt sellest, millist Update Rollup, SCOM kasutate, võidakse kuvada sõlm *System Center Advisor*, *Töö ülevaated*või *Operations Management Suite*.

3. Suunas paremas ülanurgas linki **registreerida tegevuse ülevaated** ja järgige juhiseid.
4. Pärast viisardi registreerimine, linki **Lisa arvuti/rühm** .
5. Dialoogiboks **Otsing arvuti** saate otsida arvutites või rühmade Toiminguhalduri jälgida. Valige arvutites või rühmi, et pardal need Log Analytics, klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **OK**. Saate kontrollida OMS teenuse saab andmete **kasutus** paan Operations Management Suite portaali kaudu. Andmed kuvatakse umbes 5 – 10 minutit.

Lugege veebisaidil [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md)Operations Management Suite Toiminguhalduri ühenduse loomise kohta.

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Soovi korral võite analüüsimiseks Microsoft Azure pilveteenused

Operations Management Suite, saate kiiresti otsida sündmus ja IIS-i logides pilveteenustega ja virtuaalmasinates, võimaldades diagnostika puhul pilveteenuste Azure. Samuti saate täiendavaid ülevaateid jaoks oma Azure'i virtuaalmasinates installite Microsoft Agenti jälgimine. Lugege lisateavet selle kohta, kuidas konfigureerida Azure keskkonna kasutada Operations Management Suite [ühenduse Azure storage Log Analytics abil](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Järgmised sammud

- Funktsioonide lisamine ja andmete kogumine [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .
- Saate tutvuda [log otsingud](log-analytics-log-searches.md) üksikasjaliku teabe kogutud lahendusi.
- [Armatuurlaudade](log-analytics-dashboards.md) abil saate salvestada ja kuvada kohandatud otsingusse.
