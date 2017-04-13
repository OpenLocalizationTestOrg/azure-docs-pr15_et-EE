
<properties
    pageTitle="Tõrkeotsing loomise RemoteApp hübriid saidikogumid | Microsoft Azure'i"
    description="Saate teada, kuidas probleemide tõrkeotsing RemoteApp hübriid saidikogumi loomine"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Tõrkeotsing loomise Azure RemoteApp hübriid saidikogumid

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Hübriidjuurutuse saidikogumi on majutatud ja salvestab andmed Azure'i pilves, kuid ka võimaldab kasutajatel Accessi andmed ja ressursse, mis on talletatud teie kohalikus võrgus. Kasutajad pääsevad rakendused, logige sisse oma ettevõtte mandaadi sünkroonitud või Azure Active Directory ühendatud. Saate juurutada hübriid saidikogumi, mis kasutab olemasolevaid Azure virtuaalse võrgu või saate luua uue virtuaalse võrgu. Soovitame loomisel või Azure RemoteApp tulevaste kasvuga CIDR vahemiku jaoks piisavalt suur, virtuaalse alamvõrku abil.

Teie saidikogumi veel pole loonud? Juhised leiate teemast [loomine hübriid saidikogumi](remoteapp-create-hybrid-deployment.md) .

Kui teil on probleeme oma saidikogumi loomine või kui kogumine ei toimi nii, nagu teie arvates peaks, vaadake järgmist teavet.

## <a name="your-image-is-invalid"></a>Pilt ei sobi ##
Kui ootate Azure'i ettevalmistamise oma saidikogumi, kuvatakse järgmine teade, "GoldImageInvalid", see tähendab, et teie malli pilti ei vasta, [määratletud pilt nõuded](remoteapp-imagereqs.md). Avage nii, et lugeda nende [nõuete](remoteapp-imagereqs.md), lahendada oma pilt ja proovige uuesti oma saidikogumi loomiseks.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Kas teie VNET on määratletud võrgu turberühmad? ##
Kui teil on võrgu turberühmad alamvõrgu kasutate oma saidikogumi jaoks määratletud, veenduge, et need [URL-id ja pordid](remoteapp-ports.md) pääseb teie alamvõrku sees.

Saate lisada täiendavad võrgu turberühmad alamvõrgu rakendusekäivitiga juhtelemendi abil saate juurutada vms.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Kas kasutate oma DNS-serverid? Ja nad teie alamvõrku VNET pääseb? ##
>[AZURE.NOTE] Teil on veenduge, et teie VNET DNS-i serverid on alati ülespoole ja alati võimalik virtuaalmasinates, on VNET majutatud lahendada. Ärge kasutage seda Google DNS-i.


Hübriidjuurutuse saidikogumid saate ise oma DNS-serverid. Saate määrata need oma võrgu konfiguratsioon skeemi või kaudu haldusportaal virtuaalse võrgu loomisel. DNS-servereid kasutatakse määratletud viisil Tõrkesiirde (erinevalt round jaan) järjestuses.  
Vaadake veendumaks, et teie DNS-serverid on konfigureeritud aitab [Nimelahendus VMs ja rolli aknad](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) .

Veenduge, et teie saidikogumi jaoks DNS-i serverid juurdepääsetavat ja saadaval VNET alamvõrgu määratud selle saidikogumi jaoks.

Näiteks:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![DNS-i määratlemine](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Kas kasutate mõnda Active Directory domeenikontrolleri oma saidikogumi? ##
Praegu ainult ühe Active Directory domeeni võib olla seotud Azure RemoteApp. Hübriidjuurutuse saidikogumi toetab ainult Azure Active Directory kontod, mis on sünkroonitud tööriistaga DirSync kaudu Windows Server Active Directory juurutuse; eelkõige sellest, kas sünkroonitud parooli sünkroonimise suvandi või sünkroonitud Active Directory Federation Services (AD FS) liiduserveri konfigureeritud. Peate luua kohandatud domeeni, mis vastab teie domeeni kohapealse UPN-i domeeni järelliide ja kataloogi integreerimise häälestada.

Lisateabe saamiseks vaadake [Konfigureerimise Active Directory Azure RemoteApp](remoteapp-ad.md) .

Veenduge, et domeeni leiate kehtivad ja selle domeenikontrolleri on eesandmebaasis juurdepääsetavad alamvõrku kasutatakse Azure Remote App loodud VM. Lisaks veenduge, et selle teenuse konto mandaadid on õigused arvutite esitatud domeeni lisamiseks ja et esitatud AD nime saab lahendada lähtutud on VNET DNS-i kaudu.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Millist domeeninime ei määrata oma saidikogumi loomisel? ##

Teie loodud või lisatud domeeninimi peab olema sisemine domeeninime (mitte Azure AD domeeni nimi) ja peab olema lahutatavat DNS-i vormingus (contoso.local). Näiteks on Active Directory sisemine nimi (contoso.local) ja Active Directory UPN (contoso.com) – peate kasutama sisemine nimi, kui loote oma saidikogumi.
