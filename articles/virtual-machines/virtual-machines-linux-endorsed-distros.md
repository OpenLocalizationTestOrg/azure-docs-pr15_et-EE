<properties
    pageTitle="Kinnitatud Linux jaotamine | Microsoft Azure'i"
    description="Lisateavet Linux Azure'i kinnitatud jaotuse, sealhulgas juhised Ubuntu, OpenLogic, Oracle'i ja SUSE kohta."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linuxi Azure'i kinnitatud üldiste müügijaotustega tutvumine

> [AZURE.NOTE] Kui teil on mõne hetke aega, aidake meil täiustada Azure Linux VM dokumentatsiooni võttes selle [kiire küsitluse](https://aka.ms/linuxdocsurvey) oma kogemusi. Iga vastus aitab aitavad teil oma töö valmis.

Azure'i Galerii või turuplatsi Linux pildid on esitatud partnerite arv ja erinevate Linuxi ühenduste lisada rohkem maitsed kinnitatud leviloendi töötavad. Vahepeal jaoks pole saadaval jaotuse galeriist saate alati toovad teie-omanik-Linux täitke [sellel](virtual-machines-linux-classic-create-upload-vhd.md)lehel toodud juhised.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Toetatud jaotuse & versioonid ##

Järgmises tabelis on loetletud Linuxi ja versioonid, mis on toetatud Azure. Vaadake ka [kasutajatugi Linux pildid Microsoft Azure'i](https://support.microsoft.com/en-us/kb/2941892) üksikasjalikumat teavet.

Hyper-V ja Azure draiverid Linux Integration Services (Poolelioleva) on Microsofti panus otse varustava Linuxi tuum tuum moodulid.  Poolelioleva draiverid on kas sisse ehitatud selle jaotuse tuum vaikimisi või vanemate RHEL/CentOS-ga jaotuse on eraldi allalaadimiseks saadaval [siin](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Lugege [artiklit](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) Poolelioleva draiverid kohta lisateavet.

Azure'i Linux Agent on juba eelinstallitud opsüsteemi Azure'i galerii pildid ja selle jaotuse paketi hoidla on tavaliselt kättesaadav.  Lähtekoodi leiate [github](https://github.com/azure/walinuxagent).

Jaotuse.|Versioon|Draiverid|Agent
---|---|---|---
CentOS OpenLogic järgi | CentOS 6.3 + 7.0 + | CentOS 6.3: [Poolelioleva allalaadimine](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: Rakenduses tuum | Pakett: Rakenduses [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) jaotises "WALinuxAgent" <br/>Lähtekoodi: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Klõpsake tuum | Lähtekoodi: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7,9 +, 8,2 + | Klõpsake tuum | Pakett: rakenduses repo jaotises "waagent" <br/>Lähtekoodi: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle'i Linux | 6.4 +, 7.0 + | Klõpsake tuum | Pakett: rakenduses repo jaotises "WALinuxAgent" <br/>Lähtekoodi: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Punane rolli Enterprise Linux | RHEL 6,7 + 7.1 + | Klõpsake tuum|Pakett: rakenduses repo jaotises "WALinuxAgent" <br/>Lähtekoodi: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + ja <p> SAP-i 11 SP3 + SLES | Klõpsake tuum | Pakett: Klõpsake [Tööriistad: pilveteenuste](https://build.opensuse.org/project/show/Cloud:Tools) repo jaotises "python-Azure'i – agent" <br/>Lähtekoodi: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | Klõpsake tuum | Pakett: Klõpsake [Tööriistad: pilveteenuste](https://build.opensuse.org/project/show/Cloud:Tools) repo jaotises "python-Azure'i – agent" <br/>Lähtekoodi: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Klõpsake tuum | Pakett: rakenduses repo jaotises "walinuxagent" <br/>Lähtekoodi: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partnerite

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic on ees pakkuja ettevõtte avatud allika lahendused pilveteenuses ja andmekeskuse. OpenLogic aitab sadu ees enterprise mitmes sidumine turvaline omandada, toetavad ja avage kontrollida. OpenLogic pakub äriliseks äriklassi tehnilise toe poole ja kahjude hüvitamine 600 avatud allikas pakette OpenLogic ekspert ühenduse, sh enterprise taseme tugi CentOS samuti on käivitada partneri osutamiseks CentOS-põhiste piltide Azure toetavad.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/Running-coreos/Cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS veebisaidilt:

*CoreOS on mõeldud turvalisus, järjepidevuse ja usaldusväärsuse. Asemel pakettide yum kaudu või apt installimist CoreOS kasutab Linux ümbriste hallata oma teenuste võtmiseks kõrgemal tasemel. Ühe teenuse kood ja kõik sõltuvused on pakitud container, ühe või mitu CoreOS masinad töötavad sees.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-Blog/Debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ on sõltumatu nõustamisteenuseid ja ettevõtte, loomine ja rakendamine professionaalse lahenduste tasuta tarkvara abil. Kui ees avatud allika spetsialistid, on Credative rahvusvaheline tuvastus paljud, kasutades oma IT-osakonna. Koos Microsoft, Credativ valmistab praegu vastavate Debian piltide Debian 8 (Jessie) ja Debian enne 7 (hingeldav), mis käivitamiseks Azure spetsiaalselt ja saab hõlpsasti hallata platvormi kaudu. Credativ toetatakse ka pikaajaline hooldus ja värskendamise Debian piltide Azure selle Ava allikas tugi andmekeskuste kaudu.

### <a name="oracle"></a>Oracle'i
[http://www.Oracle.com/technetwork/topics/Cloud/FAQ-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle'i strateegia on pakkuda üldiseid portfellivalikuotsuse lahendusi avalike ja privaatvõ pilved, andes kliendid valida, kas ja kuidas need juurutamine Oracle tarkvara Oracle'i pilved kui ka muude pilved paindlikkust.  Oracle'i koostöös Microsoft võimaldab Oracle'i tarkvara Microsoft avalike ja privaatvõ pilve sees sertimine usaldus juurutada ja tuge Oracle'i kliendid.  Oracle'i kohustuse ja Oracle'i avalike ja privaatvõ pilve lahendusi investeeringud ei muutu.

### <a name="red-hat"></a>Punane rolli
[http://www.RedHat.com/en/Partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Maailma andja avatud allika lahendusi, punane rolli aitab rohkem kui 90% Fortune 500 ettevõtete business probleeme lahendada, joondada oma IT ja, ja tulevikus tehnoloogia ettevalmistamine. Punane rolli teeb seda mudelit, mis avatud äri ja taskukohane, prognoositavad tellimuse mudelit, mis kaudu turvalist lahendusi.

### <a name="suse"></a>SUSE
[http://www.suse.com/SuSE-Linux-Enterprise-server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server Azure'i on tõestatud platvorm, mis pakub töökindlus ja turve cloud arvutuste. SUSE's mitmekülgne Linux platvormi integreerub sujuvalt Azure pilveteenused esitamisel hõlpsalt mõistliku cloud keskkonnas. Ja rakendustega rohkem kui 9200 sertifitseeritud rohkem kui 1800 sõltumatu tarkvara tarnijate SUSE Linux Enterprise server, SUSE tagab, et töökoormus töötab toetatud andmekeskuse saab kindlalt juurutada Azure.

### <a name="canonical"></a>Kanoonilise
[http://www.Ubuntu.com/Cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanoonilise matemaatika ja avatud ühenduse juhtimise ketas Ubuntu edu klient, server ja arvuti, sh isiklik pilveteenustega tarbijatele pilve. Nägemispuudega Canonical on ühendatud tasuta platvormi Ubuntu, telefonist Cloud pere ühtse liidesed telefoni, tahvelarvuti, Televiisor ja töölaud, teeb Ubuntu mitmesuguse avaliku pilveteenuse pakkujate tarbekaubad tegijad ja lemmikute hulka üksikuid tehnoloogid õppeasutuste esimene valik.

Arendajad ja engineering andmekeskuste kogu maailmas, paigutust Canonical kordumatult partner riistvara tegijad, sisu pakkujate ja tarkvaraarendajad Ubuntu lahendusi, turu, serverid ja pihuseadmetele arvutitest.

