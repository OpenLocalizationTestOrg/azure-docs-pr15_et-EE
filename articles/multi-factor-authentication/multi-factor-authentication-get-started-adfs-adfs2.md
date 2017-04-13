<properties
    pageTitle="Azure'i MFA serveri kasutamine AD FS 2.0 | Microsoft Azure'i"
    description="See on Azure mitmekordne autentimine leht, mis kirjeldab, kuidas alustada Azure'i MFA ja AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Turvaline pilve ja asutusesisese ressursse AD FS 2.0 Azure'i mitme teguriga autentimine serveri kasutamine

Selles artiklis on ettevõtted, mis on ühendatud Azure Active Directory ja soovite secure ressursse, mis on asutusesisese või pilveteenuses. Kaitsta oma ressursse, kasutades Azure mitmekordne autentimise Server ja konfigureerida selle töötama teenusekomplektiga AD FS-i, et kaheastmelise kontrollimise käivitatakse kõrge väärtusega lõpp-punktid.

Dokumentide hõlmab Server Azure'i mitme teguri autentimist kasutades AD FS 2.0.  Lisateave [turvamine pilveteenuste ja kohapealsete](multi-factor-authentication-get-started-adfs-w2k12.md)ressursid Azure'i mitmekordne autentimine serveris Windows Server 2012 R2 AD FS -i abil.


## <a name="secure-ad-fs-20-with-a-proxy"></a>Turvaline AD FS 2.0 proxy
AD FS 2.0 turvamiseks proxy installida Azure mitmekordne autentimise Server ADFS-i puhverserveri ja konfigureerida.

