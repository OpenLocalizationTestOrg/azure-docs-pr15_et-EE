<properties
    pageTitle="Juurdepääs klahvi Vault taga tulemüüri | Microsoft Azure'i"
    description="Saate teada, kuidas pääseda rakenduse tulemüüriga klahvi Vault"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Juurdepääs klahvi Vault tulemüüri taha
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>K: võtme vault klientrakendusega peab olema tulemüüriga, mis pordid/hosts/IP-aadresside peaks võtme vault juurdepääsu lubamiseks avage?

Võtme vault juurdepääsu võtme vault klientrakenduse peab olema juurdepääs mitmele näitajate osas jaoks erinevaid funktsioone.

- Autentimine (Azure Active Directory) kaudu
- Klahv Vault (mis sisaldab loomine ja lugemine/värskendamise ja kustutamise ja ka sätte juurdepääsupoliitikaid) kaudu Azure'i ressursihaldur haldamine
- Juurdepääs ja haldamise võtme vault ise talletatud objektide (võtmed ja saladusi), läheb läbi võtme vault kindla lõpp-punkti (nt [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Olenevalt teie konfiguratsioonist ja keskkonnas on mõned muudatused.   

## <a name="ports"></a>Pordid

Kogu liikluse võtme vault jaoks kõik 3 funktsioonid (autentimine, haldus ja lennuk access) läheb https: pordi 443. Kuid CRL, tekib aeg-ajalt liikluse HTTP (port 80). Kliendid, mis toetavad OCSP ei tohiks jõuda CRL, kuid võib aeg-ajalt [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)saavutamiseks.  

## <a name="authentication"></a>Autentimine

Key Vault klientrakendusega on vaja juurdepääsu autentimiseks Azure Active Directory lõpp-punktid. Lõpp-punkti kasutatakse sõltub AAD rentniku konfigureerimine ja tüüp põhisumma--kasutaja põhisumma, teenuse põhilise ja tüüpi kontot, nt Microsofti Account või ettevõtte ID-ga.  

| Põhisumma tüüp | Lõpp-punkt: port |
|----------------|---------------|
| Microsofti Account Kasutaja<br> (nt)user@hotmail.com) | **Globaalne:**<br> login.microsoftonline.com:443<br><br> **Azure'i Hiina:**<br> login.chinacloudapi.CN:443<br><br>**Azure'i USA valitsuse:**<br> Logi sisse – us.microsoftonline.com:443<br><br>**Azure'i Saksamaa:**<br> login.microsoftonline.de:443<br><br> ja <br>login.live.com:443   |
| Ettevõtte ID abil AAD (nt kasutaja/teenuse põhisummauser@contoso.com) | **Globaalne:**<br> login.microsoftonline.com:443<br><br> **Azure'i Hiina:**<br> login.chinacloudapi.CN:443<br><br>**Azure'i USA valitsuse:**<br> Logi sisse – us.microsoftonline.com:443<br><br>**Azure'i Saksamaa:**<br> login.microsoftonline.de:443 |
| Kasutaja/teenuse põhilise (nt ettevõtte ID + ADFS-i või muu ühendatud lõpp-punkti abiluser@contoso.com) | Kõik ülaltoodud lõpp-punktide ettevõtte ID pluss ADFS-i või muu ühendatud lõpp-punktid |

On võimalik keerukas stsenaariumide. Lisateabe saamiseks vaadake [Azure Active Directory autentimine Flow](/documentation/articles/active-directory-authentication-scenarios/), [Rakenduste integreerimine Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) ja [Active Directory autentimise Protokollid](https://msdn.microsoft.com/library/azure/dn151124.aspx) .  

## <a name="key-vault-management"></a>Võtme Vault haldus

Klahv Vault haldamine (CRUD ja säte juurdepääsu poliitika), peab võtme vault klientrakendusega Azure'i ressursihaldur lõpp-punkti juurdepääs.  

| Toimingu tüüp | Lõpp-punkt: port |
|----------------|---------------|
| Klahv Vault juhtelemendi lennuk toimingud<br> Azure'i ressursihaldur kaudu | **Globaalne:**<br> Management.Azure.com:443<br><br> **Azure'i Hiina:**<br> Management.chinacloudapi.CN:443<br><br> **Azure'i USA valitsuse:**<br> Management.usgovcloudapi.net:443<br><br> **Azure'i Saksamaa:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph API | **Globaalne:**<br> Graph.Windows.net:443<br><br> **Azure'i Hiina:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure'i USA valitsuse:**<br> Graph.Windows.net:443<br><br> **Azure'i Saksamaa:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Võtme Vault toimingud

Võtme vault objekti (võtmed ja saladusi) haldus ja cryptographic toimingud, võtme vault kliendi peab võtme vault lõpp-punkti juurdepääs. Sõltuvalt teie võti Vault asukohast, erineb lõpp-punkti DNS-i järelliite. Vormingu on võti Vault lõpp-punkti: < vault nimi >. < piirkonna-kindla-dns-sufiks >, nagu on kirjeldatud järgmises tabelis.  

| Toimingu tüüp | Lõpp-punkt: port |
|----------------|---------------|
| Klahv Vault toimingute cryptographic toimingute kiirklahvid, loodud, lugemine, värskendamine ja kustutamine võtmed ja saladusi, nt määramine või tuua sildid ja muud atribuudid võtme vault objektid (klahvid/saladused)     | **Globaalne:**<br> &lt;Vault-nime&gt;. vault.azure.net:443<br><br> **Azure'i Hiina:**<br> &lt;Vault-nime&gt;. vault.azure.cn:443<br><br> **Azure'i USA valitsuse:**<br> &lt;Vault-nime&gt;. vault.usgovcloudapi.net:443<br><br> **Azure'i Saksamaa:**<br> &lt;Vault-nime&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-aadresside vahemikud

Klahv Vault teenuse omakorda kasutab Azure muud ressursid, nt PaaS taristu, seega ei ole võimalik teatud vahemikus, IP-aadressid, võtme vault teenuse lõpp-punktid on mis tahes ajal esitada. Juhul kui teie tulemüür toetab ainult vahemike seejärel vaadake [Microsoft Azure'i andmekeskuse IP-aadresside vahemikud](https://www.microsoft.com/download/details.aspx?id=41653) dokumendi IP-aadress.   Autentimiseks ja identiteedi (Azure Active Directory), tuleb rakenduse lõpp-punktid kirjeldatud [autentimis- ja identiteedi aadressid](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)ühendust luua.

## <a name="next-steps"></a>Järgmised sammud

- Kui teil on küsimusi klahvi Vault, külastage [Azure'i Key Vault Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
