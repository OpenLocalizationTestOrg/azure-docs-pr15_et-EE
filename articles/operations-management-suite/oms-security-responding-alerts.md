<properties
   pageTitle="Jälgimine ja reageerimine Turbeteatiste toimingud halduse komplekti turvalisus ja auditi lahenduse | Microsoft Azure'i"
   description="Selle dokumendi aitab teil saadaval OMS turvalisus ja Audit ohtu Ajateabe suvandi abil saate jälgida ja turbeteatised vastata."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Jälgimine ja turbeteatised toimingud halduse komplekti turvalisus ja Audit lahenduse reageerimine

Selle dokumendi aitab teil saadaval OMS turvalisus ja Audit ohtu ärianalüüsi suvandi abil saate jälgida ja turbeteatised vastata.

## <a name="what-is-oms"></a>Mis on OMS?

Microsoft toimingute komplekti (OMS) on Microsofti pilveteenuste vastavalt selle lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu. OMS kohta lisateabe saamiseks lugege artiklit [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Ohtu ärianalüüsi

Ettevõtte keskkonnas, kus kasutajad on võrku ühendatud ja kasutada erinevaid seadmeid ettevõtte andmetega ühenduse loomiseks, on aktiivselt jälgida oma ressursse ja turvalisus juhtumite kiirelt vastata. See on eriti oluline turvalisus elutsükli Kuna mõned küberjulgeoleku ohtude ei tohi võtta teatiste või kahtlaste tegevused, mida saab traditsiooniline turbemeetmed tehnilise kindlaks teha. 

**Ohtu ärianalüüsi** suvandi saadaval OMS turvalisus ja Audit abil IT-administraatorid tuvastada turvalisus ohtude vastu keskkonnas, näiteks, tehke kindlaks, kas teatud arvuti on [bot](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Arvutisse võib muutuda aeglaseks sõlmed installimisel ründajad salvestab ründevara, mis ühendab selle arvuti salaja käsud ja juhtimine. Samuti saate kindlaks määrata pärit maa side kanaleid, nt [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents)võimalike ohtudega. 

See ohtu Ajateabe koostamiseks kasutada OMS turvalisus ja Audit jooksul Microsoft erinevatest allikatest pärit andmete. OMS turvalisus ja Audit kogusummaks andmete tuvastamiseks võimalike ohtudega keskkonna suhtes.

Paani ohtu ärianalüüsi koosneb kolm põhilist võimalust:
- Pahatahtlik väljaminev liiklus serverite
- Tuvastatud ohud tüübid
- Ohtu ärianalüüsi vastendust

> [AZURE.NOTE] Kõik need suvandid ülevaate lugeda [toimingute haldus komplekti turvalisus ja Audit lahenduse töötamise alustamine](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Turbeteatiste reageerimine

Üks juhiseid [Turvalisus langeva vastuse](https://technet.microsoft.com/library/cc512623.aspx) protsess on kompromiss süsteemid tõsidust tuvastamiseks. Selles etapis peaks tegema järgmist:

- Laadi rünnak
- Määratakse rünnak punkt päritolu
- Määratlege rünnak soovidele. Oli rünnak suunatud teie asutuses hankida kindla teabe või seda juhusliku?
- Tuvastada süsteemide, mis on rikutud
- Failid, mis on külastatud ja määrata need failid tundlikkust tuvastamine

Saate kasutada **Ohtu** teabe OMS turvalisus ja Audit lahenduse järgmised toimingud aidata. Allpool selle **Ohtu Ajateabe** suvanditele juurdepääsuks tehke järgmist.

1. Klõpsake **Microsoft Operations Management Suite** peamine armatuurlaud **Turvalisus ja Audit** paani.

    ![Turvalisus ja Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. **Turvalisus ja auditi** armatuurlaual kuvatakse **Ohtu ärianalüüsi** suvandid paremas, nagu allpool näidatud:

    ![Ohtu Inteli](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Need kolm paanid annab teile ülevaate praeguse ohtude. **Pahatahtlik väljaminev liiklus serveris** saab kindlaks teha, kui seal on kõik arvutid, et olete järelevalve (sees või väljaspool teie võrku) mis saadab pahatahtlik liikluse Interneti-ühendus. 

Paani **avastati ohtu tüübid** kuvatakse kokkuvõte ohtude, mis on praeguse "looduses", kui klõpsate sellel paanil kuvatakse täpsemat teavet nende ohtudega nimega Näita allpool:

![Tuvastati ohtu tüübid](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Klõpsates saate rohkem teavet iga ohtu ekstraktida. Järgmises näites on kuvatud bot kohta rohkem üksikasju:

![Täpsemat teavet ohtu](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Alguses selles jaotises kirjeldatud see teave võib olla väga kasulik on langeva vastuse juhul ajal. See ka saab oluline kohtuekspertiisi käigus, kuhu soovite leida rünnak, milline oli rikutud ja ajaskaala. Selles aruandes saate kerge vaevaga tuvastada mõningaid olulisi andmeid infarkti, näiteks: allikas rünnak, kohalik IP, mis on rikutud ja praeguse seansi ühenduse olek. 

**Ohtu ärianalüüsi vastendust** aitavad teil tuvastada praeguse asukohad kogu maailmas, mis on pahatahtlik liiklus. Seal on oranž (sissetulev) ja (väljamineva) nooled selles mapis tuvastavat liikluse suunda, kui klõpsate ühel noolte, kuvatakse see tüüp ja liikluse suunas, nagu allpool näidatud:

![ohtu Inteli kaart](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Saate vaadata selle funktsiooni kasutamise ajal juhtum tutvustava vastuse protsessi, jälgides esitluse [Mitigate andmekeskuse turvalisus ohtude juhendavat uurimise Operations Management Suite abil](https://myignite.microsoft.com/videos/5000) , mis on esitatud veebisaidil Microsoft Ignite.

## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut vastata Turbeteatiste **Ohtu ärianalüüsi** suvand OMS turvalisus ja Audit lahenduse kasutamise kohta. OMS turvalisuse kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Toimingute haldus komplekti (OMS) ülevaade](operations-management-suite-overview.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse töötamise alustamine](oms-security-getting-started.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse jälgimisega seotud ressursid](oms-security-monitoring-resources.md)
