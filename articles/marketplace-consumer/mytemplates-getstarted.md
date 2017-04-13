<properties
   pageTitle="Isiklike mallide kasutamise alustamine | Microsoft Azure'i"
   description="Lisada, hallata ja jagada isiklike mallide Azure portaali Azure'i CLI või PowerShelli abil."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Azure'i portaalis isiklike mallide kasutamise alustamine

Mõni [Azure ressursihaldur](../resource-group-authoring-templates.md) mall on deklaratiivseid malli abil saate määratleda juurutamise. Saate määratleda ressursid lahenduse juurutamine ja määrake parameetrite ja muutujate, mis võimaldavad teil viibite väärtuste sisestamiseks. Mall sisaldab JSON ja avaldised, mille abil saate koostada oma juurutamiseks väärtused.

Abil saate uue **malli** võimalus [Azure portaali](https://portal.azure.com) koos **Microsoft.Gallery** ressursi pakkuja [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/) pikendamise lubamine loomine, haldamine ja juurutamine isiklike mallide isikliku teegist.

Selle dokumendi juhatab teid läbi lisamise, haldamine ja jagamine privaatne **malli** Azure'i portaalis.

## <a name="guidance"></a>Juhised

Järgmiseid soovitusi aitab teil ära **malle** oma lahenduste töötamisel:

- **Mall** on kapseldamist ressurss, mis sisaldab mõnda ressursihaldur Mall ja täiendavad metaandmed. See käitub üsna samamoodi üksuse kuvatakse Marketplace'ist. Võtme vahe, et see on privaatne üksuse asemel avaliku turuplatsi üksused.
- **Mallide** teegi sobib hästi kasutajatele, kes peavad saama kohandada.
- **Mallide** töötada ka kasutajatele, kellel on vaja lihtsa hoidla Azure'is.
- Alustage olemasoleva ressursihaldur malli. Mallid leiate [github](https://github.com/Azure/azure-quickstart-templates) või [eksport malli](../resource-manager-export-template.md) olemasoleva ressursi rühmast.
- **Mallid** on seotud kasutaja avaldab. Publisheri nimi on nähtav kõigile, kellel on see lugemisõigus.
- **Mallid** on ressursihaldur ressursid ja ei saa ümber nimetada üks kord avaldatud.

## <a name="add-a-template-resource"></a>Malli ressursi lisamine

On kaks võimalust luua **malli** ressursi Azure'i portaalis.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Meetod 1: Luua uue malli ressursi töötava ressursirühm

1. Liikuge ressursi olemasolevasse rühma Azure'i portaalis. Valige **ekspordi malli** **sätted**.
2. Kui eksporditakse ressursihaldur Mall, nupp **Salvesta malli** abil salvestada **mallide** hoidla. Kõigi üksikasjade ekspordi malli otsimine [siin](../resource-manager-export-template.md).
<br /><br />
![Ressursi rühma eksportimine](media/rg-export-portal1.PNG)  <br />

3. Valige nupu **Salvesta Mall** .
<br /><br />

4. Sisestage järgmine teave:

    - Nimi – objekti malli nimi (Märkus: see on Azure ressursihaldur vastavalt nimi. Kõigi nimede piirangud ja seda ei saa muuta, kui loonud).
    - Kirjeldus – lühikese kokkuvõtte malli kohta.

    ![Malli salvestamine](media/save-template-portal1.PNG)  <br />

5. Klõpsake nuppu **Salvesta**.

    > [AZURE.NOTE] Ekspordi malli tera kuvab teatisi, kui eksporditud ressursihaldur mall on tõrkeid, kuid te saab salvestada selle ressursihaldur malli mallid. Veenduge, et kontrollida, ja mis tahes ressursihaldur enne ümbersuunamine eksporditud ressursihaldur malli malli seotud probleemide lahendamine.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Meetod 2: Saate lisada uue ressursi malli põhjal sirvimine

Saate lisada uue **malli** nullist kasutada funktsiooni + nuppu Lisa käsk **sirvimine > Mallid**. Peate nimi, kirjeldus ja ressursihaldur malli JSON.

![Malli lisamine](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery on rentniku Azure ressursi pakkuja. Ressursi mall on seotud loonud selle kasutaja. See on seotud mis tahes teatud tellimus. Tellimuse tuleb valida ainult siis, kui malli juurutamine.

## <a name="view-template-resources"></a>Kuva malli ressursid

Kõik **Mallid** on saadaval näha **sirvimine > Mallid**. See sisaldab **malle** loodud need, mis on teiega jaganud erineva õigused. Rohkem üksikasju allpool jaotises [juurdepääsu reguleerimine](#access-control-for-a-tenant-resource-provider) .

![Vaate Mall](media/view-template-portal1.PNG)  <br />

Saate **malli** üksikasjade vaatamiseks klõpsata üksusesse loendis.

![Vaate Mall](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Ressursi malli redigeerimine

Saate algatada **malli** redigeerimine voogu paremale klõpsake Sirvi loendipunktiga või, valides käsu nuppu Redigeeri.

![Malli redigeerimine](media/edit-template-portal1a.PNG)  <br />

Saate muuta kirjeldust või ressursihaldur malli teksti. Nime ei saa redigeerida, kuna see on ressursihaldur ressursi nimi. Kui redigeerite ressursihaldur malli JSON me kinnitada tagamaks, mis ei sobi JSON. Valige **OK** ja seejärel **salvestage** värskendatud malli salvestamiseks.

![Malli redigeerimine](media/edit-template-portal2a.PNG)  <br />

Kui **mall** on salvestatud, kuvatakse kinnitusteade.

![Malli redigeerimine](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Malli ressursi juurutamine

Saab kasutada mis tahes **malli** **lugemine** õigusi. Juurutamise voogu käivitab standardset Azure'i malli juurutamise toru. Täitke jätkamiseks juurutamise ressursihaldur malli parameetrite väärtused.

![Malli juurutamine](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Malli ressursi jagamisel

**Malli** ressursi saab jagada oma kolleegidega. Ühiskasutuse käitub samamoodi [Rollimäärang mis tahes ressursi Azure](../active-directory/role-based-access-control-configure.md). **Malli** omanik pakub õigused teistele kasutajatele, kes saab interaktiivselt kasutada malli ressursi. Isik või rühm inimesi jagatava **malli** abil saab ressursihaldur Mall ja selle galerii atribuutide kuvamiseks.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Juurdepääsu reguleerimine Microsoft.Gallery ressursid

Roll | Õigused
---|----
Omanik | Võimaldab Täielik kasutusõigus, sh ühiskasutus malli ressurss
Lugeja | Võimaldab lugemis- ja Execute(Deploy) malli ressurss
Kaasautor | Malli ressurss võimaldab õiguste redigeerimine ja kustutamine. Kasutaja ei saa teistega jagada Mall

Valige **ühiskasutus** üksusel Sirvi, klõpsates paremale või kindla üksuse kuvamine enne. See käivitab ühiskasutus kogemus.

![Ühiskasutus Mall](media/share-template-portal1a.png)  <br />

 Nüüd saate valida rollid ja kasutaja või rühma juurdepääsu kindla **malli**. Saadaval rollid on omanik, lugeja ja kaasautor. Rohkem üksikasju eelmises jaotises [juurdepääsu reguleerimine](#access-control-for-a-tenant-resource-provider) .

![Ühiskasutus Mall](media/share-template-portal2b.png)  <br />

![Ühiskasutus Mall](media/share-template-portal3b.png)  <br />

Klõpsake nuppu **Ok**ja **Valige** . Nüüd näete kasutajate või rühmade lisatud ressurss.

![Ühiskasutus Mall](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Malli saab jagada ainult kasutajate ja rühmade Azure Active Directory samal rentnikukontol. Kui jagate malli e-posti aadressi, mis pole teie rentnikus, kutse saadetakse kasutajalt rentniku külalisena liitumiseks.

## <a name="next-steps"></a>Järgmised sammud

- Ressursihaldur mallide loomise kohta leiate lisateavet teemast [funktsiooniga Mallid](../resource-group-authoring-templates.md)
- Funktsioonide abil saate ressursihaldur mallis, vt [malli funktsioonid](../resource-group-template-functions.md)
- Juhised mallide kujundamine, leiate [Azure'i ressursihaldur Mallid kujundamise head tavad](../best-practices-resource-manager-design-templates.md)
