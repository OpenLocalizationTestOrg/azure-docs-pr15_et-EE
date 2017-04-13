<properties
    pageTitle="Automaatne mastaapimiseks pilveteenus portaalis | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida automaatne skaala reeglid pilvepõhise teenuse web rolli või töötaja rolli Azure portaali abil."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Kuidas automaatne skaala pilveteenus

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-how-to-scale-portal.md)
- [Azure'i klassikaline portaal](cloud-services-how-to-scale.md)

Pilveteenuse teenuse töötaja roll skaala sisse- või toimingu käivitamine saab määrata tingimused. Roll tingimuste põhjal CPU, kettale või võrgu koormus roll. Samuti saate conditation, mis on sõnumi järjekorda või mõne muu Azure ressursiga teie tellimusega seostatud mõõdiku põhjal.

>[AZURE.NOTE] See artikkel keskendub pilveteenuses veebi ja töötaja rollid. Kui loote virtuaalse masina (klassikaline) otse, see on majutatud mõnes pilveteenuses. Saate mastaapimiseks standard virtuaalse masina järgi seostada see on [kättesaadavus](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) ja käsitsi need sisse või välja lülitada.

## <a name="considerations"></a>Kaalutlused

Enne konfigureerida rakenduse skaleerimist arvestage järgmist teavet.

- Skaleerimist ei mõjuta core kasutus. Suurema rolliga eksemplari kasutada rohkem tuuma. Muudate ainult piires valdkond rakenduse tellimuse. Näiteks kui teie tellimus on kuni kakskümmend ja -vormid saate käivitada rakenduse koos kahe keskmise suurusega cloud services (kokku neli valdkond), saate ainult skaalal muude pilvepõhise teenuse juurutuste teie tellimus 16 puursüdamikproovide. Vt [Pilvepõhise teenuse suurused](cloud-services-sizes-specs.md) suurused kohta lisateavet.

- Saab aluseks on järjekorda sõnum. Järjekorrad kasutamise kohta lisateabe saamiseks vaadake, [Kuidas kasutada salvestusteenus järjekorda](../storage/storage-dotnet-how-to-use-queues.md).

- Samuti saate skaala muud ressursid, mis on teie tellimusega seostatud.

- Rakenduse kättesaadavuse lubamiseks veenduge, et see on juurutatud kahe või enama rolli aknad. Lisateavet leiate teemast [Taseme lepingute](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Kus asub skaala

Valige oma pilveteenuses, peaks teil olema pilvepõhise teenuse tera nähtav.

1. Valige pilvepõhise teenuse enne paanil **rollid ja eksemplari** nimi pilveteenusesse.   
**Tähtis**: Veenduge, et nuppu pilvepõhise teenuse rolli, mitte rolli eksemplari, mis on allpool roll.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Valige paan **skaala** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automaatse skaala

Saate konfigureerida kahel viisil **käsitsi** või **automaatse**rolli skaala sätteid. Käsitsi ootuspäraselt, saate määrata absoluutne eksemplaride arv. Automaatne, kuid lubab teil määrata reeglid, mis haldavad, kuidas ja kui palju peaksite skaala.

Valige suvand **skaala järgi** **plaanimine ja jõudluse reeglid**.

![Cloud services skaala sätteid profiili ja reegel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Olemasolevale profiilile.
2. Reegli ema profiili lisamine.
3. Lisage teine profiil.

Valige **Lisa profiil**. Profiili määratleb, milliseid režiimi soovite kasutada skaala: **alati**, **Korduvus**, **kindel kuupäev**.

Kui olete konfigureerinud profiili ja reegleid, valige **salvestamine** ülaosas ikooni.

#### <a name="profile"></a>Profiil

Profiili määrab skaala, miinimum- ja eksemplari ja ka siis, kui see skaala vahemikus on aktiivne.

* **Alati**

    Hoidke selles vahemikus eksemplaride saadaval.  

    ![Mis on alati mastaapimiseks pilveteenuses](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Korduvus**

    Valige nädalapäevad mastaapimiseks kogumi.

    ![Teenus Cloud skaala Korduvus ajakava](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Kindla kuupäeva**

    Fikseeritud kuupäevavahemiku mastaapimiseks roll.

    ![Teenus cLoud skaala kuupäeva](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Kui olete konfigureerinud profiili, valige profiili tera allservas nuppu **OK** .

#### <a name="rule"></a>Reegli

Reeglite lisatakse profiili ja tähistada tingimus, mis käivitab skaala. 

Reegli päästik põhineb mõõdiku, mille saate lisada tingimusvormingu väärtus pilveteenuses (CPU hõivatus, ketta aktiivsust või võrgu tegevuse). Lisaks saate määrata päästik, sõnumi järjekorda või mõne muu Azure ressursiga teie tellimusega seostatud mõõdiku põhjal.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Kui olete konfigureerinud reegel, valige reegli tera allservas nuppu **OK** .

## <a name="back-to-manual-scale"></a>Naasmiseks käsitsi skaala

Liikuge [skaala sätteid](#where-scale-is-located) ja seadke mastaap **,** **eksemplari arv, mis käsitsi sisestada**.

![Cloud services skaala sätteid profiili ja reegel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

See eemaldab automatiseeritud skaleerimist kaudu roll ja seejärel saate määrata eksemplari arv otse. 

1. Skaala (käsitsi või automatiseeritud) suvand.
2. Roll eksemplari liugurit eksemplarid ulatusega, et määrata.
3. Roll ulatusega, et eksemplarid.

Kui olete konfigureerinud skaala sätteid, valige **salvestamine** ülaosas ikooni.

