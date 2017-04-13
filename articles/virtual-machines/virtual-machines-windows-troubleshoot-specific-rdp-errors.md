<properties
    pageTitle="Teatud RDP tõrketeated Azure VMs | Microsoft Azure'i"
    description="Mõista kindlaid tõrketeateid, mis võidakse kuvada, kui proovite kasutada kaugtöölaua ühendus virtuaalse masina Windows Azure"
    keywords="Remote'i töölaua viga, kaugtöölaua ühendus viga, ei saa ühendust luua VM Remote'i töölaua tõrkeotsing"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Teatud RDP tõrketeated Windows Azure VM tõrkeotsing
Kaugtöölaua ühendus virtuaalse masina (VM) Windows Azure'i kasutamisel võidakse kuvada vastava tõrketeate. See artikkel üksikasjad osa rohkem levinud tõrketeated, mis esinevad koos nende lahendamiseks tõrkeotsingujuhiseid. Kui teil on probleeme oma VM abil RDP ühenduse, kuid ilmneda teatud tõrketeade, lugege teemat [tõrkeotsing kaugtöölaua juhend](virtual-machines-windows-troubleshoot-rdp-connection.md).

Kindlaid tõrketeateid kohta lisateabe saamiseks vaadake järgmist:

- [Kaugseanss katkestati, kuna puuduvad Remote'i töölaua litsentsi serverid saadaval esitada litsentsi](#rdplicense).
- [Kaugtöölaud ei leia arvuti "nimi"](#rdpname).
- [Autentimise tõrke tõttu. Kohalik turvalisus asutus ei saa ühendust](#rdpauth).
- [Windowsi turvalisus tõrge: oma kasutajanimi ja parool ei tööta](#wincred).
- [See arvuti ei saa ühendust kaugarvuti](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Kaugseanss katkestati, kuna puuduvad Remote'i töölaua litsentsi serverid saadaval esitada litsentsi.

Põhjus: 120-päevane hulgilitsentsimise tutvumisperiood kaugserverisse töölaua rolli on aegunud ja peate installima litsentsid.

Lahendusena portaalist RDP-faili kohaliku koopia salvestamine ja piisavaid PowerShelli käsuviibas, ühenduse. Selles etapis tuleb keelab vaid selle ühenduse litsentsimine.

        mstsc <File name>.RDP /admin

Kui te tegelikult ei vaja rohkem kui kaks samaaegne kaugtöölaua ühendusi VM, saate Server Manager eemaldamiseks kaugserverisse töölaua roll.

Lisateavet leiate ajaveebipostitusest [Azure VM nurjub "Pole Remote'i töölaua litsentsi serverid saadaval"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Kaugtöölaud ei leia arvuti "nimi".

Põhjus: Kaugtöölaua kliendi arvutisse ei saa lahendada arvuti sätted RDP-faili nimi.

Võimalikud lahendused:

- Kui olete ettevõtte sisevõrgus, veenduge, et teie arvuti on juurdepääs puhverserveri ja saate selle saata HTTPS-liikluse.

- Kui kasutate kohalikult talletatav RDP-faili, proovige kasutada ühe, mis on loodud portaali. See toiming tagab, et teil on virtuaalse masina, või selle pilveteenusesse ja lõpp-punkti pordi VM õige DNS-i nimi. Siin on portaali RDP näidisfaili:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

RDP-faili aadressi osa on.
- Täielikult kvalifitseeritud domeeninime pilveteenuses, mis sisaldab VM ("tailspin-azdatatier.cloudapp.net" selles näites).

- Välise TCP pordi lõpp-punkti kaugtöölaua liikluse (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Ilmnes tõrge autentimist. Kohalik turvalisus asutus ei saa ühendust võtta.

Põhjus: Target VM ei leia turvalisus asutuse kasutaja nime osa oma kasutajanimi ja parool.

Kui teie nimi on kujul *SecurityAuthority*\\*kasutajanimi* (näide: CORP\User1), *SecurityAuthority* osa on esitatud VM arvuti nimi (kohalik asutus) või mõne Active Directory domeeni nimi.

Võimalikud lahendused:

- Kui konto on kohalik VM, veenduge, et VM nimi pole õigesti kirjutatud.

- Kui konto on Active Directory domeeni, õigekirja domeeninime.

- Kui see on konto Active Directory domeeni ja domeeni nimi on õigesti kirjutatud, veenduge, et selle domeeni domeenikontrolleri on saadaval. See on levinud probleem Azure virtuaalse võrkudes, mis sisaldavad domeenikontrolleritesse, et domeenikontrolleri pole saadaval, kuna seda pole alustatud. Lahendusena saate kohaliku administraatorikonto asemel domeenikonto.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Windowsi turbe tõrge: oma kasutajanimi ja parool ei tööta.

Põhjus: Target VM ei saa valideerida oma konto nimi ja parool.

Windowsi-põhises arvutis saate kinnitada, kas kohalik konto või domeeni konto identimisteave.

- Kohalikud kontod, kasutage *arvutinimi*\\*UserName* süntaks (näide: SQL1\Admin4798).
- Domeeni kontode puhul kasutada *DomainName*\\*UserName* süntaks (näide: CONTOSO\peterodman).

Kui on esiletõstetud oma uue Active Directory kogumis domeenikontrolleri VM, teisendatakse kohaliku administraatorikonto, et olete sisse loginud võrdväärse kontot, millel on sama parool uus mets ja domeeni. Kohalik konto siis kustutatakse.

Näiteks kui DC1\DCAdmin kohaliku kontoga sisse loginud, ja seejärel edendada virtuaalse masina uus mets corp.contoso.com domeeni domeenikontrolleri, DC1\DCAdmin kohaliku konto kustutamine ja sama parool on loodud uue domeeni kontoga (CORP\DCAdmin).

Veenduge, et konto nimi on nimi, mis virtuaalse masina saate kontrollida, kui kehtiv konto ja parool on õiged.

Kui teil on vaja kohaliku administraatorikonto parooli muuta, vaadake, [Kuidas lähtestada parooli või kaugtöölaua teenus Windows virtuaalmasinates](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>See arvuti ei saa ühendust kaugarvuti.

Põhjus: Konto, mida kasutatakse ühenduse ei ole kaugtöölaua sisselogimise õigused.

Iga Windowsi arvuti on kaugtöölaua kasutajate kohaliku rühma, mis sisaldab kontod ja rühmad, mida saate sisse logida selle kaugühenduse teel. Kohalike administraatorite rühma liikmetel on ka juurdepääs, isegi juhul, kui need kontod on loetletud kohalike kaugtöölaua kasutajate rühma. Domeeni ühendatud seadmed kohalike administraatorite rühma sisaldab domeeni domeeni administraatorid.

Veenduge, et kasutate suhtlemiseks kontol kaugtöölaua sisselogimise õigused. Lahendusena, kasutage domeeni või kohaliku administraatorikonto Kaugtöölaud. Kaugtöölaua kasutajate rühma soovitud konto lisamiseks kasutada Microsofti halduskonsooli lisandmooduli (**Süsteemiriistad > kohalikud kasutajad ja rühmad > Rühmad > kaugtöölaua kasutajate**).


## <a name="next-steps"></a>Järgmised sammud
Kui ükski nende vigade ilmnenud ja teil on tundmatu teema, mille abil RDP ühendamine, lugege teemat [tõrkeotsingujuhendi kaugtöölaua](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure'i IaaS (Windows) diagnostika pakett](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Tõrkeotsingujuhiseid juurdepääs VM töötavad rakendused, vt [tõrkeotsing Accessi rakendus töötab ka Azure VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Kui teil on probleeme ühendamine Linux VM Azure Secure Shell (SSH) abil, lugege teemat [tõrkeotsing SSH ühenduste Linux VM Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).