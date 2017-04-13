<properties
   pageTitle="Plaanimine teenuse struktuuri kobar võimsus | Microsoft Azure'i"
   description="Teenuse struktuuri kobar valmisoleku kavandamine peaksite arvesse võtma. Nodetypes, kestvus ja usaldusväärsuse astme"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Teenuse struktuuri kobar võimsus kasutuse kavandamine

Mis tahes tootmise juurutamiseks võimsuse planeerimine on oluline samm. Siin on mõned üksused, mida tuleb arvesse võtta selle protsessi osana.

- Arvu klaster peab algama tüüpi.
- Atribuutide iga sõlm tüüp (suurus, esmane, Interneti ees, arv VMs jne).
- Klaster töökindlust ja kestvus omaduste

Andke meile lühidalt läbi kõik need üksused.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Arvu klaster peab algama tüüpi.

Esmalt peate aru saada, mida loote klaster saab kasutada ja milliseid rakendusi kavatsete juurutada see sisse. Kui te pole selge klaster eesmärki, tõenäoliselt pole veel valmis sisestada võimsuse kavandamise protsess.

Kindlaks klaster peab algama tüüpi arvu.  Iga sõlm tüüp on vastendatud virtuaalse masina ulatuse määramiseks. Iga sõlm tüüp saab siis ülespoole või allapoole sõltumatult, on erinevaid avatud pordid ja võib olla eri võimsuse mõõdikute. Seda otsust tüüpi arvu põhiosas on näha, järgmisega:

- Kas teie taotlus on mitmes teenuses ja teha neid peavad olema avalik või Interneti-ühendusega? Tüüpilised kasutusalad sisaldada ees lüüsi teenust, mida saab sisestusmeetodi klient ja ühe või mitme tagaandmebaas teenuseid, mis suhelda ees teenustega. Nii sel juhul saate lõpuks, millel on vähemalt kaks tüüpi.

- Kas teenuste (mis moodustavad rakenduse) on nagu rohkem RAM-i või kõrgema protsessori erinevate taristu vajadustele? Näiteks Oletame, et rakendus, mida soovite juurutada sisaldab ees teenuse ja teenuse tagaandmebaas. Ees teenuse käivitada väiksema VMs (nt D2 VM suurused), mis on pordid avada Interneti-ühendus.  Teenuse tagaandmebaas siiski arvutus mahukamate ja vajadustele käitamist suuremat VMs (koos VM suurused nagu D4 D6, D15), mis ei ole internet ees.

 Selles näites kuigi saate otsustada, kas kõik teenused sellele ühe sõlme tüüp, soovitame, et saate paigutada klaster sõlm andmetüüpidega.  See võimaldab iga sõlm tüüp on erinevate atribuudid, nt Interneti-ühendus või VM suurus. Sõltumatult, samuti saate ülespoole arvu VMs.  

- Kuna te ei saa prognoosida tulevikus, asjaolud, mida te ei tea minna ja otsustada, missugused sõlm tüübid, mida on vaja alustada rakenduste arv. Saate alati lisada või eemaldada tüüpi hiljem. Teenuse struktuuri kobar peab olema vähemalt üks sõlm tüüp.

## <a name="the-properties-of-each-node-type"></a>Tippige iga sõlme atribuudid

**Sõlm tüüp** näha rollidele pilveteenustega samaväärsed kujul. Tüüpi määratleda VM suurused, VMs arv ja nende atribuudid. Iga sõlm tüüp, mis on määratletud teenuse struktuuri kobar on seadistada eraldi virtuaalse masina skaala määramine. VM skaala kogumid on Azure arvutada ressursi abil saate juurutada ja hallata kogumi virtuaalmasinates kogumina. On määratletud erinevate VM skaala määrab, iga sõlm tüüp saab siis ülespoole või allapoole sõltumatult, on erinevaid avatud pordid ja võib olla eri võimsuse mõõdikute.

Klaster võib olla rohkem kui üks sõlm tüüp, kuid esmane sõlm tüüp (esimene osa määratlete portaalis) peab olema vähemalt viis VMs jaoks kasutatakse tootmise töökoormus kogumite (või vähemalt kolme jaoks testi kogumite VMs). Kui loote klaster on ressursihaldur malli abil, siis leiate **on esmane** atribuut jaotises sõlm tüüp definitsiooniga. Esmane sõlm tüüp on sõlm tüüp, kuhu paigutatakse teenuse struktuuri teenused.  

### <a name="primary-node-type"></a>Esmane sõlm tüüp
Mitme sõlm andmetüüpidega klaster, peate valige üks neist määrata esmane. Siin on esmane sõlm tüüp omadused.

