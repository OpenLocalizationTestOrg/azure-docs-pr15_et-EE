<properties
   pageTitle="Azure'i ressursside kasutamisega loogika rakendustes | Microsoft Azure'i rakendust Service"
   description="Kuidas luua ja Azure ressursi konnektor või API rakenduse konfigureerimiseks ja kasutamine loogika rakenduse teenuses Azure rakenduse"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Alustamine Azure'i ressursi konnektor ja lisada selle oma loogika rakenduse
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Azure'i ressursi konnektor abil hõlpsasti hallata Azure ressursse rakenduse loogika sees.

## <a name="create-the-azure-resource-connector"></a>Azure'i ressursi konnektor loomine
Azure'i ressursi konnektor API rakenduse kasutamiseks peate looma esmalt selle eksemplari. Seda saab teha kas Tekstisisese loogika rakenduse loomisel või valides Azure'i ressursi sõim konnektor API rakenduse Azure'i turuplatsilt.

Konfigureerimiseks, teil on teil vaja üles teenuse subjekt õigustega, mida iganes see on Azure soovite seada. Kõnede tehakse sisse-nimel-, teenuse põhilist seadistatud. See võimaldab teil ulatus konnektor kasutada ainult täpselt mida soovite teha, ja pigem.

David Ebbo on kirjutanud [hea ajaveebipostituse](http://blog.davidebbo.com/2014/12/azure-service-principal.html) häälestamise kohta. Järgige kõiki seal ja oma **Rentniku ID**, **Kliendi ID** ja **salajane**. Need kolm väljad pluss **Tellimuse ID**, mis on vaja konfigureerida konnektor.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Loogika rakenduste Designeris Azure'i ressursi Connectori abil
### <a name="trigger"></a>Päästik
On kaks käivitab, mis on toetatud konnektor.

Nimi | Kirjeldus
---- | -----------
Sündmuse | Päästik sündmuse ilmnemisel ressursi teie tellimus.
Meetermõõdustik ületab läve |  Kui mõõdiku vastab teatud ülemmäära päästik.

### <a name="action"></a>Toiming

Samuti saate sisestada sees Azure tellimuse suure hulga toimingud:

**Ressursi rühmade** jaoks saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Rühmade loendi ressurss | Loetle kõik tellimuse ressursi rühmad.
Ressursirühm hankimine | Saada oma id ressursirühma.
Ressursi rühma loomine | Saate luua või värskendada ressursirühma.
Ressursirühm kustutamine | Kustutage ressursirühma.

**Ressursid** saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Loendi ressursid | Loendi ressursid teie tellimus erinevat tüüpi filtrite abil.
Ressursside hankimine | Saada ressurss, selle ressursi ID-ga.
Loomine ja värskendamine ressurss | Ressursi loomine või värskendamine on olemasolev ressurss. Selle ressursi jaoks peate sisestama kõik atribuudid.
Ressursi toimingu |  Mis tahes muud toimingut teha ressursi. Peate teadma toimingu nime ja last, et see toiming võtab (vajaduse korral).
Ressursi kustutamine | Kustutage ressursi.

**Ressursi** pakkujate saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Loendi ressursi pakkujad | Loetle kõik saadaolevad ressursi pakkujad tellimuse.

**Ressursi rühma** juurutuste saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Loendi juurutuste | Loetle kõik juurutuste ressursside rühma.
Juurutamise hankimine | Saada oma id malli juurutamine.
Juurutamise loomine | Luua uue ressursi rühma juurutamise, andes malli.

**Sündmuste** kohta ressursid saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Saada sündmused | Sündmuste tellimuse või ressursi hankimine

**Mõõdikute** ressursside kohta saate teha järgmist.

Nimi | Kirjeldus
---- | -----------
Saada mõõdikud | Mõõdiku saamiseks ressursi ID-ga.

## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada on voog loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Kui soovite alustada Azure'i loogika rakendused enne Azure'i konto kasutajaks, minge [proovige loogika rakenduse](https://tryappservice.azure.com/?appservice=logic), kus saate kohe luua lühiajaline starter loogika rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

Vaadata ärplema REST API viide veebisaidil [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
