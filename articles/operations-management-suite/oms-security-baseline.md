<properties
   pageTitle="Toimingute haldus komplekti turvalisus ja Audit lahenduse võrdlusalus | Microsoft Azure'i"
   description="Selles dokumendis selgitatakse, kuidas kasutada OMS turvalisus ja Audit lahenduse teha võrdlusalus assessment jälgida kõik arvutid nõuetele vastavus ja turvalisuse eesmärgil."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Alusjoone hindamise toimingud halduse komplekti turbe- ja auditi lahendus

Selle dokumendi aitab teil teie jälgitud turvaline seisundi [toimingud halduse komplekti (OMS) turbe- ja auditilogi lahenduse](operations-management-suite-overview.md) alusjoone võimaluste abil.

## <a name="what-is-baseline-assessment"></a>Mis on võrdlusalus hindamise?

Microsoft, koos valdkonna ja government ettevõtted kogu maailmas, määratleb Windows konfiguratsiooni, mis tähistab turvaline server juurutuste. Selle konfiguratsiooni on poliitika auditeerimissätted ja turbe sätted koos Microsofti Soovitatavad väärtused nende sätete jaoks registrivõtmed. Reeglite tuntakse turvalisuse alused. OMS turvalisus ja Audit alusjoone võimalus saate sujuvalt nõuetele vastavuse arvutite skannida. 

On kolme tüüpi reegleid:

- **Registri reegleid**: märkige registrivõtmete on õigesti seatud.
- **Auditilogi poliitika reegleid**: reeglid teie auditilogi poliitika kohta.
- **Turvalisus poliitika reegleid**: reeglid arvutis kasutaja õiguste kohta.

> [AZURE.NOTE] Lugege [Kasutamine OMS turvalisus turvalisuse konfiguratsiooni alused hindamaks](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) lühiülevaade selle funktsiooni.

## <a name="security-baseline-assessment"></a>Turvalisuse võrdlusalus hindamine

Saate vaadata oma praeguse turvalisuse alusjoone hindamise kõik arvutid, mis on jälgida OMS turbe- ja armatuurlaua Audit.  Käivita juurdepääsu turvalisus alusjoone assessment armatuurlaud järgmist:

1. Klõpsake **Microsoft Operations Management Suite** peamine armatuurlaud, **Turvalisus ja Audit** paani.
2. **Turvalisus ja Audit** armatuurlaua, klõpsake jaotises **Turvalisus domeenide** **Võrdlusalus hindamine** . **Turvalisuse alusjoone hindamise** armatuurlaua kuvatakse, nagu on näidatud järgmisel pildil:
    
    ![OMS turvalisus ja Audit alusjoone hindamine](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Armatuurlaua on jaotatud kolmes põhivaldkonnas:

- **Arvutid võrreldes võrdlusalus**: See jaotis annab Kokkuvõte arvutid, mis olid arv ja protsent arvutid, mille hinnang on möödas. Samuti annab ülemine 10 arvutit ja protsent tulemi hindamiseks.
- **Nõutav reeglid olek**: selles jaotises on tuua teadlikkuse nurjunud reeglite abil raskusaste soovidele ja nurjus reeglite tüübi järgi. Kas soovite esimese graafik saate kiiresti tuvastada kui enamik nurjunud reegleid olulised, või mitte. Samuti annab ülemine 10 nurjunud reeglid ja raskusaste loendit. Teine joonisel on nurjunud hindamise reegli tüüp. 
- **Puuduvad võrdlusalus assessment arvutites**: selles jaotises loendi arvutites, mis olid ei avata operatsioonisüsteemi ühildamatus või tõrgete tõttu. 

### <a name="accessing-computers-compared-to-baseline"></a>Juurdepääs võrreldes alusjoone arvutites

Teie arvutis asuvad kõik toimuma nõuetele vastavust turvalisuse alusjoone hindamine. Siiski eeldatakse, et teatud asjaolude see ei juhtu. Turvalisuse halduse protsessi osana on oluline läbida kõik turvalisus testide nurjus arvutites olla. Kiiresti visualiseerida, mis on, valides suvandi **arvutite juurde** asub **arvutites võrreldes võrdlusalus** osa. Peaksite nägema log otsingutulemite arvutite nagu kujutatud järgmisel kuvatõmmisel loendi nähtaval:

![Juurde pääseda arvuti tulemused](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Otsingutulemite on esitatud tabelina, kus esimene veerg on arvuti nimi ja teise värvi on reeglid, mida ei saanud arv. Reegel, mida ei saanud tüüpi teavet, klõpsake nuppu arv nurjunud reeglite peale arvuti nimi. Peaksite nägema tulemus on näidatud järgmisel pildil sarnane:

![Juurde pääseda arvuti tulemusi üksikasjad](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Otsingu tulemis teil kokku juurde eeskirjad, kriitiliste reeglid, mida ei saanud arvu, hoiatus ja teave nurjus reegleid.

### <a name="accessing-required-rules-status"></a>Juurdepääs nõutud reeglid olek

Pärast protsent arvu arvutid, et möödunud hindamise kohta teabe saamiseks, soovite saada lisateavet kohta, mis ei suuda reeglite kohaselt määravat. See visualiseerimise abil saate eriti, millises arvutis tuleb esmalt tegeleda neid järgmise hindamisel nõuetele vastavuse tagamiseks. Viige kursor asub **nurjus reeglite abil raskusaste** paani jaotises **nõutav reeglid olek** graafik kriitiline osa ja klõpsake seda. Peaksite nägema tulemus on sarnane järgmisel kuvatõmmisel:

![Raskusaste andmed reeglid nurjus](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

Log tulem kuvatakse tüüpi võrdlusalus reegel, mida ei saanud selle reegli kirjeldust ja selle reegli turvalisus levinud konfiguratsiooni loendamine (CCE) ID-d. Neid atribuute peaks olema piisa probleemi lahendamiseks arvuti target parandamise toimingu tegemiseks.

> [AZURE.NOTE] Lisateavet CCE [Liikmesriikide haavatavuse andmebaasi](https://nvd.nist.gov/cce/index.cfm)juurdepääsu.

### <a name="accessing-computers-missing-baseline-assessment"></a>Juurdepääs arvutite puuduvad võrdlusalus hindamine

OMS toetab Windows Server 2008 R2 kuni Windows Server 2012 R2 domeeni liikme võrdlusalus profiil. Windows Server 2016 alusjoone pole veel lõplik ja lisatakse kohe, kui see on avaldatud. Kõik muud opsüsteemid skannitud OMS turvalisus ja Audit võrdlusalus hindamise kaudu kuvatakse jaotises **puuduvad võrdlusalus assessment arvutites** .

## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut OMS turvalisus ja Audit võrdlusalus hindamise kohta. OMS turvalisuse kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Toimingute haldus komplekti (OMS) ülevaade](operations-management-suite-overview.md)
- [Jälgimine ja toimingute haldus komplekti turvalisus ja Audit lahenduse Turbeteatiste reageerimine](oms-security-responding-alerts.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse jälgimisega seotud ressursid](oms-security-monitoring-resources.md)

