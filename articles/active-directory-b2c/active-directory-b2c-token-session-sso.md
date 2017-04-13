<properties
    pageTitle="Azure Active Directory B2C: Luba, seansi ja ühekordse sisselogimise konfigureerimine | Microsoft Azure'i"
    description="Luba, seansi ja ühekordse sisselogimise konfigureerimine Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Luba, seansi ja ühekordse sisselogimise konfigureerimine

Selle funktsiooni abil saate kohandatud juhtelemendile [poliitika kohta alus](active-directory-b2c-reference-policies.md)on:
 
1. Eluajal, Turve sõned kiiratava Azure Active Directory (Azure AD) B2C.
2. Eluajal web rakenduse seansside Azure AD B2C hallata.
3. Ühekordne sisselogimine (SSO) käitumine üle mitme rakendused ja -poliitikad teie B2C rentnikus.

Saate selle funktsiooni oma B2C rentniku järgmiselt:

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Klõpsake nuppu **Logi sisse poliitikate**. *Märkus: saate kasutada see funktsioon mistahes poliitika, mitte ainult sisse* *Sisselogimise poliitikate***.
3. Poliitika avamiseks klõpsake seda. Näiteks klõpsake **B2C_1_SiIn**.
4. Klõpsake nuppu **Redigeeri** tera ülaosas.
5. Klõpsake nuppu **Luba, seansi ja ühekordse sisselogimise config**.
6. Tehke soovitud muudatused. Lisateavet saadaolevate atribuudid järgmistes lõikudes.
7. Klõpsake nuppu **OK**.
8. Klõpsake tera ülaosas nuppu **Salvesta** .

![Luba, seansi ja ühekordse sisselogimise config kuvatõmmis](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Turbeloa eluajal konfigureerimine

Azure'i AD B2C toetab turvaline juurdepääs kaitstud ressursse, mis võimaldab [OAuth 2.0 autoriseerimine Protocol (protokoll)](active-directory-b2c-reference-protocols.md) . See tugi kasutusele Azure AD B2C eraldab erinevate [Turvalisus sõned](active-directory-b2c-reference-tokens.md). Need on atribuutide abil saate hallata, Turve sõned kiiratava Azure AD B2C eluajal.

- **Access & ID Turbeloa eluajal (minutites)**: eluiga OAuth 2.0 esitaja luba kasutada juurde pääseda kaitstud ressursi. Azure'i AD B2C probleemid ainult ID sõned sel ajal. See väärtus kehtiksid Accessi sõned ka, kui lisame need tugi.
   - Vaikimisi = 60 minutit.
   - Miinimum (kaasa arvatud) = 5 minutit.
   - Maksimum (kaasa arvatud) = 1440 minutit.
- **Värskendamise sümboolne eluiga (päeva)**: kus saab kasutada värskendamise luba omandada uue Accessi või ID luba maksimaalne ajavahemik (ja soovi korral saate uue värskendamise loa, kui antud rakenduse soovitud `offline_access` ulatus).
   - Vaikimisi = 14 päeva jooksul.
   - Miinimum (kaasa arvatud) = 1 päeva.
   - Maksimum (kaasa arvatud) = 90 päeva.
- **Värskendamise luba libistades akna eluiga (päeva)**: pärast selle tähtaja aja möödumisel kasutaja on sunnitud uuesti autentida, olenemata kehtivuse viimase Värskenda omandanud rakenduse luba. Saab ainult pakkuda, kui parameetrit on seatud **Bounded**. See peab olema suurem või võrdne soovitud **värskendamise sümboolne eluiga (päeva)** väärtus. Kui aktiveerimine on seatud **Unbounded**, te ei saa anda kindla väärtuse.
   - Vaikimisi = 90 päeva.
   - Miinimum (kaasa arvatud) = 1 päeva.
   - Maksimum (kaasa arvatud) = 365 päeva.

Need on mõned näited kasutamine, et nende atribuutide abil saate lubada:

- Kasutajal püsimiseks allkirjastatud Mobiiliversiooni rakendusse lõputult, kui ta on pidevalt aktiivse rakenduse. Saate seda teha, seades selle **Värskenda Turbeloa libistades akna eluiga (päeva)** **Unbounded** aktiveerige oma poliitika.
- Vastavad teie valdkond turvalisus ja nõuetele vastav Accessi Turbeloa eluajal seadmisega.

## <a name="session-configuration"></a>Seansi konfigureerimine

Azure'i AD B2C toetab turvalist sisselogimine veebirakendusi, mis võimaldab [OpenID Connect autentimise protokoll](active-directory-b2c-reference-oidc.md) . Need on atribuutide abil saate hallata web rakenduse seansid.

- **Veebirakenduse seansi eluiga (minutites)**: Azure'i AD B2C seansi küpsise talletatud kasutaja brauseri kinnitamisel eduka eluiga.
   - Vaikimisi = 1440 minutit.
   - Miinimum (kaasa arvatud) = 15 minutit.
   - Maksimum (kaasa arvatud) = 1440 minutit.
- **Web Appi seansi ajalõpp**: kui see Vaheta seatakse **absoluutne**, kasutaja on sunnitud **veebirakenduse seansi eluiga (minutites)** möödub määratud ajaperioodi pärast uuesti autentida. Seda parameetrit on seatud **Rolling** (vaikesäte), kasutaja jääb kehtib, kui kasutaja on oma veebirakenduse pidevalt aktiivne.

Need on mõned näited kasutamine, et nende atribuutide abil saate lubada:

- Vastavad teie valdkond Turve ja nõuetele vastavus nõuetele vastav web rakenduse seansi seadmisega eluajal.
- Jõusta uuesti autentida kasutaja käigus kõrge turvalisusega osa oma veebirakenduse aja möödumisel. 

## <a name="single-sign-on-sso-configuration"></a>Ühekordne sisselogimine (SSO) konfigureerimine

Kui teil on mitu rakenduste ja -poliitikad teie B2C rentnikus, saate hallata kasutaja tegevuste üle nende abil atribuudi **ühekordse sisselogimise konfigureerimine** . Saate seada atribuudi üks järgmistest sätetest:

- **Rentnik**: see on vaikesäte. Selle sätte abil võimaldab mitme rakenduste ja -poliitikad teie B2C rentniku sama kasutaja seansi ühiskasutusse anda. Näiteks kui kasutaja logib rakendusse, Contoso ostmine, millega ta saab ka sujuvalt sisse logida teise ühe, Contoso apteek, tema see.
- **Rakendus**: nii saate säilitada kasutaja seansi ainult rakenduse sõltumatu muude rakenduste jaoks. Näiteks, kui soovite kasutaja sisse logida Contoso apteek (koos samasse mandaat), isegi juhul, kui ta on juba sisse logitud Contoso ostmine, teise rakenduse sama B2C sihtrentnikus. 
- **Poliitika**: See võimaldab teil säilitada kasutaja seansi üksnes poliitika, sõltumatu abil selle rakenduste jaoks. Näiteks kui kasutaja on juba sisse logitud ja lõpetanud mitut tegurit autentimine (MFA) sammu, ta saab pöörata Accessi suurem turvalisus osi, mitme kui poliitika seotud seanssi ei aeguks.
- **Keelatud**: See sunnib kasutaja iga täitmise poliitika kogu kasutaja reisi kaudu käivitada. Näiteks see võimaldab mitu kasutajat (stsenaariumis ühiskasutusega töölaua) rakenduse registreeruda, isegi samal ajal üksikkasutaja jääb kehtib kogu aja jooksul.
