<properties
    pageTitle="Kuidas seadistada VPN-ühendused Azure'i API haldus"
    description="Saate teada, kuidas Azure'i API haldus ja selle kaudu juurdepääsu veebiteenuste VPN-ühenduse häälestamine."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Kuidas seadistada VPN-ühendused Azure'i API haldus

API halduse VPN-i tugi võimaldab ühendada API andmehalduslüüsi Azure virtuaalse võrku (klassikaline). See võimaldab API halduse kliendid turvaliselt ühenduse loomiseks oma kirjutamata veebiteenuseid, mis on kohapealse või muul viisil kättesaamatuks avaliku Interneti-ühendus.

>[AZURE.NOTE] Azure'i API halduse töötab klassikaline VNETs. Klassikaline VNET loomise kohta leiate teavet teemast [loomine (klassikaline), kasutades Azure portaali virtuaalse võrgus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Klassikaline VNETs ühenduse ARM VNETS kohta leiate teavet teemast [ühenduse loomine klassikaline VNets uue VNets abil](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Lubada VPN-ühendused

>Virtuaalse Privaatvõrgu ühendus kättesaadav ainult **Premium** ja **arendaja** astme. Selle aktiveerimiseks avage API halduse teenust [Azure klassikaline portaali][] ja seejärel vahekaarti **skaala** . **Üldist** jaotises Valige Premium taseme ja klõpsake nuppu Salvesta.

Virtuaalse Privaatvõrgu ühendus lubamiseks avage API halduse teenust [Azure klassikaline portaali][] ja **konfigureerimine** vahekaardi aktiveerimine. 

Jaotises VPN aktiveerige **VPN-ühenduse** **kohta**.

![Menüü eksemplari API halduse konfigureerimine][api-management-setup-vpn-configure]

Nüüd kuvatakse kõik regioonid, kus on ette valmistatud API halduse teenust loendit.

Valige VPN- ja alamvõrgu iga piirkonna jaoks. Loendi VPN sisustatakse virtuaalse võrgud saadaval Azure tellimuse, mis on häälestamise konfigureerite piirkonna põhjal.

![Valige VPN][api-management-setup-vpn-select]

Klõpsake nuppu **Salvesta** ekraani allosas. Te ei saa teha muude toimingute API halduse teenuse Azure klassikaline portaalist samal ajal, kui see on värskendamine. Teenuse lüüsi jäävad ja käitusaja kõned ei tohiks mõjutada.

Pange tähele, et lüüsi VIP-aadressi muudab iga kord, kui VPN on lubatud või keelatud.

## <a name="connect-vpn"> </a>Ühenduse veebiteenuse VPN taha

Pärast API halduse teenust on ühendatud VPN, web services virtuaalse võrgustikus juurdepääs ei erine avaliku teenuste kasutamiseks. Ainult kohaliku aadress või hosti nimi (kui DNS-i server on konfigureeritud Azure virtuaalse võrgu) oma veebiteenuse **veebiteenuse URL** väljale tippida uue API loomisel või olemasoleva redigeerimiseks.

![Lisage API VPN][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>API halduse VPN-i tugiteenuste vajalikud pordid

Kui API halduse teenuse eksemplar on majutatud on VNET, kasutatakse pordid järgmises tabelis. Kui järgmised pordid on blokeeritud, ei pruugi teenuse õigesti töötada. Ühe või mitme need pordid blokeeritud on kõige sagedamini vale konfiguratsioon on probleem on VNET API halduse kasutamisel.

| Port(s))                      | Suund        | Transport Protocol (protokoll) | Otstarve                                                          | Andmeallika / sihtkoht              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Sissetulev          | TCP                | Kliendi side API haldus                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Väljamineva meili         | TCP                | Azure'i salvestus- ja Azure teenuse siini API halduse sõltuvus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Väljamineva meili         | TCP                | SQL-i halduse API sõltuvused                               | VIRTUAL_NETWORK / INTERNET        |
| 9350 9351 9352, 9353, 9354 | Väljamineva meili         | TCP                | Teenuse siini API halduse sõltuvused                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Väljamineva meili         | AMQP               | Sündmuse jaoturi poliitika Logi API halduse sõltuvus            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Sissetulev/väljaminev liiklus | UDP                | Redis vahemälu halduse API sõltuvused                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Väljamineva meili         | TCP                | API halduse sõltuvus git Azure faili ühiskasutus            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Kohandatud DNS-i serveri häälestamine

API halduse sõltub Azure'i teenuste arv. Kui API halduse teenuse eksemplar on majutatud on VNET, kus kasutatakse kohandatud DNS-i server, see peab olema nende Azure'i teenuste hostinimed lahendada. Järgige [juhised](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) kohandatud DNS-i häälestamise kohta.  

## <a name="related-content"> </a>Seotud sisu


* [Virtuaalse võrgu klassikaline portaalis Azure-saidilt VPN-ühenduse loomine][]
* [Jälgida inspektor API kasutamine kutsub Azure'i API haldus][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure'i klassikaline portaal]: https://manage.windowsazure.com/

[Virtuaalse võrgu klassikaline portaalis Azure-saidilt VPN-ühenduse loomine]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Jälgida inspektor API kasutamine kutsub Azure'i API haldus]: api-management-howto-api-inspector.md