### <a name="configure-iis-authentication"></a>IIS-i autentimise konfigureerimine

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni **IIS-i autentimist** .
2. Klõpsake vahekaarti **Vormipõhiseid** .
3. Klõpsake soovitud **Lisa...** nupp.
<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Kasutajanimi, parool ja domeeni muutujate automaatselt tuvastada, sisestage dialoogiboksis Auto-Configure Form-Based veebisaidi (nt https://sso.contoso.com/adfs/ls) sisselogimise URL ja klõpsake nuppu OK.
5. Märkige ruut **nõua Azure'i Mitmikautentimise kasutaja vastavad** , kui kõik kasutajad on või server ja kaheastmelise kontrollimise importida. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine kaheastmelise kontrollimise, jätke ruut märkimata. Selle funktsiooni kohta lisateabe saamiseks vt spikrifaili.
6. Kui lehe muutujate ei leita automaatselt, klõpsake selle **Määrata käsitsi...** dialoogiboksi Auto-Configure Form-Based veebisaidi nupp.
7. Dialoogiboksis Add Form-Based veebisaidil Sisestage väljale esitage URL (nt https://sso.contoso.com/adfs/ls) ADFS-i sisselogimislehe URL ja sisestage rakenduse nimi (valikuline). Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid. Vt lisateavet spikrifaili esitada URL-i.
8. Määrake taotluse vorming "POSTITADA, või tooge."
9. Sisestage kasutajanimi muutuja (ctl00$ ContentPlaceHolder1$ UsernameTextBox) ja parool muutuja (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Kui vormi sisselogimislehe kuvab domeeni tekstivälja, sisestage domeeni muutuja ka. Kui peate liikuge sisselogimislehe veebibrauseris, paremklõpsake lehel ja valige **Kuva allikas** väljanimede sisend lahtrid sisselogimise leht.
10. Märkige ruut **nõua Azure'i Mitmikautentimise kasutaja vastavad** , kui kõik kasutajad on või server ja kaheastmelise kontrollimise importida. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine kaheastmelise kontrollimise, jätke ruut märkimata.
<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klõpsake nuppu **Täpsem...** nupp Täpsemad sätted üle vaadata. Saate konfigureerida sätted, sh võimalus kohandatud eitamine lehe faili, valige vahemälu eduka isikutuvastusi veebisaidile, kasutades küpsiseid ja valige, kuidas autentimiseks esmane mandaadi.
12. Kuna ADFS-i puhverserver tõenäoliselt pole domeeniga liidetud, saate LDAP domeenikontrolleri eelse autentimist ja kasutajale impordi ühendamiseks. Dialoogiboksi Advanced Form-Based veebisaidi **Esmane autentimine** vahekaarti ja valige **LDAP siduda** puhul eelse autentimise autentimistüüp.
13. Kui olete lõpetanud, klõpsake nuppu **OK** , et naasta dialoogiboksi Add Form-Based veebisaidi. Vt lisateavet spikrifaili Täpsemad sätted.
14. Dialoogiboksi sulgemiseks nuppu **OK** .
15. Kui URL ja lehe muutujate tuvastatud või sisestatud veebisaidi andmed kuvatakse paanil vormipõhiseid.
16. **Kohalikke mooduli** vahekaarti ja valige server, veebisaidi, kus töötab ADFS-i puhverserveri jaotises (nt "Vaikeveebisait") või rakenduse ADFS-i puhverserveri (nagu "ls" jaotises "i") on IIS-i lisandmooduli soovitud tasemel.
17. Klõpsake ekraani ülaosas väljal **Luba IIS-i autentimist** .
18. Nüüd on lubatud IIS-i autentimine.

### <a name="configure-directory-integration"></a>Kataloogi integreerimise konfigureerimine

Märkisite IIS-i autentimine, kuid eelse autentida, et teie Active Directory (AD) kaudu LDAP tuleb konfigureerida selle domeenikontrolleri LDAP ühendus.

1. **Kataloogi integreerimise** ikooni.
2. Klõpsake vahekaardil sätted, valige **teatud LDAP konfigureerimine** .
<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Valige soovitud **allalaadimissuvand** nupp.
4. Dialoogiboksis Redigeeri LDAP konfiguratsiooni väljad asustada AD domeenikontrolleri loomiseks vajalik teave. Väljade kirjelduste sisalduvad järgmises tabelis. See teave on ka Azure mitmekordne autentimine serveri spikrifaili.
5. LDAP ühenduse testimiseks klõpsake nuppu **testi** .
<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Kui LDAP ühenduse test õnnestus, klõpsake nuppu **OK** .

### <a name="configure-company-settings"></a>Ettevõtte sätete konfigureerimine

1. Järgmiseks klõpsake ikooni **Ettevõtte sätted** ja valige vahekaart **Kasutajanimi eraldusvõime** .
2. Klõpsake raadionuppu **kattuvad kasutajanimede kasutamine Lightweight kordumatut tunnust atribuut** .
3. Kui kasutaja sisestab oma kasutajanimi vormingus "domeeni", peab serveri saama ribad välja kasutajanimi domeeni loob LDAP päringu. Mida saab teha registri sätte kaudu.
4. Avage registriredaktor ja seejärel HKEY_LOCAL_MACHINE, tarkvara, Wow6432Node ja positiivne võrkude/PhoneFactor 64-bitine serveris. Kui serveris 32-bitine võtta "Wow6432Node" tee välja. Luua nimega "UsernameCxz_stripPrefixDomain" registrivõtme DWORD ja määrake väärtuseks 1. Azure'i Mitmikautentimise on nüüd turvaliseks ADFS-i puhverserver.

Veenduge, et kasutajad imporditud Active Directory Server. Kui soovite valge sisemise IP-aadressid, et kaheastmelise kontrollimise ei ole vajalik, kui need asukohtadest veebisaidile sisse logida, leiate [jaotise usaldusväärsed IP -d](#trusted-ips) .

<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 otse ilma proxy

Saate tagada AD FS-i, kui ei kasutata AD FS puhverserver. AD FS server Azure'i mitmekordne autentimise Server installida ja konfigureerida kohta järgmist:

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni **IIS-i autentimist** .
2. Klõpsake vahekaarti **HTTP** .
3. Klõpsake soovitud **Lisa...** nupp.
4. Sisestage dialoogiboksis lisamine URL-i ADFS-i veebisaidil, kui HTTP autentimine toimub (nt https://sso.domain.com/adfs/ls/auth/integrated) väljale URL-i URL. Sisestage rakenduse nimi (valikuline). Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid.
5. Soovi korral korrigeerida jõudeaja ajalõpu ja maksimum seansi korda.
6. Märkige ruut **nõua Azure'i Mitmikautentimise kasutaja vastavad** , kui kõik kasutajad on või server ja kaheastmelise kontrollimise importida. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine kaheastmelise kontrollimise, jätke ruut märkimata. Selle funktsiooni kohta lisateabe saamiseks vt spikrifaili.
7. Kui soovitud, märkige ruut küpsise vahemälu.
<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klõpsake nuppu **OK** .
9. Klõpsake vahekaarti **Kohalikke mooduli** ja valige server, veebisaidi (nt "Vaikeveebisait") või ADFS-i rakendus (nt "ls" jaotises "i") on IIS-i lisandmooduli soovitud tasemel.
10. Klõpsake ekraani ülaosas väljal **Luba IIS-i autentimine** . Azure'i Mitmikautentimise on nüüd turvaliseks ADFS-i.

Veenduge, et kasutajad imporditud Active Directory Server. Vaadake jaotist usaldusväärsed IP-d, kui soovite valge sisemise IP-aadressid, et kaheastmelise kontrollimise ei ole vajalik, kui need asukohtadest veebisaidile sisse logida.


## <a name="trusted-ips"></a>Usaldusväärsete IP-d
Usaldusväärsete IP-d lubada kasutajatel mööduda Azure'i Mitmikautentimise veebisaidi päringuid, mis on pärit teatud IP-aadresside või alamvõrku. Näiteks võite välistatud kasutajate kaheastmelise kontrollimise, kui neid sisselogimisel Office. Selleks määrate office alamvõrgu usaldusväärsed IP-d kirje nimega.

### <a name="to-configure-trusted-ips"></a>Kui soovite konfigureerida usaldusväärsed IP-d


1. IIS-i autentimist jaotise vahekaarti **Usaldusväärsed IP -d** .
1. Klõpsake soovitud **Lisa...** nupp.
1. Kui kuvatakse dialoogiboks lisamine usaldusväärsete IP-d, valige üks raadionuppude **Ühe IP**, **IP-vahemiku**või **alamvõrgu** .
1. Sisestage IP-aadressi, vahemiku IP-aadresside või alamvõrku, mis peaks olema whitelisted. Kui soovitud alamvõrku, valige sobiv Võrgumask ja klõpsake nuppu **OK** . Usaldusväärsete IP on nüüd lisatud.


<center>![Häälestamine](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
