<properties
    pageTitle="Suure juurutuste juurutamine"
    description="Saate teada, kuidas juurutada suurte juurutuste Azure'i tööriistakomplekt kasutamise Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Suure juurutuste juurutamine #

Kui teie juurutusega on liiga suur sisalduvate approot vaikekausta, saate kasutada kohalikku ressursi juurutamise juurkausta jaoks oma JDK ja rakenduse serveris.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Suure juurutuste juurutamise juurkausta kohalikku ressursi kasutamiseks ##

1. Saate luua uue kohaliku ressursi. Ressursi nimi pole oluline. Salvestusruumi ressursid on määratletud rolli tasemel. Kiireim viis kohaliku konfiguratsiooni dialoogi, kus saate luua uue kohaliku ressursi, juurdepääsuks on, kasutades järgmist: Paremklõpsake **Project Exploreri** vaade roll (laiendage oma Azure'i projekti sõlm, kui te ei näe roll), klõpsake **Azure**, ja seejärel nuppu **Kohalikku**. Klõpsake dialoogiboksis **Kohalikku** sees luua uue kohaliku ressursi **lisamine** .
1. Määrake soovitud suuruse vähemalt 2048 MB (midagi vähem võib põhjustada sama faili maht probleeme nagu sooviksite ilmneda on approot).
1. Veenduge, et **puhastada sisu kui rolli eksemplari toimub töötlemine** möllitud; See aitab takistada selle juurutamise käivitus loogika töötab konfliktide olemasolevaid faile ressursi taaskasutada roll eksemplari korral.
1. Veenduge, et **säilitamiseks ressursi kausta tee pärast juurutamise muutuja** väärtuseks on seatud string **DEPLOYROOT**. Teie kohaliku ressursi dialoogiboksi sarnanema järgmisega.
    ![][ic667943]

Teine võimalus, kui kasutate **DEPLOYROOT** teie kohaliku ressursi *nimi* ja muudate automaatselt loodud keskkonna muutuja nimi (mis seatakse **DEPLOYROOT_PATH** sel juhul), mis sobib ka rakenduse.

Kohaliku ressursi loomise kohta lisateabe leiate [kohalikku atribuudid][].

## <a name="see-also"></a>Vt ka ##

[Azure'i tööriistakomplekt Eclipse][]

[Tere tulemast maailma rakenduste loomise Azure Eclipse][]

[Azure'i tööriistakomplekt Eclipse installimine][] 

Azure'i Java kasutamise kohta leiate lisateavet teemast [Azure Java Arenduskeskus][].

<!-- URL List -->

[Azure'i Java Arenduskeskus]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure'i tööriistakomplekt Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tere tulemast maailma rakenduste loomise Azure Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure'i tööriistakomplekt Eclipse installimine]: http://go.microsoft.com/fwlink/?LinkId=699546
[Kohaliku atribuudid]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
