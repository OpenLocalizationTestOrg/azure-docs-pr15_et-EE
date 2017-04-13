<properties
    pageTitle="Azure'i AD domeeniteenused: Võrgunduse juhised | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused võrgunduse kaalutlused"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Azure'i AD domeeniteenused võrgunduse kaalutlused

## <a name="how-to-select-an-azure-virtual-network"></a>Mõne Azure virtuaalse võrgu valimine
Järgmised juhised aitavad teil valige virtuaalse võrgu Azure AD domeeni teenustega kasutamiseks.

### <a name="type-of-azure-virtual-network"></a>Azure virtuaalse võrgu tüüp

- Saate lubada Azure AD domeeniteenused klassikaline Azure virtuaalse võrku.

- Azure'i AD domeeniteenused **ei saa lubada Azure ressursihaldur abil loodud virtuaalse võrgu**.

- Saate luua ühenduse ressursihaldur vastavalt virtuaalse võrgu klassikaline virtuaalse võrk, mille Azure AD domeeniteenused on lubatud. Pärast seda, saate kasutada Azure AD domeeniteenused ressursihaldur vastavalt virtuaalse võrgu. Lisateavet leiate jaotisest [võrguühendus](active-directory-ds-networking.md#network-connectivity) .

- **Virtuaalne piirkondliku võrke**: kui kavatsete kasutada olemasolevat virtuaalse võrku, veenduge, et see on piirkondliku virtuaalse võrgu.

    - Azure'i AD domeeniteenused ei saa kasutada virtuaalse võrkudes, mis kasutavad pärand osaleja rühmade süsteem.

    - [Migreerimine pärand virtuaalse võrgu virtuaalse piirkondliku võrke](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)Azure AD domeeni teenuste kasutamiseks.


### <a name="azure-region-for-the-virtual-network"></a>Azure'i piirkond virtuaalse võrgu jaoks

- Azure'i AD domeeni teenuste hallatava domeeni juurutatakse virtuaalse võrgu Azure sama piirkonna võite teenuse sisse.

- Azure'i AD domeeniteenused ei toeta Azure piirkonnas virtuaalse võrgu valimine

- Teemast [Azure teenuste regiooniti](https://azure.microsoft.com/regions/#services/) lehe leida Azure piirkonnad, mille Azure AD domeeniteenused on saadaval.


### <a name="requirements-for-the-virtual-network"></a>Virtuaalse võrgu nõuded

- **Oma Azure'i töökoormus lähedus**: virtuaalse võrk, mis on praegu majutab/kuvatakse host virtuaalmasinates, mida on vaja juurdepääsu Azure AD domeeni teenuste valimine.

- **Kohandatud/tuua-oma-enda DNS-serverid**: Veenduge, et oleks pole kohandatud DNS-serverid virtuaalse võrgu jaoks konfigureeritud.

- **Olemasolev domeene sama domeeninime**: Veenduge, et teil pole saadaval, et virtuaalse võrgus domeeni sama nimega olemasoleva domeeni. Näiteks Oletame, et teil on domeeni nimega contoso.com juba saadaval valitud virtuaalse võrgus. Hiljem proovite lubada Azure AD domeeniteenused hallatava domeeni domeeni nimi (see on "contoso.com") selles virtuaalse võrgus. Luba Azure AD domeeniteenused katsel ilmneb tõrge. Selle tõrke põhjuseks nimekonfliktide virtuaalse võrgus selle domeeni nimi. Sellisel juhul tuleb kasutada erineva nimega Azure AD domeeniteenused hallatava domeeni häälestamiseks. Vaheldumisi, saate asutusest olemasoleva domeeni ja siis jätkake lubamiseks Azure AD domeeniteenused.

> [AZURE.WARNING] Domeeni teenuste ei saa teisaldada teise virtuaalse võrgu teenus on aktiveeritud.


## <a name="network-security-groups-and-subnet-design"></a>Võrgu turberühmad ja alamvõrgu kujundus
Mõne [Turvalisus jaotises (NSG)](../virtual-network/virtual-networks-nsg.md) sisaldab juurdepääsu juhtimine loend (ACL) reeglid, mis lubada või keelata võrguliikluse virtuaalse võrgu VM eksemplaride loendi. NSGs võib olla seotud alamvõrku või üksikud VM eksemplarid selle alamvõrgu sees. Kui ka NSG on seostatud mõne alamvõrku, ACL reeglite rakendamine VM juhtudel selle alamvõrgu. Lisaks on üksikuid VM-liikluse võib olla piiratud veel mõne NSG otse, et selle VM seostamise kaudu.

![Soovitatav alamvõrgu kujundus](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Valides mõne alamvõrgu head tavad
- Azure'i AD domeeni teenuste juurutamine **eraldi sihtotstarbeline alamvõrgu** oma Azure virtuaalse võrgustikus.

- Rakendada NSGs sihtotstarbeline alamvõrgu hallatava domeeni jaoks. Kui rakendate NSGs sihtotstarbeline alamvõrku, veenduge te **ei vaja teenuse ja hallata oma domeeni blokeerimine**.

- Piira liiga arvu IP-aadresside sihtotstarbeline alamvõrgu hallatava domeeni jaoks saadaval. See piirang takistab teenuse kaks domeenikontrollerid hallatava domeeni jaoks kättesaadavaks tegemine.

- **Luba Azure AD domeeniteenused lüüsi alamvõrgu** virtuaalse võrgu.


> [AZURE.WARNING] Kui seote on NSG koos mõne alamvõrku, kus Azure AD domeeniteenused on lubatud, saate häirida Microsofti võimalus teenuse ja haldamine Domeen. Lisaks teie Azure AD rentniku ja hallatava domeeni sünkroonimiseks katkeb. **SLA ei kehti juurutuste, kus on NSG on rakendatud mis blokeerib Azure AD domeeniteenused värskendamine ja domeeni haldamise kaudu.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Nõutav Azure AD domeeni teenuste pordid
Järgmised pordid on nõutav Azure AD domeeni teenuste teenusega ja säilitada hallatava domeeni. Veenduge, et need pordid on blokeeritud alamvõrgu, kus on lubatud hallatava domeeni jaoks.

| Pordi number | Otstarve |
|---|---|
| 443 | Teie Azure AD rentniku sünkroonimine |
| 3389 | Oma domeeni haldus |
| 5986 | Oma domeeni haldus |
| 636 | Hallatava domeeni turvaline juurdepääs LDAP (LDAPS) |



## <a name="network-connectivity"></a>Võrguühendus
Mõni Azure AD domeeniteenused hallatava domeeni saab lubada üksnes ühe klassikaline virtuaalse võrgu Azure. Azure'i ressursihaldur abil loodud virtuaalne võrkude ei toetata.


### <a name="scenarios-for-connecting-azure-networks"></a>Azure'i võrkude ühendamiseks stsenaariumid
Ühenduse Azure virtuaalse võrgu selle hallatava domeeni mis tahes juurutamise järgmistel juhtudel:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Rohkem kui üks Azure klassikaline virtuaalse võrk hallatava domeeni kasutamine
Saate luua ühenduse muude Azure klassikaline virtuaalse võrkude Azure klassikaline virtuaalse võrgu, kus on lubatud Azure AD domeeniteenused. VPN-ühendus võimaldab kasutada hallatava domeeni koos oma töökoormus juurutatud teistes virtuaalse võrkudes.

![Klassikaline virtuaalse võrguühendus](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Ressursihaldur vastavalt virtuaalse võrgu hallatava domeeni kasutamine
Saate luua ühenduse ressursihaldur vastavalt virtuaalse võrgu Azure klassikaline virtuaalse võrgu, kus on lubatud Azure AD domeeniteenused. See ühendus võimaldab kasutada hallatava domeeni koos oma töökoormus ressursihaldur vastavalt virtuaalse võrgu juurutatud.

![Ressursihaldur klassikaline virtuaalse võrguühenduse abil](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Võrgu ühendussuvandid

- **VNet-VNet ühenduste abil - saidilt VPN-ühendused**: virtuaalse võrgu ühenduse mõne muu virtuaalse võrgu (VNet VNet) on sarnane virtuaalse võrgu ühenduse oma kohapealse saidi asukoha. Mõlemat tüüpi ühenduvuse kasutavad VPN-lüüsi turvaline tunneliga, kasutades IPsec/IKE.

    ![Virtuaalne võrguühendus VPN-lüüsi abil](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Lisateavet - ühenduse, virtuaalse võrgu kaudu VPN-lüüsi](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **Kasutades virtuaalse võrgu silmitsemine VNet-VNet ühendused**: virtuaalse võrgu silmitsemine on süsteem, mis ühendab kaks virtuaalne võrkude piirkonna on Azure võrgu kaudu. Kui piilus, kahe virtuaalse võrgu kuvatakse kõik ühenduvuse eristata. Hallatakse endiselt eraldi ressurssidena, kuid virtuaalmasinates nende virtuaalse võrkudes saate omavahel otse privaatse IP-aadresside abil suhelda.

    ![Virtuaalne võrguühendus silmitsemine abil](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Lisateavet - virtuaalse võrgu silmitsemine](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Seotud teemad

- [Azure virtuaalse võrgu silmitsemine](../virtual-network/virtual-network-peering-overview.md)

- [Klassikaline juurutamise mudeli VNet-VNet ühenduse konfigureerimine](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure'i võrgu turberühmad](../virtual-network/virtual-networks-nsg.md)
