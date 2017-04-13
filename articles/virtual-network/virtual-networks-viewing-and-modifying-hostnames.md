<properties 
   pageTitle="Vaatamine ja muutmine hostinimed | Microsoft Azure'i"
   description="Kuidas vaadata ja muuta hostinimed jaoks Azure'i virtuaalmasinates, veebi ja nimelahendus töötaja rollid"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Vaatamine ja muutmine hostinimed

Oma rolli aknad hostinimi viidatakse lubamiseks peate määrama hosti nimi väärtus konfiguratsioonifailis teenuse iga rolli. Saate seda teha, lisades soovitud hostinimi **vmName** atribuut elemendi **roll** . **VmName** atribuudi väärtust kasutatakse alusena iga rolli eksemplari hosti nimi. Näiteks kui **vmName** on *webrole* ja on kolm eksemplari rolli, linnanimede host nimed on *webrole0*, *webrole1*ja *webrole2*. Pole vaja hosti nimi virtuaalmasinates määramaks konfiguratsioonifailis, kuna virtuaalse masina nimi host sisustatakse virtuaalse masina nime järgi. Microsoft Azure'i teenus konfigureerimise kohta leiate lisateavet teemast [Azure teenuse konfiguratsioon skeemi (.cscfg fail)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Hostinimed kuvamine

Allpool tööriistade abil saate vaadata mõnes pilveteenuses virtuaalmasinates ja rolli aknad host nimed.

### <a name="azure-portal"></a>Azure'i portaal

[Azure'i portaali](http://portal.azure.com) abil saate vaadata hostinimed jaoks virtuaalmasinates ülevaade enne virtuaalse masina jaoks. Pidage meeles, et tera kuvatakse **nimi** ja **Serveri nimi**väärtus. Kuigi nad on esialgu sama, hostinimi muutmine ei muuda virtuaalse masina või rolli eksemplari nimi.

Azure'i portaalis saate vaadata ka rolli aknad, kuid kui olete loendi eksemplari mõnes pilveteenuses, ei kuvata hosti nimi. Kuvatakse iga eksemplari nimi, kuid selle nime ei ole hosti nimi.

### <a name="service-configuration-file"></a>Teenuse konfiguratsioonifail

Saate alla laadida teenuse konfiguratsioonifail juurutatud teenuse teenuse Azure portaali keelest **konfigureerimine** . Seejärel saate otsida **vmName** atribuut host nime kuvamiseks elemendi **rolli nimi** . Pidage meeles, et see hostinimi kasutatakse alusena iga rolli eksemplari hosti nimi. Näiteks kui **vmName** on *webrole* ja on kolm esinemisjuhud rolli, linnanimede host nimed on *webrole0*, *webrole1*ja *webrole2*.

### <a name="remote-desktop"></a>Kaugtöölaua

Kui olete lubanud kaugtöölaua (Windows), Windows PowerShelli remoting (Windows) või SSH (Windows ja Linux) ühendused virtuaalmasinates või rolli aknad, saate vaadata hosti nimi: aktiivne kaugtöölaua ühendus mitmel viisil:

- Tippige käsuviiba või SSH terminalis hostname (hostinimi).

- Tippige ipconfig/korraga käsk Küsi (ainult Windows).

- Saate vaadata arvuti nimi süsteemisätetest (ainult Windows).

### <a name="azure-service-management-rest-api"></a>Azure'i Teenusehaldus REST API-ga

ÜLEJÄÄNUD klientarvutist täitke järgmised juhised.

1. Veenduge, et on ühenduse Azure portaali kliendi serti. Kliendi serti saamiseks järgige toodud juhiseid [kohta: alla laadida ja impordisätete avaldamine ja tellimuse teave](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Määrake on päise kirje nimega x-ms-versiooni 2013-11-01 väärtusega.

1. Saata järgmises vormingus: https://management.core.windows.net/\<subscrition-id\>/teenused/hostedservices/\<teenuse nime\>?embed üksikasjade = true

1. Otsige **HostName** elemendi **RoleInstance** iga elemendi jaoks.

>[AZURE.WARNING] Saate vaadata sisedomeeninime järelliite jaoks oma pilveteenuses ülejäänud kõne vastust, märkides ruudu **InternalDnsSuffix** elemendi või käivitades ipconfig/kõik käsuviiba kaugtöölaua (Windows) või töötava kass /etc/resolv.conf on SSH terminalis (Linux) kaudu.

## <a name="modifying-a-hostname"></a>Hostinimi muutmine

Saate muuta hostinimi virtuaalse masina või rolli eksemplari muudetud teenuse konfiguratsiooni faili üleslaadimine või ümbernimetamine kaugtöölaua arvuti.

## <a name="next-steps"></a>Järgmised sammud

[Nimelahendus (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure'i teenus konfiguratsioon skeemi (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure'i Virtual Network Configuration skeemi](http://go.microsoft.com/fwlink/?LinkId=248093)

[Saate määrata DNS-i sätete failid võrgu kaudu](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
