<properties
   pageTitle="Azure'i turbekeskus kasutamise langeva vastuse | Microsoft Azure'i"
   description="Selles dokumendis selgitatakse, kuidas kasutada Azure turbekeskus on langeva vastuse stsenaarium."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Azure'i turbekeskus kasutamise langeva vastuse
Paljud organisatsioonid saate teada, kuidas vastata turvalisus juhtumite alles pärast rünnaku all. Kulude ja kahjude vähendamiseks on oluline, et leping kohas enne rünnaku langeva vastuse. Saate kasutada Azure turbekeskus eri langeva vastuse.

## <a name="incident-response-planning"></a>Langeva vastuse kavandamine

Efektiivse tegevuskava sõltub kolme core võimaluste: on võimalik, et kaitsta, tuvastada ja ohtude vastata. Kaitse on juhtumite vältimine, avastamine on umbes ohtude varakult tuvastada ja vastus on evicting ründaja ja süsteemide rikkumise mõju taastamise kohta.

Selles artiklis kasutatakse turvalisus langeva vastuse etappide artiklist [Microsoft Azure'i turvalisus vastuse pilves](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) , nagu on näidatud järgmisel joonisel:

![Langeva vastuse elutsükkel](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Saate turbekeskus tuvasta, hinnata ja diagnoosimine etapi jooksul. Siit leiate näiteid, kuidas turbekeskus võib olla kasulik ajal algsete langeva vastuse kolm etappi:

- **Tuvasta**: vaadake sündmuse juurdlust esimene märge.
    - Näide: läbivaatus, et kõrge tähtsusega turbeteates esitati turbekeskus armatuurlaua algse kinnitamine.
- **Hinnata**: teha esialgse hindamise kahtlaste tegevuse kohta lisateabe saamiseks.
    - Näide: saada lisateavet turvalisus teatise.
- **Diagnoosimine**: tehnilise juurdluse ja piiramist, vähendamiseks ja lahendus leida.
    - Näide: järgige selle kindla turbeteates turbekeskus kirjeldatud parandamise juhiseid.

Stsenaarium, mis tuleneb näitab, kuidas kasutada turbekeskus turvalisus juhtum tuvasta, hinnata ja diagnoosimine ja vasta etapi jooksul. Turbekeskus, on [Turvalisus juhtum](security-center-incident.md) on kõik teatised, ressursi joondamiseks [tappa ahelas](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) mustrite liitmise. [Turbeteatiste](security-center-managing-and-responding-alerts.md) paani ja tera kuvada juhtumid. Juhtum näitab loendit seotud teatised, mis võimaldab teil saada lisateavet iga kord. Turbekeskus esitatakse ka eraldi Turbeteatiste, mida saate kasutada ka kahtlaste tegevuste välja selgitada.

## <a name="scenario"></a>Stsenaarium

Contoso viimati viiakse osa oma kohapealse ressursid Azure, sh mõned virtuaalse masina vastavalt ärivaldkonna töökoormus ja SQL-i andmebaasid. Praegu Contoso's Core arvuti turvalisuse juhtum vastuse meeskonnatöö (CSIRT) on probleem uurimiseks tõttu Turve intelligence ei oma praeguse langeva vastuse tööriistad integreeritud turvalisus. Integreerimine puudumine tutvustab probleem tuvasta esitusala (liiga palju false positiivsed), samuti hinnata ja diagnoosimine etappide ajal. See migreerimise osana otsustanud valida turbekeskus, mis aitavad probleemi lahendamiseks.

Esimese etapi selle migreerimise lõpule jõudnud pärast nende onboarded kõik ressursid ja kõik soovitused turbekeskus turvalisuse kohta. Contoso CSIRT on arvuti turvalisuse juhtumite tegelemiseks. Meeskond koosneb rühma inimeste kohustused mis tahes turvalisus juhtum tegelemiseks. Meeskonna liikmed on selgelt ülesannete vastus ei ala on jääda katmata.

Selleks, et selle stsenaariumi korral meil läheb fookus järgmised personas, mis on osa Contoso CSIRT rolle:

![Langeva vastuse elutsükkel](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy on turvalisus toimingud. Tema kohustused on järgmised.

- Jälgimine ja reageeri suhtes ööpäev.
- Suurenevad cloud töökoormus omanik või turvalisus analüütik vastavalt vajadusele.

Sam on turvalisus analüütik ja tema kohustuste hulgas olla:

- Välja selgitada eest.
- Remediating teatised.
- Töötamine abil saate määratleda ja rakendada kergendamise töökoormus omanikud.

Nagu näete, Judy ja Sam on erinevad ülesanded ja peab koos töötada turbekeskus teabe jagamiseks.

## <a name="recommended-solution"></a>Soovitatav lahendus

Kuna Judy ja Sam on erinevad rollid, nad kasutame erinevatel aladel turbekeskus saada asjakohast teavet oma jaoks. Judy kasutatakse **Turbeteatiste** osana tema igapäevane jälgimine.

![Turbeteatiste](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy kasutatakse Turbeteatiste tuvasta ja hindamise etapi jooksul. Kui Judy esialgse hindamise lõpule jõudnud, võib ta Videokõnele Sam küsimus, kui on vaja teha täiendavaid uurimine. Selles etapis Sam kasutab saadud turbekeskus, mõnikord koos teistest andmeallikatest liikumiseks diagnoosimine esitusala teavet.


## <a name="how-to-implement-this-solution"></a>Kuidas seda lahendust

Vaadata, kuidas soovite kasutada Azure turbekeskus langeva vastuse stsenaariumi puhul, tuleb me Judy juhiste tuvasta ja hinnata järk-järgult ja seejärel näha, mida Sam teeb probleemi tuvastada.

### <a name="detect-and-assess-incident-response-stages"></a>Avastada ja hinnata langeva vastuse etappi

Judy Azure portaali sisse logitud ja turbekeskuse konsooli töötab. Osana oma igapäevast jälgida tegevuste alustamine ta läbivaatamise kõrge tähtsusega Turbeteatiste, tehes järgmist:

1. Klõpsake paani **Turbeteatiste** ja **turbeteatised** tera juurde.
    ![Turvalisus teatiste blade](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Selleks, et selle stsenaariumi korral Judy saab sooritada hinnang pahatahtlik SQL-i tegevuse teatise, nagu eelmisel joonisel näha.
2. Klõpsake **pahatahtlik SQL-i tegevuse** teatise ja vaadake üle ründasid ressursside kohta **pahatahtlike SQL-i tegevuse** tera:  ![juhtum üksikasjad](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Klõpsake selle tera Judy märkmeid saate teha ründasid piisavalt ressursse, kuidas juhtunud mitu korda selle rünnaku ja millal see tuvastati.
3. Klõpsake selle **Ressursi ründas** selle rünnaku kohta lisateabe saamiseks.

Pärast lugemist kirjeldus, Judy on veendunud, et see ei ole vale positiivne ja, et ta peaks Videokõnele sel juhul, kui soovite Sam.

### <a name="diagnose-incident-response-stage"></a>Langeva vastuse etapi diagnoosimine

Sam saab juhul Judy ja käivitab parandamise juhiseid, mis turbekeskus soovitatud läbivaatamine.

![Langeva vastuse elutsükkel](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Lisaressursid

Langeva vastuse meeskonnatöö saate ka ära [Turvalisus Center Power BI](security-center-powerbi.md) võimalus erinevat tüüpi aruannete kuvamiseks. Need aruanded aitavad need täiendav uurimine, visualiseerimine, analüüsida ja filtreerida soovitused ja turbeteatised ajal. Ettevõtted, mis kasutada oma Turbeteave ja sündmuse (SIEM) lahendus uurimise käigus, nad võivad ka, [integreerida turbekeskus oma lahendus](security-center-integrating-alerts-with-log-integration.md). [Azure'i log integreerimine tööriista](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)abil saate ka integreerida Azure auditilogid ja virtuaalse masina (VM) turvalisus sündmused. Rünnaku uurimiseks saate kasutada seda teavet koos turbekeskus annab teavet.


## <a name="conclusion"></a>Kokkuvõte

Kokkupanekuks meeskonna enne juhtum esineb on väga oluline ettevõtte ja positiivse mõjutab, kuidas juhtumite käsitlemist. Jälgida ressursse vajalikele tööriistadele aitavad see meeskonnatöö vajadus täpne migreerimisel ning probleemide lahendamisel turvalisus juhtum. Turbekeskus [võimaluste avastamiseks](security-center-detection-capabilities.md) aitab see kiiresti vastata turvalisus juhtumite ja migreerimisel ning probleemide lahendamisel turvalisus.
