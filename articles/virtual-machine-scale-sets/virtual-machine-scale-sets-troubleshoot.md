<properties
    pageTitle="Autoscale koos virtuaalse masina skaala komplekti tõrkeotsing | Microsoft Azure'i"
    description="Tõrkeotsing autoscale koos virtuaalse masina skaala komplektid. Tüüpilised probleemid ja nende lahendamiseks ette võtta mõistmiseks."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Autoscale koos virtuaalse masina skaala komplekti tõrkeotsing

**Probleem** – teie loodud autoscaling infrastruktuur rakenduses Azure'i ressursihaldur abil VM skaala komplektid – näiteks juurutamine malli umbes järgmine: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – teil on määratletud skaala reeglite ja see toimib hästi, välja arvatud, et ükskõik palju laadi, saate sellele VMs, siis ei ole autoscale.

## <a name="troubleshooting-steps"></a>Tõrkeotsingu juhiste

Mida peaks arvesse võtma järgmised.

- Mitu südamikud on iga VM ja teil on laadimise iga core?
 Ülaltoodud näites Azure'i Kiirjuhend mall on do_work.php skript, mis laadib ühe core. Kui kasutate VM, mis on suurem kui ühe core VM suurus, nt Standard_A1 või D1, siis peate mitu korda laadimine selle käivitamiseks. Märkige ruut mitu südamikud oma VMs vaadates [virtuaalmasinates suurused for Windows Azure](../virtual-machines/virtual-machines-windows-sizes.md)

- Mitu VMs VM skaala määramine, läheb iga töö?

    Sündmuse välja skaala ainult liikumisel teostatava toimingu keskmise CPU üle **Kõik** VMs skaala komplekti ületab läve väärtuse, aja jooksul sisemise määratletud autoscale reegleid.

- Jäta vahele skaala sündmused

    Kontrollige auditilogid Azure'i portaalis skaala sündmuste jaoks. Võib-olla oli skaala üles ja alla, mille skaala on vastamata. Saate filtreerida "Skaala".

    ![Auditilogide][audit]

- Oma skaala sisse ja välja skaala lävede piisavalt erinevad?

    Oletame, et saate määrata reegli mastaapimiseks välja, kui keskmine CPU on suurem kui 50% rohkem kui 5 minuti jooksul ja skaala kui keskmine CPU on väiksem kui 50%. See põhjustaks "lehvitamine" probleem, kui CPU on selle piirmäära lähedale skaala toimingute pidevalt suurendamine ja vähendamine määramine. Seetõttu autoscale teenuse proovib vältida "lehvitamine", mis saab nimega pole skaleerimist. Seega veenduge, et teie skaala-out ja skaala lävede erinevad piisavalt ruumi vahepeal skaleerimist lubama.

- Kas te kirjutada oma JSON malli?

    See on lihtne teha vigu, et alustada malli nagu üks, mille kohal on tõendada töö ja small suureneva muuta. 

- Saate käsitsi muudate sisse või välja?

    Proovige eri "võimsuse" sätte muutmiseks VMs arv käsitsi VM skaala seadmine Ressursi ümbersuunamine. Siin on mõni näide Mall toiming: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – võib-olla peate veendumaks, et enamasti on sama suur masina skaala seadmine kasutab malli redigeerimine. Kui VMs arvu saate muuta edukalt käsitsi, siis teate probleem on eraldatud autoscale.

- Kontrollige oma Microsoft.Compute/virtualMachineScaleSet ja [Azure Resource Explorer](https://resources.azure.com/) Microsoft.Insights ressursid

    See on asendamatu tõrkeotsingu tööriista, mis kuvatakse teie Azure'i ressursihaldur seisundi. Klõpsake oma tellimuse ja vaadake sooritate ressursirühma. Jaotises Arvuta ressursi pakkuja pilk VM skaala seadmine loodud ja märkige ruut eksemplari vaade, mis kuvatakse olek juurutamine. Lugege lisaks ka eksemplari vaate VMs VM skaala määramine. Seejärel minema Microsoft.Insights ressursi pakkuja ja märkige ruut autoscale Reeglid näevad head välja.

- Diagnostika laiend töötab ja tekitavate jõudlusandmeid?

    __Värskendus:__ Azure'i autoscale täiustatud host vastavalt mõõdikute kohaletoimetamisel, mis pole enam vaja diagnostika laiend installimiseks kasutada. See tähendab, et mõne tekstilõigu ei kehti enam kui loote uue müügivõimaluste rakendus autoscaling järgmise. Azure'i malle, mis on teisendatud host müügivõimaluste kasutamiseks näited: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Host vastavalt mõõdikute kasutamise autoscale on paremini järgmistel põhjustel.

    - Vähem liikuvad osad nimega pole diagnostika laiendid peate olema installitud.
    - Lihtsam malle. Lisage lihtsalt ülevaateid autoscale reeglid olemasoleva mastaapimiseks määramine malli.
    - Usaldusväärne aruandlus ja uue VMs kiirem käivitamine.

    Ainult põhjused, miks võiksite kasutada diagnostika laiend oleks, kui teil on vaja mälu diagnostika aruandlus/skaleerimist. Host vastavalt mõõdikute ei aruande mälu.

    Seda silmas pidades, järgige ainult käesoleva artikli ülejäänud kui te kasutate endiselt oma autoscaling diagnostika laiendid.

    Autoscale Azure ressursihaldur saate töötada (kuid ei ole enam) abil VM laiend nimetatakse diagnostika laiendamine. See eraldab jõudlusandmeid salvestusruumi konto määratlete malli. Andmed on koondatud seejärel Azure'i monitori teenus.

    Kui ülevaateid teenus ei saa andmeid lugeda vms, see peaks teile saata meilisõnumi – näiteks kui VMs alla, seega kontrollige oma e-posti (üks määratud loomisel Azure'i konto).

    Saate minna ja ise andmeid vaadata. Vaadake cloud Explorerit Azure storage konto. Näide [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), logige sisse ja valige Azure tellimuse kasutate ja diagnostika laiend määratluse juurutamise malli viidatud diagnostika salvestusruumi konto nimi.

    ![Cloud Explorer][explorer]

    Siin näete hulga tabeleid, kus iga VM andmed on salvestatud. Viimaste ridade Linux ja CPU meetermõõdustik, nagu näiteks vaadata. Visual Studio cloud explorer toetab päringukeele, siis saate kasutada päringu nagu "ajatempli gt datetime'2016-02-02T21:20:00Z'" veendumaks, et saate viimase sündmused (endale aega on UTC-vormingus). Kas näete ei vasta skaala häälestamisel andmeid? Järgmises näites, masina 20 CPU alustamine suurendab viimase viie minuti üle 100%.

    ![Salvestusruumi tabelid][tables]

    Kui andmed ei ole olemas, siis see tähendab, probleem on diagnostika laiendiga VMs töötab. Kui andmed on olemas, tähendab see, on skaala reeglite või ülevaateid teenus on probleeme. [Azure'i olekut](https://azure.microsoft.com/status/)saate kontrollida.

    Kui olete neid juhiseid järgides, kui teil on endiselt probleeme autoscale te proovige foorumeid [MSDN-i](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)või [virnastada ületäitumine](http://stackoverflow.com/questions/tagged/azure)või kõne tugi sisse logida. Olge Mall ja jõudluse andmeid vaate jagamiseks.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
