<properties
    pageTitle="Azure'i teenuse lõpp-punktid"
    description="Kirjeldatakse Azure'i tööriistakomplekt Eclipse Azure'i teenuse lõpp-punkti sätteid."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure'i teenuse lõpp-punktid #

Azure'i teenuse lõpp-punktid kindlaks teha, kas teie taotlus on juurutatud ja haldab globaalne Azure'i platvormi, Azure, mida käitab 21Vianet Hiinas või privaatse Azure'i platvormi. **Teenuse lõpp-punktid** dialoogiboksi võimaldab teil määrata, millised teenuse lõpp-punktid soovite kasutada. Jooksul Eclipse, **Teenuse lõpp-punktid** dialoogiboksi avamiseks klõpsake **aknas**nuppu, käsku **eelistused**, laiendage **Azure**ja klõpsake **Teenuse lõpp-punktid**. **Aktiivse seada** välja säte määratleb, mis Azure teenuse lõpp-punktid kasutatakse Azure projektide oma praeguse tööruumis.

Järgmisel joonisel on kujutatud dialoogiboks **Teenuse lõpp-punktid** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Teenuse lõpp-punktid määramiseks ##

**Teenuse lõpp-punktid** dialoogiboksis tehke ühte järgmistest toimingutest.

* Kui soovite kasutada globaalse Azure'i platvormi ripploendist **Aktiivne seadmine** valige **windowsazure.com** ja klõpsake nuppu **OK**.
* Kui soovite kasutada Azure, mida käitab 21Vianet Hiinas, ripploendist **Aktiivne seadmine** valige **windowsazure.cn (Hiina)** ja klõpsake nuppu **OK**.
* Kui soovite kasutada Azure'i platvormi privaatne:
    1. Klõpsake nuppu **Redigeeri**.
    2. Avaneb dialoogiboks teatega, et **Teenuse lõpp-punktid** dialoogiboks suletakse ja eelistused komplekti fail avatakse. Klõpsake nuppu **OK**.
    3. Preferencesets.xml faili, saate luua uue `preferenceset` element. Selle uue elemendi loomine `name`, `blob`, `management`, `portalURL` ja `publishsettings` omadused ja lisada oma isiklike Azure'i platvormi neid, mis vastavad väärtused. Saate kasutada väärtuste jaoks olemasoleva esitatud `preferenceset` elementide malle. **Märkus**: jaoks kasutatav väärtus on `blob` atribuut peab sisaldama tekst "bloobimälu" URL-i.
    4. Salvestage ja sulgege preferencesets.xml.
    5. Avage uuesti dialoogiboks **Teenuse lõpp-punktid** .
    6. Ripploendist **Aktiivne seadmine** valige loodud aktiivne määramine ja klõpsake nuppu **OK**.
    7. Kui olete loonud oma isiklike Azure'i platvormi `preferenceset` element, saate muuta selle **Teenuste lõpp-punkti** dialoogiboksis nuppu **Redigeeri** määratud väärtused. Samuti saate luua mitme privaatne Azure'i platvormi `preferenceset` elemente, kui soovite.

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
