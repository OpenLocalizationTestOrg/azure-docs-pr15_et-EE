<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: tehniline põhimõtet | Microsoft Azure'i"
    description="Azure'i AD-ühenduse sünkroonimine selgitatakse tehniline."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure'i AD-ühendus sünkroonimine: tehnilised terminid
Selles artiklis on teema [mõistmine arhitektuur](active-directory-aadconnectsync-technical-concepts.md)kokkuvõte.

Azure'i AD-ühendus sünkroonimine ehitatakse üles ühtlase metadirectory sünkroonimise platvormi.
Järgmistes jaotistes tutvustada metadirectory sünkroonimiseks mõisteid.
Azure Active Directory sünkroonimise Services tuginedes MIIS, ILM ja FIM, pakub järgmise platvormi andmeallikatega ühenduse loomine, andmeallikad, kui ka selle ettevalmistamise vaheline andmete sünkroonimine ja identiteetide deprovisioning.

![Tehnilised terminid](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Järgmistest jaotistest leiate täpsemat teavet järgmiste aspektide FIM sünkroonimise teenuse:

- Konnektor
- Atribuudi voog
- Konnektori ruumi
- Metaversumi
- Ettevalmistamise

## <a name="connector"></a>Konnektor

Koodi moodulid, mida kasutatakse suhtlemiseks ühendatud kataloogi nimetatakse konnektorid (varem halduse agentide (MAs)).

Need on installitud arvuti sünkroonimine Azure'i AD-ühenduse.
Konnektorid pakuvad agentless võimalus vestelda selle asemel eriotstarbelisi agentide juurutamise kaugsüsteem Protokollid abil. See tähendab, et vähenemine ohud ja juurutamise korda, eriti siis, kui tegemist on kriitiliste rakenduste ja süsteemid.

Ülaltoodud joonisel on sünonüüm konnektori ruumi konnektor, kuid hõlmab kõiki välise süsteemiga.

Konnektor vastutab kõigi importimine ja eksportimine süsteemi funktsioonid ja vabastab arendajate, kellel on vaja mõista, kuidas iga süsteemidega ühendamiseks algupäraselt kasutamisel deklaratiivseid ettevalmistamise kohandamiseks andmeid teisendused.

Impordi ja ekspordi ainult ilmneda planeerida lubamisel edasise isolatsioon muutusi süsteemis, kuna muudatuste levitamine automaatselt ühendatud andmeallika põhjal. Lisaks võidakse luua arendajate oma konnektorid praktiliselt kõigist andmeallikaga.

## <a name="attribute-flow"></a>Atribuudi voog

Metaversumi on konsolideeritud kõigi ühendatud identiteeti naabruses konnektor tühikuid. Ülaltoodud joonisel on kujutatud atribuudi voog read, mille Noolepeade nii sissetulevate ja väljaminevate meilivoo jaoks. Kõik atribuut liikumine (sissetulevaid või väljaminevaid) atribuudi voog on kopeerimine või mis võimaldab transformeerida andmeid ühest süsteemist teise.

Atribuudi voog toimub konnektori ruumi ja metaversumi vahel kahesuunaliselt sünkroonimistoimingud (täielik või delta) on ajastatud.

Atribuudi voog ilmneb ainult need sünkroonimine käitamisel. Atribuut puhul on määratletud sünkroonimise reeglid. Need võivad olla Sissetulev (ISR ülaltoodud joonisel) või väljaminev (OSR ülaltoodud joonisel).

## <a name="connected-system"></a>Ühendatud süsteemi

Ühendatud süsteemi (ehk ühendatud kataloogi) viidates kaugsüsteem Azure'i AD-ühenduse sünkroonimine on ühendatud ja lugemine ja kirjutamine identiteedi andmed ja sealt.

## <a name="connector-space"></a>Konnektori ruumi

Iga ühendatud andmeallika on esitatud arvuna filtreeritud alamkogumit, objektide ja atribuutide konnektori ruumis.
See võimaldab sünkroonimise teenuse Sky kohalikult vajamata pöörduda kaugsüsteem objektide sünkroonimisel ja piirab suhtlust impordi ja ekspordi ainult.

Kui andmeallika ja konnektor on võimalus loetleda muudatused (delta importida), siis tulemuslikkuse suureneb oluliselt ainult muutusi kuna viimase küsitlused tsükkeldiagramm vahetatakse. Konnektori ruumi isoleerib ühendatud andmeallika muudatustest Kombelõtvuse automaatselt, mis nõuavad, et konnektor ajakava impordi ja ekspordi. See lisatakse kindlustus annab teile meelerahu testimine, eelvaate või järgmise värskenduse, mis kinnitab ajal.

## <a name="metaverse"></a>Metaversumi

Metaversumi on konsolideeritud kõigi ühendatud identiteeti naabruses konnektor tühikuid.

Identiteedid omavahel seotud ning asutus on määratud eri atribuutide kaudu importimine meilivoo vastendused, hakkab keskse metaversumi objekti liitväärtuse teavet mitme Systemsi. Objekti atribuudi voog, alates vastendused läbi teavet Väljamineva meili jaoks.

Autoriteetsete süsteem projektid need üheks metaversumi luuakse objektid. Kui kõik ühendused on eemaldatud, kustutatakse metaversumi objekti.

Objektide metaversumis ei saa otse redigeerida. Objekti kõigi andmete peab kaasa atribuudi voog kaudu. Metaversumi säilitab püsivate konnektorid iga konnektori ruumi. Need ühendused ei nõua ümberhindamiseks iga sünkroonimine. See tähendab, et Azure'i AD-ühenduse sünkroonimine ei saa iga kord, kui vastavaid remote objekti leidmiseks. See aitab vältida vajadust kulukas agentide atribuute, mis on tavaliselt vastutab tõestusmeetodid objektide muudatuste vältimiseks.

Kui avastamine uute andmeallikate, mis võib olla olemasoleva objekte, mis tuleb hallata, kasutab Azure'i AD-ühenduse sünkroonimine protsessi nimetatakse Liitu reegli hindamaks, kui võimalik leviloendite, mille abil luua seos.
Kui link on loodud, hindamise tekkida ei saa ja tavaline atribuudi voog võivad ilmneda serveri ühendatud andmeallika ja metaversumi vahel.

## <a name="provisioning"></a>Ettevalmistamise

Kui on usaldusväärne allikas projektide loomist teise konnektor tähistav järgneval ühendatud andmeallika uuele objektile arvesse metaverse uue konnektori ruumi objekti.

See loob potentsiaalselt lingi ja atribuudi voog jätkamist kahesuunaliselt.

Iga kord, kui reegel määrab, et konnektori ruumi uuele objektile tuleb luua, seda nimetatakse ettevalmistamise. Kuna seda toimingut toimub ainult konnektori ruumis, seda ei teha, ühendatud andmeallikasse kuni sooritatakse eksport.

## <a name="additional-resources"></a>Lisaressursid

* [Azure'i AD ühenduse sünkroonimine: Kohandamise sünkroonimise suvandid](active-directory-aadconnectsync-whatis.md)
* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
