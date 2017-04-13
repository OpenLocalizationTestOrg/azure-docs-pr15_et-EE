<properties 
   pageTitle="Luua kauplemise partneri lepingu Azure'i rakendust Service | Microsoft Azure'i" 
   description="Partneri lepingute börsipäev loomine" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Kauplemise partneri lepingu loomine   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Kaubanduspartnerid on kaasatud B2B üksused (Business Business) suhtlus. Kui kaks partnerite luua seose, seda nimetatakse *leping*. Määratletud lepingu põhineb side kahe partnerite lemmikute saavutamiseks ja on Protocol (protokoll) või teatud transport. Erinevate B2B protokollid ja Azure rakenduse teenus ei toeta transport on järgmised.

- AS2 (kohaldamine lause 2)
- EDIFACT (Ameerika riigid/elektroonilised andmevahetuse Administreerimine, äri ja Transport (UN/EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalki API rakendusi, mis toetavad B2B stsenaariumid
Järgmised API rakendused nende võimaluste kasutamine Azure portaali rikkaliku ja intuitiivne kogemus lubamiseks tehke järgmist.


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalki börsipäev partnerite halduse (TPM)
- Loomist ja haldamist partnerid, profiilid ja identiteedid
- Salvestamine ja EDI skeemid haldamine
- Salvestusruum ja sertifikaatide (kasutatakse AS2 protocol) haldamine
- Loomine ja AS2 lepingute haldamine
- Loomist ja haldamist EDIFACT lepingud (sh partiide saada pool)
- Loomist ja haldamist X12 lepingud (sh partiide saada pool)

![][1]


## <a name="as2-connector"></a>AS2 konnektor
- Käivitab AS2 lepingutega seotud TPM API rakenduse eksemplari määratletud
- Üksusekuvamall saab hankida AS2 töötlemine/jälgimise tõrkeotsingu teave


## <a name="biztalk-edifact"></a>BizTalki EDIFACT
- Käivitab EDIFACT lepingutega seotud TPM API rakenduse eksemplari määratletud
- Üksusekuvamall saab hankida EDIFACT töötlemine/jälgimise tõrkeotsingu teave
- Pakub riigi juhtimine pakettidena (käivitamise ja lõpetamise) määratletud EDIFACT leping(ud) seotud TPM API rakenduse eksemplari


## <a name="biztalk-x12"></a>BizTalki X12
- Käivitab X12 lepingutega seotud TPM API rakenduse eksemplari määratletud 
- Toob esile X12 töötlemine/jälgimise tõrkeotsingu teave
- Pakub riigi juhtimine pakettidena (käivitamise ja lõpetamise) määratletud X12 leping(ud) seotud TPM API rakenduse eksemplari

Nagu eelnevalt mainitud, AS2, X 12 ja EDIFACT API rakenduste jaoks on vaja TPM API rakenduse funktsiooni ootuspäraselt.


## <a name="getting-started"></a>Alustamine
Kauplemise partneri lepingute loomiseks tehke järgmist.

1. Looge **BizTalki börsipäev partnerite halduse** konnektor eksemplari. Selleks on vaja tühja SQL-andmebaasi funktsiooni. Enne alustamist kindlasti on tühi andmebaas saadaval ja kasutamiseks valmis.
2. Laadige skeeme ja sertifikaatide vastavalt vajadusele lepingutes. Selleks TPM eksemplari loodud ja samm arvesse "Skeeme" ja/või "Tunnistuste" osa sirvimine
3. Liikuge sirvides TPM eksemplari, mis on loodud ja samm **partnerite** osa
4. Saate luua partnerite vastavalt soovile. Lisaks Redigeeri profiili vastavalt vajadusele ja lisada nõutavad identiteedid
5. Nüüd looge lepingute **lepingute** osa abil. Kui loote leping, valige Protocol (protokoll), mida kasutatakse. Ülejäänud konfiguratsiooni suvandid põhinevad valitud protokolli.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
