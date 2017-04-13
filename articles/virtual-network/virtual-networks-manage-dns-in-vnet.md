<properties 
   pageTitle="Halda DNS-serverid kasutada virtuaalse võrgu (VNet)"
   description="Saate teada, kuidas lisada ja eemaldada DNS-serverid virtuaalse võrku (vnet)"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Halda DNS-serverid kasutada virtuaalse võrgu (VNet)

Saate hallata kasutatud VNet, haldusportaal või võrgu konfiguratsioonifailis nimeservereid loendist. Saate lisada kuni 12 DNS-serverid iga VNet jaoks. Kui soovite määrata DNS-serverid, on oluline, kinnitamaks, et te nimekirja oma DNS-serverid keskkonna õiges järjestuses. DNS-i serveri loendeid ei toimi round-jaan. Neid kasutatakse on määratud järjestuses. Kui esimene DNS-i server loendis saab teile helistada, kasutab kliendi selle DNS-i serveri sõltumata sellest, kas DNS-i server töötab korralikult või mitte. Virtuaalse võrgu jaoks DNS-i serveri järjestuse muutmiseks nimeservereid loendist eemaldada ja neid uuesti selles järjekorras, mida soovite lisada.

>[AZURE.WARNING] Pärast DNS-i loend on värskendatud, tuleb taaskäivitada virtuaalmasinates, mis asub virtuaalse võrgu nii, et nad kättesaamine uue DNS-i serveri sätted. Virtuaalmasinates kasutab ka edaspidi kasutada oma praeguse konfiguratsiooni, kuni ta uuesti.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>DNS-i server loendi haldusportaali abil virtuaalse võrgu redigeerimine

1. Logige **haldusportaali**.

1. Klõpsake navigeerimispaanil, klõpsake **võrgu**ja klõpsake virtuaalse võrgu **nimi** veeru nimi.

1. Klõpsake nuppu **Konfigureeri**.

1. **DNS-serverid**, saate konfigureerida järgmist:

    - **Registreerida (lisa) uue DNS-i server –** Lihtsalt Tippige väljadele nimi ja IP-aadress. DNS-i server lisatakse virtuaalse võrgu DNS-serverid loendi ja ka registreerib DNS-i server Azure'i.

    - **DNS-i server, mis on varem registreeritud – lisamine** Kui olete juba registreerunud DNS-i server Azure'i, saate selle valida juba eelnevalt täidetud loendist.

    - **DNS-i server oma virtuaalse võrgust – eemaldamine** Kõrval nuppu X server, mille soovite eemaldada. Pange tähele, et see eemaldab ainult server virtuaalse võrgu loendist. DNS-i server on endiselt registreeritud Azure oma virtuaalse võrgu jaoks kasutada. DNS-i serveri kustutamine tellimusest, minge lehele **võrgu -> DNS-serverid** .

    - **DNS-serverid – ümber korraldada** Kõik DNS-serverid, mis on loetletud ja lisage need uuesti järjestuses, mille soovite eemaldada. Pidage meeles, et see ei ole round-jaan DNS-i loendi.

    - **DNS-i server – ümbernimetamine** DNS-i server loendis esile ja tippige uus nimi. See on registreerida uus DNS-i server Azure'i ning selle virtuaalse võrgu jaoks DNS-serverid loendisse lisada. Vana DNS-serveri ja IP-aadress jääb Azure registreeritud. Kui te ei kasuta seda virtuaalse võrgu jaoks, saate selle lehel **DNS-serverid** kustutada.

1. Klõpsake nuppu **Salvesta** lehe allosas salvestada oma uue DNS-serverite konfiguratsiooni.

1. Taaskäivitage virtuaalmasinates, mis asub virtuaalse võrgu, et nad saaksid hankida uue DNS-i sätted.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>DNS-i serveri loendeid, mis kasutavad võrgu konfigureerimise faili redigeerimine

DNS-i serveri loend, kasutades võrgu konfigureerimise faili redigeerimiseks tuleb esmalt sätete konfigureerimine haldamine portaalis eksportida. Seejärel saate võrgu konfigureerimise faili redigeerimiseks ja importige see uuesti kaudu haldusportaal. Allpool on üksikasjalik loend selle protsessi lõpuleviimiseks juhised.

1. Virtuaalne võrgusätted võrgu konfigureerimise faili eksportida. Vaadake [Eksportimine virtuaalse võrgu sätteid võrgu konfigureerimise faili](virtual-networks-using-network-configuration-file.md)teabe ja konfiguratsiooni võrgusätted eksportimiseks tehke järgmist.

1. Saate määrata DNS-i serveri teave virtuaalse võrgu jaoks. DNS-i serveri määramise kohta leiate lisateavet teemast [DNS-i serveri konfiguratsioonifailis virtuaalse võrgu täpsustades](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Failide võrgu konfigureerimise kohta lisateabe saamiseks lugege teemat [Azure virtuaalse võrgu konfigureerimise skeemi](https://msdn.microsoft.com/library/azure/jj157100.aspx) ja [konfigureerida virtuaalse võrgu kaudu võrgu konfigureerimise faili](virtual-networks-using-network-configuration-file.md).

1. Võrgu konfigureerimise faili importimine. Lisateavet ja juhiseid oma võrgu konfigureerimise faili importimise kohta leiate artiklist [võrgu konfigureerimise faili importimine](virtual-networks-using-network-configuration-file.md).

1. Taaskäivitage virtuaalmasinates, mis asub virtuaalse võrgus, et omandada uue DNS-i sätted.
