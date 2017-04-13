<properties
   pageTitle="Windows Serveri ja Linux Azure teenuse struktuuri kogumite loomine | Microsoft Azure'i"
   description="Teenuse kogumite käivitamine Windows Server ja Linux, mis tähendab, et teil võimalik juurutada ja host teenuse struktuuri rakendusi kõikjalt käivitada Windows Server või Linuxi struktuuri."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Opsüsteemis Windows Server ja Linux teenuse struktuuri kogumite loomine

Azure teenuse struktuuri võimaldab loomine teenuse struktuuri kogumite VMs või arvutid, kus töötab Windows Server või Linuxi jaoks. See tähendab, et teil on võimalik juurutada ja käivitada teenuse struktuuri rakendusi mis tahes keskkonnas, kui teil on Windows Server või Linuxi arvutite, mis on omavahel seotud, olgu see asutusesiseselt, Microsoft Azure'i või mis tahes pilveteenuste pakkujaga.

##<a name="create-service-fabric-clusters-on-azure"></a>Azure teenuse struktuuri kogumite loomine

Luua klaster Azure on valmis, kas ressursi mudeli malli või Azure portaali kaudu. Lugege lisateavet [loomine teenuse struktuuri kobar ressursihaldur malli abil](service-fabric-cluster-creation-via-arm.md) või [teenuse struktuuri kobar Azure portaali loomine](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Toetatud operatsioonisüsteemid kogumite Azure

Teil on võimalik luua kogumite VMs töötab need opsüsteemid:

* Windows Server 2012 R2
* Windows Server 2016 (kui see on teada üldiselt saadaval)
* Linux Ubuntu 16.04 (jaotises avaliku eelvaade) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Kohapealne või mis tahes pilveteenuste pakkujaga teenuse struktuuri autonoomse kogumite loomine

Teenuse struktuuri pakub mõne installi paketi loomiseks autonoomse teenuse struktuuri kogumite kohapealse või mis tahes pilveteenuse pakkuja

Autonoomse teenuse struktuuri kogumite Windows Server häälestamise kohta lisateabe saamiseks lugege [teenuse struktuuri kobar loomine Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Mis tahes pilve juurutuste vs kohapealse juurutuse
Teenuse struktuuri kobar kohapealse loomise protsess on sarnane luua klaster, mis tahes pilve teie valitud VMs kogum. Esimesed sammud ettevalmistamise VMs reguleerib pilveteenuse pakkuja või kohapealse keskkonna, mida te kasutate. Kui teil on võrguühendus lubatud nende vahel VMs kogum, siis juhiseid häälestada teenuse struktuuri paketti, kobar sätete redigeerimine ja käivitada kobar loomine ja haldus skriptide on täpselt ühesugused. See tagab, et teie teadmised ja kogemused kasutamise ja haldamise teenuse struktuuri kogumite on ülekantav, kui te ei vali uus majutusteenuse keskkonnas.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Luua eraldi teenuse struktuuri kogumite
* Teil on õigus valida klaster majutada mis tahes teenusepakkuja.
* Teenuse struktuuri rakendused, üks kord kirjutada, saab käitada mitut majutusteenuse keskkondade minimaalsete muudatused.
* Teenuse struktuuri rakenduste loomine tundmine viib üle ühe majutusteenuse keskkonnast teise.
* Kogemusi, töötab ja teenuse struktuuri haldamine kogumite viib üle ühest keskkonnast teise.
* Lai klientide REACHi on ääretu majutusteenuse piirangute.
* On töökindluse ja levinud katkestuste eest kiht olemas, sest saate liikuda teenuste teise juurutamise keskkonnas kui keskele või pilvepõhises andmepakkuja on elektrikatkestus.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Toetatud operatsioonisüsteemid autonoomse kogumite
Teil on võimalik luua kogumite VMs või need opsüsteemid kasutavad arvutid:

* Windows Server 2012 R2
* Windows Server 2016 (kui see on teada üldiselt saadaval)
* Linux (tulekul)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Teenuse struktuuri kogumite Azure teenuse struktuuri kogumite autonoomse üle eelised loodud kohapealse

Töötab teenuse struktuuri kogumite Azure pakub asutusesisese suvand eeliseid, seega kui teil pole vajadustele kui käivitate oma kogumite, siis soovitame neid käivitada Azure. Azure, pakume integreerimine muude Azure funktsioone ja teenuseid, mis muudab toimingute ja klaster haldamise hõlbustamiseks ja usaldusväärne.

* **Azure portaali:** Azure'i portaalis lihtne loomine ja haldamine kogumite.

* **Azure ressursihaldur:** Azure'i ressursihaldur kasutamine võimaldab lihtne hallata kõik ressursse, mis kasutavad klaster üksus ja lihtsustab kulude jälgimise ja arveldamine.
* **Teenuse struktuuri kobar, mis on Azure ressurss** Teenuse struktuuri kobar on ARM ressurss, nii, et saate seda nagu muud ARM ressursid Azure mudel.
* **Azure'i infrastruktuuri integreerimine** Teenuse struktuuri koordinaate aluseks Azure infrastruktuuri OS, võrgu ja muude täienduste parandamiseks kättesaadavus ja usaldusväärsus rakenduste abil.  
* **Diagnostika:** Azure, pakume integreerimine Azure diagnostika- ja Log Analytics.
* **Automaatne skaleerimist:** Kogumite Azure, pakume sisseehitatud automaatne skaleerimist funktsioonid tõttu virtuaalse masina skaala komplektid. Kohapealse ja muude cloud keskkonnas, peate või ise koostada automaatne skaleerimist funktsiooni abil käsitsi API-d, mis teenuse struktuuri seab skaleerimist kogumite skaala.

## <a name="next-steps"></a>Järgmised sammud
Looge klaster VMs või arvutites, kus töötab Windows Server: [teenuse struktuuri kobar loomine Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Luua klaster VMs või arvutid Linux: [Teenuse struktuuri Linux](service-fabric-linux-overview.md)
