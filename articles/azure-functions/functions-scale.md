<properties
   pageTitle="Kuidas mastaapimiseks Azure funktsioonid | Microsoft Azure'i"
   description="Mõista, kuidas oma sündmuse põhinev töökoormus vajaduste rahuldamiseks mastaapimiseks Azure'i funktsioonid."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Kuidas mastaapimiseks Azure funktsioonid

## <a name="introduction"></a>Sissejuhatus

Azure'i funktsioonide eelis on see, et Arvuta ressursid ainult tarbitud vastavalt vajadusele. See tähendab, et te ei maksta jõude vms või reserveerida võimsus kui peate töötama. Selle asemel platvormi eraldab Arvuta power, kui teie kood töötab, skaala vajadusel käsitlema laadi ja seejärel skaala, kui kood ei tööta.

Selle jaoks selle uue võimaluse on dünaamiline teenusleping.  

Selles artiklis antakse ülevaade dünaamiline teenusleping tööpõhimõte ja kuidas platvormi nõudmisel oma koodi käivitada.

Kui te pole veel Azure'i funktsioonidega tuttav, veenduge, et kontrollida [Azure'i funktsioonide ülevaate](functions-overview.md) artikli paremini mõistmiseks võimalusi.

## <a name="configure-azure-functions"></a>Azure'i funktsioonide konfigureerimine

Kaks peamist sätted on seotud skaleerimist:

* [Azure'i rakenduse teenusleping](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) või dünaamiline teenuselepingu raames
* Mälumaht täitmise keskkonnas

Funktsiooni kulu muutub sõltuvalt teie valitud teenusleping. Dünaamiline teenusleping maksumus on funktsiooni täitmisaeg, mälu suurus ja täitmised arv. Kulude kogu ainult kui kood on tegelikult töötab.

Mõni rakendus teenusleping majutab oma funktsioonide olemasoleva VMs, mida võib kasutada ka muid koodi käivitada. Kui maksate need VMs iga kuu, on tasuta täitmise funktsioonide neid.

## <a name="choose-a-service-plan"></a>Valige teenuse leping

Funktsioonide loomisel saate kasutada neid dünaamiliseks teenusleping või [rakenduse teenusleping](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Rakenduse teenusleping töötab teie funktsioonide spetsiaalne VM, nagu web Appsi tööd täna Basic, Standard või Premium SKU-de jaoks.
See sihtotstarbeline VM on eraldatud oma rakenduste ja funktsioonide ja on alati saadaval, kas kood aktiivselt käivitatakse või mitte. See on hea valik, kui teil on olemasolev, klõpsake jaotises kasutatud VMs, mis on juba muu koodi või kui eeldate, et käivitada funktsioonid pidevalt või peaaegu pidevalt. VM decouples maksumus: nii käitusaja ja mälu suurus. Selle tulemusena saate piirata kulu palju pikaajalisi funktsioone töötada ühe või mitme vms hinnale.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