- Esmane sõlm tüübi VMs suurus määratakse kestvus taseme, valite. Kestvus taseme vaikeväärtus pronks. Liikuge kerides allapoole täpsemat mis kestvus taseme ja võib kuluda väärtused.  

- Minimaalne arv VMs esmane sõlm tüüp määratakse töökindluse taseme, valite. Töökindluse taseme vaikeväärtus hõbe. Liikuge kerides allapoole, mis töökindluse taseme ja väärtused võib kuluda täpsemat.

- Teenuse struktuuri süsteemi teenuseid (nt halduri teenuse või pildi poe teenus) paigutatakse esmane sõlm tüüp ja seega töökindlust ja kestvus klaster määratakse töökindluse taseme väärtus ja kestvus taseme väärtus valite esmane sõlm tüüp.

![Kuvatõmmis kobar, mis sisaldab kahte tüüpi ][SystemServices]


### <a name="non-primary-node-type"></a>Mitte-esmane sõlm tüüp
Mitme sõlm andmetüüpidega klaster, seal on kirjas esmane sõlm ja ülejäänud need on peamise. Siin on omaduste peamise sõlm tüüp.

- VMs suurus sõlm selle tüübi puhul määratakse kestvus taseme, valite. Kestvus taseme vaikeväärtus pronks. Liikuge kerides allapoole täpsemat mis kestvus taseme ja võib kuluda väärtused.  

- Seda tüüpi sõlm VMs minimaalne arv võib olla üks. Siiski valida see number, mida soovite seda tüüpi sõlm käivitada rakenduste ja teenuste koopiate arvu põhjal. Kui olete juurutanud klaster tõsta VMs arv sõlm tüüp.


## <a name="the-durability-characteristics-of-the-cluster"></a>Klaster kestvus omadused

Kestvus taseme kasutatakse süsteemile õigused, mis on teie VMs aluseks Azure infrastruktuuri. Esmane sõlm tüübi, see õigus lubab teenuse struktuuri iga VM infrastruktuuri taotluse (nt VM taaskäivitage arvuti, VM reimage või VM migreerimise) mõjutada kvoorumi nõuded süsteemi teenuste ja stateful teenuste peatamiseks. Peamise sõlm tüübid, see õigus lubab teenuse struktuuri peatamiseks mis tahes VM infrastruktuuri koosolekukutse nagu VM taaskäivitage arvuti, VM reimage, VM migreerimise jne, mis mõjutavad kvooruminõuded stateful teenused ei tööta.

See õigus on väljendatud järgmised väärtused:

- Kuld - taristu tööde haldamine saate peatatud jaoks 2 tundi UD kohta

- Hõbe - taristu tööde haldamine saate peatatud kestuse 30 minutit kohta UD (see on praegu pole lubatud kasutamiseks. Kui lubatud, see on saadaval kõigi standard VMs ühe Core ja kohal).

- Pronksi - pole õigusi. See on vaikimisi.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Töökindluse omadused klaster

Töökindluse taseme abil saate seada vastuste süsteemi teenused, mida soovite rakendada see peamine sõlm tippige arv. Rohkem soovitud arv koopiad, on usaldusväärne süsteemi teenused on klaster.  

Töökindluse taseme saate teha järgmised väärtused.

- Platinum - süsteemi teenuseid koos siht koopia seadmine count / 9

- Kuld - süsteemi teenuseid koos siht koopia seadmine arvu 7

- Hõbe - süsteemi teenuseid koos siht koopia seadmine arvu 5

- Pronks - süsteemi teenuseid koos siht koopia seadmine arvu 3

>[AZURE.NOTE] Töökindluse taseme, valite määratleb väikseima arvu sõlmed peab olema oma peamine sõlm tüüp. Töökindluse taseme ei mõjuta klaster max maht. Nii saate määrata 20 sõlm kobar, mis töötab pronks töökindluse.

 Saate valida ühe taseme teise klaster usaldusväärsuse värskendada. See käivitab kobar täienduste vaja süsteemi teenuste koopia määrata arv. Oodake, kuni versioonitäienduse poolelioleva lõpuleviimiseks enne muude muudatuste tegemist klaster, nagu lisades sõlmed jne.  Saate jälgida edenemist versioonile teenuse struktuuri Explorer või [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) käivitades

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Kui olete lõpetanud oma võimsuse plaanimise klaster häälestada, lugege järgmist:
- [Teenuse struktuuri kobar turvalisus](service-fabric-cluster-security.md)
- [Teenuse struktuuri seisund mudeli tutvustus](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
