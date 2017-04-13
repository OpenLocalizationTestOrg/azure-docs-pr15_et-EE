<properties 
    pageTitle="Testimise on käitusjuhendi Azure automatiseerimine | Microsoft Azure'i"
    description="Azure'i automaatika lisamine käitusjuhendi avaldamiseks saab katsetage seda mis toimib ootuspäraselt.  Selles artiklis kirjeldatakse, kuidas on käitusjuhendi testimiseks ja selle väljundi kuvamine."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Testimise on käitusjuhendi Azure automatiseerimine
Kui soovitud käitusjuhendi testimiseks [mustand versioon](automation-creating-importing-runbook.md#publishing-a-runbook) on täidetud ja kõik toimingud, mida see teeb on lõpule viidud. Töö ajalugu on loodud, kuid voogu [väljundi](automation-runbook-output-and-messages.md#output-stream) ja [hoiatus ja viga](automation-runbook-output-and-messages.md#message-streams) kuvatakse testi väljund paani. [Verbose voo](automation-runbook-output-and-messages.md#message-streams) sõnumid kuvatakse paanil väljundi ainult siis, kui [$VerbosePreference muutuja](automation-runbook-output-and-messages.md#preference-variables) väärtuseks Jätka.

Kuigi mustand versioon töötab käitusjuhendi endiselt töövoo käivitab tavaliselt ja toiminguid mis tahes esitada ressursse keskkonnas. Peaks ainult test, mitte tekitamiseks vahendeid tegevusraamatud.

Toimingu katsetamiseks iga [tüüpi käitusjuhendi](automation-runbook-types.md) on sama ja ei ole testimine teksti redaktori ja Azure portaali pildiredaktor vahel vahet.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Kontrollimiseks mõnda käitusjuhendi Azure'i portaal

Saate töötada [käitusjuhendi tüüp](automation-runbook-types.md) Azure'i portaalis.

1. Avage mustand versioon käitusjuhendi [teksti toimetaja](automation-editing-a-runbook.md#Portal) või [graafiline redaktor](automation-graphical-authoring-intro.md).
2. Test tera avamiseks nuppu **testi** .
3. Kui käitusjuhendi on parameetrid, loetletakse need kuhu saate sisestada kasutatavad katsetamiseks vasakul paanil.
4. Kui soovite käivitada test [Hü Käitusjuhendi töötaja](automation-hybrid-runbook-worker.md), **Käivitage sätete** muutmine **Hü** töötaja ja valige sihtrühma nimi.  Hoida vaikimisi **Azure'i** käivitamiseks test pilveteenuses.
5. Klõpsake nuppu testi alustamiseks nuppu **Start** .
6. Kui käitusjuhendi on [PowerShelli töövoo](automation-runbook-types.md#powershell-workflow-runbooks) või [graafilised](automation-runbook-types.md#graphical-runbooks), saate lõpetada või peatada selle ajal seda testitakse nuppudega väljundi paani all. Kui käitusjuhendi peatamiseks lõpetab praeguse tegevuse on peatatud enne. Kui käitusjuhendi on peatatud, saate selle peatada või käivitage see uuesti.
7. Kontrolli käitusjuhendi väljundi paanil väljund.


## <a name="next-steps"></a>Järgmised sammud

- Saate teada, kuidas luua või importimine on käitusjuhendi, vt [loomine või importimine on käitusjuhendi Azure'i automaatika](automation-creating-importing-runbook.md)
- Graafilise loomise kohta leiate lisateavet teemast [graafilised loome Azure'i automaatika](automation-graphical-authoring-intro.md)
- Alustamine PowerShelli töövoo tegevusraamatud, lugege teemat [minu esimese PowerShelli töövoo käitusjuhendi](automation-first-runbook-textual.md)
- Runboks tagastamiseks olekuteade ja tõrkeid, sealhulgas soovitatav konfigureerimise kohta leiate lisateavet teemast [Käitusjuhendi väljundi ja sõnumite Azure automatiseerimine](automation-runbook-output-and-messages.md)