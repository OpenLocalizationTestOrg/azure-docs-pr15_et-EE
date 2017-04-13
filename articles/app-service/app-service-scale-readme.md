<properties
    pageTitle="Azure'i rakenduse teenus: Skaleerimist rakenduse teenuserakendused"
    description="Siit saate teada, põhjalikumat ning rakenduse App teenuses."
    keywords="rakenduse, azure rakenduse teenus, skaala scalable, rakenduse teenusleping, rakenduse teenuse kulu"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure'i rakenduse teenus: Skaleerimist rakenduse teenuserakendused

Teenuses Azure rakenduse rakendused saate [saavutada suuri skaala](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Siiski skaleerimist rakendus on keeruline probleem, mis ei ole "üks suurus sobib kõigile" lahendus. Rakenduse seal on õigesti skaala 3 olulistes valdkondades, mis aitavad teie rakendused edu.

1. Oma rakenduse arhitektuur ja nõrkuste mõistmine.
    * Kas teie taotlus Stateful? Kodakondsuseta?
    * Mis on rakenduse kõik komponendid?
        * Kus asuvad rakenduse kitsaskohad?
    * Kui laadimine on seotud teie rakendus, mis murda esimese?
2. Oodatud laadimine ja jõudluse nõuete mõistmine.
    * Rakendus vajab teenida tuhande kasutajad? või üks miljon?
    * Liikluse tulevad ühe geograafilise asukoha või globaalselt?
    * Kas hooajaline variatsioonid? liikluse peaks?
    * Kui kiiresti peaks rakenduse vastata? 1 teise? 1 millisekundilist?
3. Mõistmine ja õigesti võimendada majutada oma rakenduse platvorm.
    * Milliseid funktsioone tuleks kasutada minu skaala eesmärkide?

Selles jaotises aitab teil mõista tegurid ja abi, saate töötada strateegia, mis suunab teie skaleeritavus eesmärkidele vajalikud rakenduse teenuse funktsioone.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
