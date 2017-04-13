<properties
  pageTitle="Azure'i muude teenustega Azure'i DNS-i abil | Microsoft Azure'i"
  description="Kuidas Azure'i DNS-i abil saate lahendada nimi muude Azure'i teenuste mõistmine"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Azure'i muude teenustega Azure'i DNS-i abil

Azure'i DNS-i on majutatud DNS-i halduse ja nimi eraldusvõime teenus. See võimaldab teil luua avaliku DNS-i nimed ning muude rakenduste ja teenuste juurutanud Azure. Kohandatud domeeni nimi Azure teenuse loomine on lihtne nagu teie teenuse jaoks õiget tüüpi kirje lisamine.

* Dünaamiliselt eraldatud IP-aadressid, peate looma DNS-i CNAME-kirjet, mida saab vastendada Azure'i teie teenuse jaoks loodud DNS-i nimi. DNS-i standardid ära kasutada zone tipp CNAME-kirje.
* Staatiliselt eraldatud IP-aadressid, saate luua mis tahes nimega, sh _alasti_ domeeninime tsooni tipus A DNS-i kirje.

Järgmises tabelis kirjeldatakse, mida saate kasutada mitmesuguste Azure toetatud kirjetüüpide. Nagu näete seda tabelit, toetab Azure DNS-i DNS-i kirjeid ainult Interneti-ühendusega võrgu ressursse. Azure'i DNS-i ei saa kasutada nimelahendus sisemise, Privaatne aadresse.

| Azure'i teenus | Võrgu liidese | Kirjeldus |
|---------------|-------------------|-------------|
| Rakenduse lüüsi | Ees avaliku IP | Saate luua DNS-i A- või CNAME-kirje. |
| Laadi koormusetasakaalustusteenuse | Ees avaliku IP | Saate luua DNS-i A- või CNAME-kirje. Laadi koormusetasakaalustusteenuse võib olla dünaamiliselt määratud IPv6 avaliku IP-aadressi. Seega peate looma CNAME-kirje IPv6-aadress. |
| Liikluse haldur | Avalik nimi | Saate luua ainult CNAME, mida saab vastendada trafficmanager.net määratud liikluse haldur profiili nimi. Lisateabe saamiseks lugege teemat [Kuidas liikluse haldur töötab](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Pilveteenuses | Avaliku IP | Staatiliselt eraldatud IP-aadressid, saate luua A DNS-i kirje. Dünaamiliselt eraldatud IP-aadressid, peate looma CNAME-kirjet, mida saab vastendada _cloudapp.net_ nimi. See reegel rakendub loodud klassikaline portaalis, kuna need on juurutatud pilveteenus VMs. Lisateavet leiate teemast [konfigureerimine pilveteenustega kohandatud domeeni nime](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Rakenduse teenus | Väline IP | Välise IP-aadressid, saate luua A DNS-i kirje. Muul juhul peate looma CNAME-kirjet, mida saab vastendada azurewebsites.net nimi. Lisateabe saamiseks lugege teemat [vastendamine kohandatud domeeninime Azure rakendusse](../app-service-web/web-sites-custom-domain-name.md) |
| Ressursihaldur VMs | Avaliku IP | Ressursihaldur VMs võib olla avaliku IP-aadressid. Laadi koormusetasakaalustusteenuse taga võib olla ka VM avaliku IP-aadressiga. Saate luua avaliku aadressi DNS-i A- või CNAME-i kirje. Mööda hiilida VIP klõpsake Laadi koormusetasakaalustusteenuse saab kasutada seda kohandatud nime. |
| Klassikaline VMs | Avaliku IP | Klassikaline VMs loodud PowerShelli kaudu või CLI saab konfigureerida dünaamilised või staatilised (reserveeritud) virtuaalse aadress. Saate luua DNS-i CNAME- või kirje, vastavalt. |
