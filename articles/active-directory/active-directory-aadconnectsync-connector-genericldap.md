<properties
   pageTitle="Üldise LDAP konnektor | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas konfigureerida Microsofti üldise LDAP konnektor."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-ldap-connector-technical-reference"></a>Üldise LDAP konnektor tehniline teave
Selles artiklis kirjeldatakse üldise LDAP konnektor. See artikkel kehtib järgmiste toodete:

- Microsofti Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Peate kasutama kiirparandus 4.1.3671.0täiendama või uuem versioon [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2, konnektor on saadaval [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=717495)alla laadida.

IETF teadusfondi viitamisel selles dokumendis on vormingus (RFC [RFC number] ja [jaotise RFC dokumendis]), näiteks (RFC 4512/4.3).
Lisateavet leiate veebisaidil http://tools.ietf.org/html/rfc4500 (peate asendamiseks 4500 õige RFC number).

## <a name="overview-of-the-generic-ldap-connector"></a>Üldise LDAP konnektor ülevaade
Üldise LDAP Connector võimaldab teil sünkroonimise teenuse integreerida LDAP v3 server.

Teatud toimingute ja skeemi elemente, nagu need on vaja teha delta importida, ei ole määratletud IETF teadusfondi. Nendeks toiminguteks toetatakse ainult LDAP kataloogid järel konkreetselt nimetatud.

Praeguse versiooni konnektor toetavad kõrgetasemelisi seisukohalt järgmised funktsioonid:

Funktsioon | Tugi
--- | --- |
Ühendatud andmeallikas | Konnektor on toetatud kõik LDAP v3 serverid (RFC 4510 nõuetele). See on testitud järgmist: <li>Microsoft Active Directory kataloogisirvimise Services (AD LDS)</li><li>Microsoft Active Directory (AD c) globaalse kataloogi</li><li>389 directory Server</li><li>Apache Directory Server</li><li>IBM Tivoli DS</li><li>Isode kataloog</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Avatud DJ</li><li>Avatud DS</li><li>Avatud LDAP (openldap.org)</li><li>Oracle'i (varem p) Directory Server Enterprise Edition</li><li>RadiantOne Virtual Directory Server (VDS)</li><li>P ühe Directory Server</li>**Kuid kataloogid ei toetata:** <li>Microsoft Active Directory Domain Services (AD DS) [selle asemel kasutada sisseehitatud Active Directory konnektor]</li><li>Oracle'i Interneti-kataloog (objekti ID)</li>
Stsenaariumid   | <li>Objekti elutsükli haldus</li><li>Rühma haldamine</li><li>Parooli haldus</li>
Toimingud |Kõik LDAP kataloogid on toetatud järgmised toimingud: <li>Täielik importimine</li><li>Eksportimine</li>Järgmised toimingud on toetatud ainult määratud kataloogide:<li>Delta importimine</li><li>Määra parool, parooli muutmine</li>
Skeemi | <li>Skeemi tuvastatakse LDAP skeemist (RFC3673 ja RFC4512/4.2)</li><li>Toetab strukturaalset tunnid, aux tunnid ja extensibleObject objekti klassi (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Delta impordi- ja parooli dokumendihalduse toe
Toetatud kataloogide Delta impordi- ja parooli haldus:

- Microsoft Active Directory kataloogisirvimise Services (AD LDS)
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab parooliga määramine
- Microsoft Active Directory (AD c) globaalse kataloogi
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab parooliga määramine
- 389 directory Server
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab seatud parool ja parooli muutmine
- Apache Directory Server
    - Ei toeta delta impordi, kuna see kaust ei ole püsiv muutmine Logi
    - Toetab parooliga määramine
- IBM Tivoli DS
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab Määra parool ja parooli muutmine
- Isode kataloog
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab Määra parool ja parooli muutmine
- Novell eDirectory ja NetIQ eDirectory
    - Toetab delta impordi toimingute lisamine, värskendamine ja ümbernimetamine
    - Ei toeta kustutada toimingute jaoks delta importimine
    - Toetab Määra parool ja parooli muutmine
- Avatud DJ
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab seatud parool ja parooli muutmine
- Avatud DS
    - Toetab delta importida kõik toimingud
    - Toetab Määra parool ja parooli muutmine
- Avatud LDAP (openldap.org)
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab parooliga määramine
    - Ei toeta parooli muutmine
- Oracle'i (varem p) Directory Server Enterprise Edition
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab Määra parool ja parooli muutmine
- RadiantOne Virtual Directory Server (VDS)
    - Peate kasutama versiooni 7.1.1 või uuem versioon
    - Toetab delta importida kõik toimingud
    - Toetab Määra parool ja parooli muutmine
-  P ühe Directory Server
    - Toetab kõiki toiminguid delta importimiseks
    - Toetab Määra parool ja parooli muutmine

### <a name="prerequisites"></a>Eeltingimused
Enne konnektor kasutamiseks veenduge, et teil on sünkroonimise server järgmist.

- Microsoft .NET 4.5.2 Frameworki või uuem versioon

### <a name="detecting-the-ldap-server"></a>LDAP server tuvastamine
Konnektor tugineb mitmesuguste tehnikate leida ja tuvastada LDAP server. Konnektor kasutab Root kuvariga töötamise riskide, tarnija nimi/versioon ja selle uurib skeemiga leidmiseks kordumatu objektid ja atribuudid teada, et teatud LDAP serverites. Andmed, kui leitud, mida kasutatakse asustamiseks eelnevalt konnektor valikuid.

### <a name="connected-data-source-permissions"></a>Ühendatud andmeallika õigused
Tehke import ja eksporditoimingu ühendatud kataloogi objektid, Connectori konto peab olema piisavaid õigusi. Konnektor peab kirjutusõiguste saama eksportida ja importida lugemisõigused. Õiguste konfigureerimine toimub halduse kogemusi siht kataloogi ise sees.

### <a name="ports-and-protocols"></a>Pordid ja protokollid
Konnektor kasutab konfiguratsiooni, mis vaikesättena on 389 LDAP ja 636 LDAPS jaoks määratud pordinumber.

LDAPS, peate kasutama SSL 3.0 või TLS. SSL-i 2.0 ei toetata ja ei saa aktiveerida.

### <a name="required-controls-and-features"></a>Nõutavad juhtelemendid ja funktsioonid
Järgmised LDAP juhtelementide/funktsioonid peavad olema LDAP server töötaks õigesti konnektor:  
`1.3.6.1.4.1.4203.1.5.3`Tõene/Väär filtrid

Tõene/Väär filter on sageli teatada ei toeta LDAP kataloogid ja võidakse kuvada **Globaalne lehel** jaotises **Kohustuslik funktsioonid ei leitud**. Seda kasutatakse LDAP päringute, nt mitme objekti tüübid importimisel **või** filtrite loomise kohta. Kui saate importida mitu objekti tüüp, LDAP teie server toetab seda funktsiooni.

Kui kasutate kataloog, kus on kordumatu ankur järgmist ka tuleb (vt lisateavet selle artikli jaotises [Konfigureerimine ankrud](#configure-anchors) ):  
`1.3.6.1.4.1.4203.1.5.1`Kõik funktsionaalseid atribuudid

Kui kataloogi on veel objekte, mis ei sobi ühe kõne kataloogi kui, siis on soovitatav kasutada Piipar. Piipar töötamiseks, tuleb teil üks järgmistest suvanditest:

**Variant 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Variant 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Kui mõlema variandi on lubatud konnektori konfiguratsiooni, kasutatakse pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl kasutatakse ainult USNChanged delta impordi meetod vaadata kustutatud objektide.

Konnektor proovib tuvasta server Esita suvandid. Kui suvandid ei saa tuvastada, hoiatus on globaalne lehel konnektor atribuudid. Toetavad kõiki LDAP serverite Esita kõik juhtelemendid/funktsioone ja isegi juhul, kui hoiatus on olemas, konnektor ei pruugi töötada probleemideta.

### <a name="delta-import"></a>Delta importimine
Delta impordi on saadaval ainult siis, kui on tuvastatud tugi kataloogi. Praegu kasutatakse järgmist:

- LDAP Accesslog. Vt [http://www.openldap.org/doc/admin24/overlays.html#Access logimine](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP muutus. Vt [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Ajatempli. Novell/NetIQ eDirectory, konnektor kasutab viimase kuupäeva/kellaaja saada ja uuendamise objektid. Novell/NetIQ eDirectory ei paku samaväärne tähendab, et tuua kustutatud objektide. Selle suvandi saate kasutada ka ilma muude delta impordi meetod on aktiivne LDAP server. See suvand pole võimalik objektide importimine kustutatud.
- USNChanged. Vaadake teemat: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Pole toetatud
LDAP järgmised funktsioonid pole toetatud:

- LDAP viited serveri (RFC 4511/4.1.10) vahel

## <a name="create-a-new-connector"></a>Looge uus konnektor
Üldise LDAP konnektor loomiseks **Sünkroonimise teenuse** valige **Haldamise Agent** ja **loomine**. Valige konnektor **üldise LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Ühenduvus
Ühenduvuse lehel Määrake Host, portide ja sidumine teave. Sõltuvalt sidumine on valitud, täiendav teave võib esitada järgmistes jaotistes.

![Ühenduvus](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Ühenduse ajalõpp sätet kasutatakse ainult esimese serveriga ühenduse tuvastamiseks skeemiga.
- Kui sidumine on Anonüümne, siis ei kasutajanime ja parooli ega serdi kasutatakse.
- Muud sidumiste, sisestage teave kummagi tekstivormingus kasutajanime ja parooli või valige sert.
- Kui kasutate Kerberose autentida, siis ka pakkuda kasutaja domeen ja domeeni.

Tekstivälja **atribuudis pseudonüümid** kasutatakse atribuutide määratletud skeemi RFC4522 süntaks. Neid atribuute ei saa tuvastada skeemi tuvastamise ajal ja konnektor peab olema aitab tuvastada neid atribuute. Näiteks on õigesti tuvastada kahendarvu atribuut nimega atribuut userCertificate väljale atribuudi pseudonüümid kantavad vaja järgmist:

`userCertificate;binary`

Järgmine on näide, kuidas see konfiguratsioon võib välja näha:

![Ühenduvus](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Märkige ruut **kaasa funktsionaalseid atribuute skeemi** ka kaasata atribuute, mis on loodud server. Nendeks atribuute, näiteks kui objekt on loodud ja kellaaeg viimati muudetud.

Valige **kaasa laiendatav skeemi atribuudid** , kui kasutatakse laiendatav objektid (RFC4512/4.3) ja selle lubamine võimaldab kõik objekti kasutatakse iga atribuudi. Selle suvandi valimisel teeb skeemiga väga suurte nii, et juhul, kui ühendatud kataloogi kasutab seda funktsiooni soovitus on märkeruut tühjaks jätta.

### <a name="global-parameters"></a>Globaalne parameetrid
Lehel globaalne parameetrid saate konfigureerida DN delta muutmine Logi ja LDAP lisavõimalusi. Leht on juba eelnevalt täidetud LDAP serveri teave.

![Ühenduvus](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Ülaosas kuvatakse teave, mida server ise, näiteks serveri nimi. Konnektor ka kinnitab, et kohustuslik juhtelementide viibivad Root kuvariga töötamise riskide. Kui neid juhtelemente ei ole loetletud, on esitatud hoiatus. Mõned LDAP kataloogid ei ole loetletud kõik funktsioonid Root kuvariga töötamise riskide ja on võimalik, et konnektor töötab probleemideta isegi juhul, kui esineb hoiatus.

**Toetatud juhtelementide** ruute juhtimiseks käitumist teatud toimingute jaoks tehke järgmist.

- Hierarhia kustutatakse koos valitud puu Kustuta ühe LDAP kõne. Puu Kustuta valimata, tähendab see konnektor Rekursiivsed Kustuta vajaduse korral.
- Valitud leheküljed tulemustega konnektor ei leheküljed impordi suurus määratud Käivita juhiseid.
- VLVControl ja SortControl on alternatiiv pagedResultsControl Lightweight Directory andmeid lugeda.
- Kui kõik kolm võimalust (pagedResultsControl, VLVControl ja SortControl) on valimata konnektor impordib kõik objekti ühe toiminguga, mis võib nurjuda, kui see on suur kataloog.
- ShowDeletedControl kasutatakse ainult siis, kui Delta impordi on USNChanged.

Muutmine Logi DN on kasutatud delta muutmine Logi, näiteks nimetamise konteksti **cn = muutus**. See väärtus peab olema määratud saaks teha delta impordi.

Järgmises loendis vaikimisi muutmine Logi DNS-i:

Kataloog | Delta muutmine Logi
--- | ---
Microsoft reklaami LDS ja AD c | Automaatselt avastada. USNChanged.
Apache Directory Server | Pole saadaval.
Directory 389 | Muutmine Logi. Vaikimisi väärtuse: **cn = muutus**
IBM Tivoli DS | Muutmine Logi. Vaikimisi väärtuse: **cn = muutus**
Isode kataloog | Muutmine Logi. Vaikimisi väärtuse: **cn = muutus**
Novell/NetIQ eDirectory | Pole saadaval. Ajatempli. Viimati värskendatud Konnektori kasutab kuupäev/kellaaeg, et leida lisada ja värskendada kirjeid.
Avatud DJ/DS | Muutmine Logi.  Vaikimisi väärtuse: **cn = muutus**
Avatud LDAP | Access log. Vaikimisi väärtuse: **cn = accesslog**
Oracle'i DSEE | Muutmine Logi. Vaikimisi väärtuse: **cn = muutus**
RadiantOne VDS | Virtuaalne kaust. Sõltub VDS ühendatud kataloogi.
P ühe Directory Server | Muutmine Logi. Vaikimisi väärtuse: **cn = muutus**

Atribuut password on konnektor tuleks kasutada parooli seada parooli muutmine toodud atribuudi nimi ja parool seada toimingud.
See väärtus on vaikimisi määratud **tekst kasutajaparool** , kuid saate muuta, kui vaja kindla LDAP süsteemi.

Täiendavad sektsioonid loendis on võimalik lisada täiendavad nimeruumid automaatselt ei tuvastata. Näiteks saab seda sätet, kui mitu serverid moodustavad loogilise kobar, mis tuleks kõik importida samal ajal. Nagu Active Directory saate ühe mets on mitu domeeni, kuid kõigi domeenide jagada ühe skeemi, saate sama modelleerida täiendavad nimeruumid sisestades selle väljale. Iga nimeruum saate importida serveritest ja on konfigureeritud täpsemaks lehel konfigureerimine sektsioonid ja hierarhiad. Kasutage klahvikombinatsiooni Ctrl + Enter, et leida uus rida.

### <a name="configure-provisioning-hierarchy"></a>Ettevalmistamise hierarhia konfigureerimine
Selle lehe saab vastendada objekti tüüp, mis peaks olema ette valmistatud, näiteks organizationalUnit DN component, näiteks OU.

![Ettevalmistamise hierarhia](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Ettevalmistamise hierarhia konfigureerides saate konfigureerida konnektor automaatselt luua struktuuri vastavalt vajadusele. Näiteks kui on nimeruum AV = contoso, näiteks põhiliselt com ja uue objekti cn = = Joe ou = Seattle, c = US, AV = contoso, AV = com on ette valmistatud ja seejärel konnektor saate luua objekti tüüp riigi USA ja mõne organizationalUnit Seattle jaoks, kui need pole juba kataloogis.

### <a name="configure-partitions-and-hierarchies"></a>Sektsioonid ja hierarhiad konfigureerimine
Valige lehel sektsioonid ja hierarhiad kõik nimeruumid kavatsete importimine ja eksportimine objektidega.

![Sektsioonid](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Iga nimeruumi, on võimalik ekraanil ühenduvuse määratletud väärtused eirata ühenduvuse sätete konfigureerimine. Kui need väärtused on vaikimisi tühi väärtus, kasutab seda teavet kuval Ühenduvus.

Samuti on võimalik valida, milliseid ümbriste ja organisatsiooniüksused konnektor peaks importimine ja eksportimine.

### <a name="configure-anchors"></a>Ankrud konfigureerimine
See leht on alati eelkonfigureeritud väärtus ja ei saa muuta. Kui server müüja on tuvastatud, siis ankur võib täidetud püsiv atribuudiga, näiteks GUID objekti. Kui see on avastatud või on teada, et ei tohi olla püsiv atribuut, siis konnektor kasutab dn (eristatav nimi) ankur.

![Ankrud](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Järgmises loendis on kasutatud ankrutüübist ja LDAP serverid:

Kataloog | Ankur atribuut
--- | ---
Microsoft reklaami LDS ja AD c | FederatedIdentity
389 directory Server | DN
Apache Directory | DN
IBM Tivoli DS | DN
Isode kataloog | DN
Novell/NetIQ eDirectory | GUID
Avatud DJ/DS | DN
Avatud LDAP | DN
Oracle'i ODSEE | DN
RadiantOne VDS | DN
P ühe Directory Server | DN

## <a name="other-notes"></a>Muud märkmed
Sellest jaotisest leiate teavet aspekte, mis on omased selle konnektori või mõnel muul põhjusel on oluline teada.

### <a name="delta-import"></a>Delta importimine
Klõpsake avatud LDAP delta vesimärgi on UTC kuupäev ja kellaaeg. Seetõttu tuleb kella FIM sünkroonimise teenuse ja avatud LDAP vahel sünkroonida. Kui ei, siis võib mõni kirje delta muutmine Logi välja jäetud.

Jaoks Novell eDirectory, ei tuvasta delta impordi mis tahes objekti kustutab. Seetõttu tuleb kõik kustutatud objektide leidmiseks perioodiliselt täielik impordi käivitamine.

Kataloogid delta muutmine Logi, mis põhineb kuupäev/kellaaeg, see on väga soovitatav perioodilised. alati täielik impordi käivitamine. Selle protsessi võimaldab sünkroonimine engine otsimiseks ja ärisegmenti LDAP server ja mis on praegu konnektori ruumis vahel.

## <a name="troubleshooting"></a>Tõrkeotsing

-   Konnektor tõrkeotsingu logimine lubamise kohta leiate artiklist [Kuidas lubada ETW jälitamine konnektorid](http://go.microsoft.com/fwlink/?LinkId=335731).
