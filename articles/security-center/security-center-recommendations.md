<properties
   pageTitle="Turvalisus soovitused Azure'i Security Center haldamine | Microsoft Azure'i"
   description="Selle dokumendi juhendab teid kuidas Azure'i turbekeskus soovitusi aitab teil kaitsta oma Azure ressursse ja vastavalt turbepoliitikate."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="managing-security-recommendations-in-azure-security-center"></a>Azure'i turbekeskus väärtpaberi soovitusi haldamine

Selle dokumendi juhendab teid soovitused kasutamine Azure turbekeskus aitab kaitsta oma Azure ressursse.

> [AZURE.NOTE] Selle dokumendi tutvustab teenus on näide juurutamise abil.  See ei ole samm-sammult juhendi.

## <a name="what-are-security-recommendations"></a>Mis on soovitused turvalisuse kohta?
Turbekeskus analüüsib perioodiliselt teie Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi. Soovitused juhatavad teid protsessi konfigureerimine vajalik juhtelement.

## <a name="implementing-security-recommendations"></a>Soovitusi, Turve

### <a name="set-recommendations"></a>Soovitused määramine

[Sätte turbepoliitikate Azure'i Security Center](security-center-policies.md), saate teada, et:

- Turvalisus poliitikate konfigureerimine.
- Andmete kogumine sisse lülitada.
- Valige mis soovituste kuvamiseks teie turbepoliitika osana.

Praeguse poliitika soovitused center süsteemi värskendused, võrdlusalus reeglid, ründevaratõrje programmid, alamvõrku ja võrgu liidesed, SQL-i andmebaasi auditeerimine, SQL-i andmebaasi läbipaistev andmete krüptimine ja web rakenduse tulemüürid [võrgu turberühmad](../virtual-network/virtual-networks-nsg.md) .  [Säte turbepoliitikate](security-center-policies.md) kirjeldatakse iga soovitus suvand.

### <a name="monitor-recommendations"></a>Kuvari soovitused
Pärast turvalisuse poliitika seadmisele turbekeskus analüüsib oma ressursse, mis tähistavad võimaliku turvalisus olek. **Soovitused** paani **Turbekeskus** enne saate teada soovitused, mis on tähistatud turbekeskus koguarv.

![Soovitused paan][1]

Iga soovitus üksikasjade vaatamiseks tehke järgmist.

1. Klõpsake soovitud **soovitused paani** **Turbekeskus** enne. **Soovitused** tera avaneb.

Soovitused on esitatud tabelina, kus iga rida tähistab ühe kindla soovituse. Selles tabelis veerud on:

- **Kirjeldus**: selgitatakse soovitust ja mida tuleb teha selle lahendamiseks.
- **RESSURSI**: on loetletud ressursid, millega on seotud soovitusega.
- **Olek**: kirjeldatakse soovitust praegune olek:
    - **Avatud**: soovitust pole veel adresseeritud.
    - **Pooleli**: soovitust rakendatakse praegu ressursside ja midagi on vaja teie poolt.
    - **Lahendatud**: soovitus on juba lõpetatud (sel juhul rea võib olla tuhm).
- **Raskusaste**: kirjeldatakse kindla soovituse tõsidust:
    - **Kõrge**: haavatavuse olemas tähenduslik ressursi (nt rakenduse, VM või turberühma võrgu) ja nõuab tähelepanu.
    - **Keskmise**: on välja andnud ja mittekriitilise või täiendavad toimingud on vaja seda kõrvaldada või protsessi lõpuleviimiseks.
    - **Madal**: on välja andnud mida tuleks käsitleda, kuid ei nõua kohest tähelepanu. (Vaikimisi madal soovitused pole esitada, kuid saate filtreerida madal soovitused kui soovite näha nende.)

Kasutage järgmises tabelis, et aidata teil mõista saadaval soovitused ja mida igat teha, kui rakendate selle abimaterjalina.

> [AZURE.NOTE] Soovite [klassikaline ja ressursihaldur juurutamise mudelite](../azure-classic-rm.md) Azure ressursid.

|Soovitused|Kirjeldus|
|-----|-----|
|[Andmete kogumine tellimuste lubamine](security-center-enable-data-collection.md)|Soovitab lülitada andmete kogumise turbepoliitika iga tellimuste ja kõik virtuaalmasinates (VM) oma kontot.|
|[Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade](security-center-remediate-os-vulnerabilities.md)|Soovitab joondada oma OS konfiguratsioone soovitatav konfiguratsioon reegleid, nt võimaldab paroole salvestada.|
|[Süsteemi värskenduste rakendamine](security-center-apply-system-updates.md)|Soovitab juurutama VMs puuduvad süsteemi turbe- ja kriitilised värskendused.|
|[Taaskäivitage pärast süsteemi värskendused](security-center-apply-system-updates.md#reboot-after-system-updates)|Soovitab VM süsteemi värskenduste rakendamine protsessi lõpuleviimiseks taaskäivitada.|
|[Tulemüüri web rakenduse lisamine](security-center-add-web-application-firewall.md)|Soovitab juurutada web rakenduse tulemüüri (WAF) web lõpp-punktid. Mitme veebirakenduste turbekeskus saate kaitsta, lisades oma olemasoleva WAF juurutuste need rakendused. WAF seadmed (ressursihaldur juurutamise mudeli abil loodud) peab töötama eraldi virtuaalse võrku. WAF seadmed (klassikaline juurutamise mudeli abil loodud) on piiratud turberühma võrgu kaudu. See tugi laiendatakse tulevikus täielikult kohandatud juurutusega on WAF seadme (klassikaline). Turbekeskus soovitab, et olete ette WAF, mis aitavad teie veebirakenduste VMs ja rakenduse teenuse keskkonnas (ASE) suunamise eest kaitsta. ASE kohta leiate lisateavet teemast [Rakenduse Teenusedokumentatsiooni keskkonnas](../app-service/app-service-app-service-environments-readme.md). |
|[Kaitse viimistlemine](security-center-add-web-application-firewall.md#finalize-application-protection)|Seadistamiseks on WAF suunatakse ümber liiklus, WAF seadmele. Pärast soovitusega täiendavad häälestamise vajalikud muudatused.|
|[Järgmise genereerimine tulemüüri lisamine](security-center-add-next-generation-firewall.md)|Soovitab lisada oma turvalisus kaitstud suurendamiseks mõnelt Microsofti partnerilt järgmise genereerimine tulemüüri (NGFW).|
|[Marsruutimiseks liiklus läbi NGFW ainult](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Soovitab võrgu turvalisus jaotises (NSG) reeglid, mis jõustada sissetulev liiklus teie VM kaudu oma NGFW konfigureerida.|
|[Endpoint Protection installimine](security-center-install-endpoint-protection.md)|Soovitab, et olete ette ründevaratõrje programmid vms (ainult Windows VMs).|
|[Endpoint Protection seisund teatiste lahendamine](security-center-resolve-endpoint-protection-health-alerts.md)|Soovitab lahendada endpoint protection ebaõnnestumist.|
|[Luba võrgus turberühmad alamvõrku või virtuaalmasinates](security-center-enable-network-security-groups.md)|Soovitab, et lubada NSGs alamvõrku või VMs.|
|[Vastastikuste lõpp-punkti Interneti kaudu juurdepääsu piiramise](security-center-restrict-access-through-internet-facing-endpoints.md)|Soovitab sissetulev liiklus reeglite konfigureerimine NSGs.|
|[Luba auditi SQL server](security-center-enable-auditing-on-sql-servers.md)|Soovitab lülitada auditeerimine Azure SQL-i serverite (Azure SQL-i teenuse; ei kaasata ainult teie virtuaalmasinates töötab SQL-i).|
|[Luba auditi SQL-i andmebaas](security-center-enable-auditing-on-sql-databases.md)|Soovitab lülitada auditeerimine Azure SQL-i andmebaasid (Azure SQL-i teenuse; ei kaasata ainult teie virtuaalmasinates töötab SQL-i).|
|[Läbipaistva andmete krüptimine SQL andmebaase lubamine](security-center-enable-transparent-data-encryption.md)|Soovitab, et lubate krüptimise SQL-i andmebaasid (ainult Azure SQL-i teenus).|
|[Luba VM Agent](security-center-enable-vm-agent.md)|Võimaldab teil näha, mis nõuavad VMs VM Agent. VM Agent peab olema installitud VMs selleks, et ettevalmistamise paik skannimine, võrdlusalus skannimine ja ründevaratõrje programmid. Vaikimisi installitakse VM Agent vms Azure'i turuplatsilt juurutatud. Artikli [VM Agent ja laiendid – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) teave VM agendi installimise kohta.|
| [Ketta krüptimise rakendamine](security-center-apply-disk-encryption.md) |Soovitab teie VM ketast Azure'i ketta krüptimise (Windows ja Linux VMs) abil krüptida. Teie VM OS nii andmete maht on soovitatav krüptimine.|
|[Turvalisus kontakti üksikasjad](security-center-provide-security-contact-details.md) | Soovitab annate turvalisus kontaktteave iga tellimuste. Kontaktteave on e-posti meiliaadressi ja telefoninumbri sisestamine. Teavet kasutatakse teiega ühendust võtta, kui väärtpaberi meie meeskond leiab, et teie ressursid on kahjustada. |
| [Värskenduse OS versioon](security-center-update-os-version.md) | Soovitab värskendada oma OS pere jaoks oma pilveteenuses, et saadaval kõige uuem versioon operatsioonisüsteemi (OS) versioon.  Pilveteenustega kohta leiate lisateavet teemast [pilveteenustega ülevaade](../cloud-services/cloud-services-choose-me.md). |
| [Haavatavushindamises pole installitud](security-center-vulnerability-assessment-recommendations.md) | Soovitab lahenduse haavatavuse assessment installimine oma VM. |
| [Migreerimisel ning probleemide lahendamisel nõrkade kohtade](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Võimaldab teil näha süsteemi ja rakenduste nõrkade kohtade tuvasta installitud oma VM haavatavuse hindamise lahendus. |

Saate filtreerida ja soovitused jätta.

1. **Soovitused** enne nuppu **Filtreeri** . **Filtri** tera avaneb ning valida raskusaste ja osariigi väärtused, mida soovite näha.

    ![Filtri soovitused][2]

2. Kui olete kindlaks teinud, et soovitus ei ole, saate soovitust eiramine ja seejärel filtreerida oma vaatest. On kaks võimalust hülgamiseks soovituse. Üks võimalus on paremklõpsake üksust ja seejärel nuppu **unusta**. Teine on kursorit üksuse, klõpsake paremal kuvatavate kolme punkti ja seejärel valige **unusta**. Saate vaadata, klõpsates **Filter**ja seejärel käsku **Dismissed**töölt soovitusi.

    ![Soovitus eiramine][3]

### <a name="apply-recommendations"></a>Soovitused rakendamine
Pärast soovitused otsustada, milline enda tuleks esmalt rakendada. Soovitame kasutada raskusaste nagu rakendub esmalt peamine parameeter hindamaks, milliseid soovitusi.

Soovitused ülaltoodud tabelis, valige soovituse ja läbi see, nagu näiteks soovituse rakendamise kohta.

## <a name="see-also"></a>Vt ka
Selles dokumendis võeti kasutusele turvalisus soovitused turbekeskus. Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastata.
- [Partnerite lahenduste ja Azure turbekeskus jälgimine](security-center-partner-solutions.md) – saate teada, kuidas oma partnerite lahenduste seisund oleku jälgimine.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Leia ajaveebi Azure Turve ja nõuetele vastavuse kohta.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
