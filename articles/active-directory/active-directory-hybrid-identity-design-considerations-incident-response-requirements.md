
<properties
    pageTitle="Azure Active Directory hübriid identiteedi kujundamine - määratleda langeva rResponse nõuded | Microsoft Azure'i nõuded "
    description="Hübriid identiteedi lahenduse, mida saate kasutada seda ära tunda ja leevendada võimalike ohtudega toimingute tegemiseks järelevalve ja võimaluste määratlemine"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Langeva vastuse nõuded lahendusse hübriid identiteedi määratlemine

Suure või keskmise suurusega ettevõtted tõenäoliselt on [Turvalisus langeva vastust](https://technet.microsoft.com/library/cc700825.aspx) , et aidata seda teha toiminguid vastavalt sellele tasemele juhtum. Funktsiooni identiteedihaldussüsteemi on oluline komponent langeva vastuse protsessi, kuna seda saab kasutada kindlaks teha, kes läbi teatud toimingu soovitud sihi suhtes. Hübriid identiteedi lahenduse peab olema võimalus sisestage järelevalve ja võimalusi, mida saate kasutada seda ära tunda ja leevendada võimalikku ohtu toimingute tegemiseks. Tüüpilised langeva vastuse leping teil järgmised etapid leping osana.

1.  Esmane hindamine.
2.  Langeva suhtlus.
3.  Kahju juhtelement ja vähendamiseks.
4.  Identifitseerimise, mis oli kompromiss ja tõsidust.
5.  Tõendusmaterjalide kogumiseks säilitamine.
6.  Teatis isikutega.
7.  Süsteemi taastamise.
8.  Dokumendid.
9.  Kahju ja kulu hindamiseks.
10. Protsessi ja leping läbivaatamine.

Kindlakstegemine, mida see oli kompromiss ja raskusastme etapi, on vaja tuvastada süsteemide, mis on rikutud, failid, mis on külastatud ja määrata need failid tundlikkust. Teie hübriid identiteedi süsteemi peaks oskama aidata teil tuvastada kasutaja tehtud muudatuste nende nõuete täitmiseks. 

## <a name="monitoring-and-reporting"></a>Jälgimine ja aruandlus
Mitu korda identiteedi süsteem aitab esialgse hindamise etapis peamiselt kui süsteemi on sisseehitatud ja aruandluse võimalusi. Algne hindamisel IT-administraator peab olema määratlemaks kahtlase tegevuse või süsteemi peaks oskama päästik automaatselt põhjal eelnevalt konfigureeritud tööülesande. Mitu võib viidata võimaliku rünnaku, kuid muudel juhtudel halvasti konfigureeritud süsteemi võivad põhjustada arv vale-positiivsed sissetungi tuvastamise süsteemi. 

Funktsiooni identiteedihaldussüsteemi aitaks administraatorid tuvastada ja nende kahtlaste tegevuste aruande. Tavaliselt saab neid tehnilise nõudeid täitma jälgimise kõik süsteemid ja millel aruannete võimalus, et saate esile tõsta võimalike ohtudega. Kasutada järgmistele küsimustele, mis aitavad teil oma hübriid identiteedi loomise lahendus võttes arvesse langeva vastuse nõudeid:

- Kas teie ettevõttel on turvalisus langeva vastuse kohas?
 - Kui jah, kasutatakse praeguse identiteedihaldussüsteemi selle käigus?
- Kas teie ettevõte on vaja kahtlaste sisselogimise katsete kasutajate tuvastamiseks erinevates seadmetes
- Kas teie ettevõte on vaja võimalike rikutud kasutaja mandaat tuvastamiseks
- Ettevõtte vajab auditeeritavad kasutaja ja töötab?
- Ettevõtte vajab teadma, kui kasutaja oma parooli lähtestada?

## <a name="policy-enforcement"></a>Teabehalduspoliitika rakendamine

Kahju juhtelement ja riski vähendamine – etapp, on oluline kiiresti vähendada rünnaku tegelikku ja võimalikku mõju. Selle toimingu, et võtate sel hetkel saate teha väike ja suur erinevus. Täpse vastuse sõltub teie ettevõtte ja rünnak, mida te laad. Kui esialgse hindamise lõpule viidud, et konto turvalisust on rikutud, peate Jõusta poliitika blokeerimiseks selle kontoga. See on ainult üks näide, kus on identiteedihaldussüsteemi saavad kasutada. Kasutada järgmistele küsimustele, mis aitavad teil kujundada teie hübriid identiteedi lahendus arvestades, kuidas jõustatakse poliitikate reageerida poolelioleva juhtum:

- Kas teie ettevõttel on poliitikate kohas Blokeeri kasutajatele juurdepääsu võrgu vajadusel?
 - Kui jah, kas praegune lahendus integreerida hübriid identiteedihaldussüsteemi, mida te ei kavatse vastu?
- Kas teie ettevõte on vaja juurdepääsu kasutajatele, kellel on karantiini jõustamiseks 
 
>[AZURE.NOTE]
Veenduge, et iga vastus märkmete tegemine ja vastus vaja mõista. [Strateegia määratlemine andmed](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) lähevad üle saadaolevad suvandid ja kummagi variandi eelised ja puudused.  Millel vastused neile küsimustele valite, millise variandi parima sobib teie ettevõtte vajab.

## <a name="next-steps"></a>Järgmised sammud
[Andmete kaitsmise strateegia määratlemine](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Vt ka
[Kujundus kaalutlused ülevaade](active-directory-hybrid-identity-design-considerations-overview.md)
