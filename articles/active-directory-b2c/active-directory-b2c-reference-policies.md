<properties
    pageTitle="Azure Active Directory B2C: Laiendatav raamistik | Microsoft Azure'i"
    description="Teema laiendatav poliitika raamistiku Azure Active Directory B2C ja kuidas luua erinevate poliitika"
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

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Laiendatav raamistik

## <a name="the-basics"></a>Põhitõed

Laiendatav raamistikku Azure Active Directory (Azure AD) B2C on teenuse tugevus. Poliitika täielikult kirjeldavad tarbija identiteedi kogemusi, näiteks registreerumise, sisselogimine või profiili redigeerimine. Näiteks registreerumise poliitika abil saate reguleerida käitumist konfigureerida järgmisi sätteid:

- Kontode kohta (nt Facebooki suhtlusvõrgu kontod või kohalikud kontod, nt meiliaadress) rakenduse kasutajaks tarbijad kasutavad.
- Atribuute (nt eesnimi, sihtnumber ja kinga suurus) kogutakse tarbija registreerumise käigus.
- Mitmikautentimise kasutamine.
- Ja-ilme registreerumise kõik lehed.
- (Mis väljendub nõuded on märgiks), et rakendus saab Millal käivitada viimistluse poliitika.

Saate luua mitu erinevat tüüpi poliitika oma rentniku ja kasutada neid oma rakendused vastavalt vajadusele. Poliitika saab taaskasutada rakendused. See võimaldab arendajatel määrata ja muuta tarbija identiteedi kogemusi minimaalsete või nende koodi muudatusi.

Poliitika on lihtne arendaja kasutajaliidese kaudu kasutamiseks saadaval. Rakenduse käivitab standard HTTP autentimise taotluse (läbides poliitika parameetri taotluse) abil poliitika ja võtab vastu kohandatud luba vastus. Näiteks ainult vahe taotleb kasutada registreerumise poliitika ja neid kasutada sisselogimise poliitika on kasutatud funktsiooni "p" päringustringi parameetri poliitika nimi.

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Poliitika raames kohta leiate lisateavet teemast sellest [ajaveebipostitusest](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Registreerumise poliitika loomine

Klõpsake rakenduse registreerumise lubamiseks peate registreerumise poliitika loomiseks. Selle poliitika kirjeldatakse kogemusi, mis läheb läbi tarbijate registreerumise käigus ja sisu sõned saadud rakenduse kohta eduka sisselogimise läbivaatuste alusel.

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake **registreerumise poliitika**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. **Nime** määrab teie rakendus kasutab registreerumise poliitika nimi. Sisestage näiteks "SiUp".
5. Klõpsake **identiteedipakkujad** ja valige "E-posti registreerimine". Soovi korral võite valida suhtlusvõrgu identiteedipakkujad, ka juhul, kui juba konfigureeritud. Klõpsake nuppu **OK**.
6. Klõpsake **registreerumise atribuute**. Siin saate valida atribuute, mida soovite koguda tarbija registreerumise käigus. Näiteks valige "Riik/regioon", "Kuvatav nimi" ja "Sihtnumber". Klõpsake nuppu **OK**.
7. Klõpsake **rakenduse nõuded**. Siin valida taotluste, mida soovite tagastatud märgid, mis on saadetud tagasi pärast eduka registreerumise rakenduse sisse. Näiteks valige "Kuva nimi", "Identiteedipakkuja", "Sihtnumber", "Kasutaja on uus" ja "Kasutaja objekti ID".
8. Klõpsake nuppu **Loo**. Pange tähele, et äsja loodud poliitika kuvatakse kujul "**B2C_1_SiUp**" (selle **B2C\_1\_ ** fragment lisatakse automaatselt) **registreerumise poliitikate** tera.
9. Poliitika avamiseks klõpsake "**B2C_1_SiUp**".
10. Valige "Contoso B2C app" **rakenduste** ripploendis ja `https://localhost:44321/` sisse selle **vastus URL-i / ümber suunata URI** drop-down.
11. Klõpsake nuppu **Käivita kohe**. Avatakse brauseris uus vahekaart ja tarbija kogemuse sisselogimisega rakenduse kaudu käivitada.

    > [AZURE.NOTE]
    See kulub minutiks poliitika loomine ja Värskenduste jõustumiseks.

## <a name="create-a-sign-in-policy"></a>Sisselogimise poliitika loomine

Sisselogimiseks klõpsake rakenduse lubamiseks peate sisselogimise poliitika loomiseks. Selle poliitika kirjeldatakse kogemusi tarbijad minna ajal sisselogimise ja rakenduse saadud sõned sisu edukaks sisselogimist.

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake nuppu **Logi sisse poliitikate**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. **Nime** määrab teie rakendus kasutab sisselogimise poliitika nimi. Sisestage näiteks "SiIn".
5. Klõpsake **identiteedipakkujad** ja valige "Kohaliku kontoga sisselogimine". Soovi korral võite valida social identiteedipakkujad, ka juhul, kui juba konfigureeritud. Klõpsake nuppu **OK**.
6. Klõpsake **rakenduse nõuded**. Siin saate valida taotluste, mida soovite tagastatud sõned, saata tagasi oma pärast eduka sisselogimine rakendusse sisse. Näiteks valige "Kuvatav nimi", "Identiteedipakkuja", "Sihtnumber" ja "Kasutaja objekti ID". Klõpsake nuppu **OK**.
7. Klõpsake nuppu **Loo**. Pange tähele, et äsja loodud poliitika kuvatakse kujul "**B2C_1_SiIn**" (selle **B2C\_1\_ ** fragment lisatakse automaatselt) **sisselogimise poliitikate** tera.
8. Poliitika avamiseks klõpsake "**B2C_1_SiIn**".
9. Valige "Contoso B2C app" **rakenduste** ripploendis ja `https://localhost:44321/` sisse selle **vastus URL-i / ümber suunata URI** drop-down.
10. Klõpsake nuppu **Käivita kohe**. Avatakse brauseris uus vahekaart ja tarbija kogemus logida rakenduse kaudu käivitada.

    > [AZURE.NOTE]
    See kulub minutiks poliitika loomine ja Värskenduste jõustumiseks.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Registreerumise või sisselogimise poliitika loomine

Selle poliitika tegeleb nii tarbija registreerumise & sisselogimise kogemused ühe konfigureerimine. Tarbijad on viis maha õige tee (registreerumise või Logi sisse) vastavalt kontekstile. Samuti kirjeldatakse sisu sõned saadud rakenduse eduka sisselogimise läbivaatuste alusel või sisselogimist.  Proovi kood registreerumise või sisselogimise poliitika on [saadaval siin](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake **registreerumise või Logi sisse**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. **Nime** määrab teie rakendus kasutab registreerumise poliitika nimi. Sisestage näiteks "SiUpIn".
5. Klõpsake **identiteedipakkujad** ja valige "E-posti registreerimine". Soovi korral võite valida social identiteedipakkujad, ka juhul, kui juba konfigureeritud. Klõpsake nuppu **OK**.
6. Klõpsake **registreerumise atribuute**. Siin saate valida atribuute, mida soovite koguda tarbija registreerumise käigus. Näiteks valige "Riik/regioon", "Kuvatav nimi" ja "Sihtnumber". Klõpsake nuppu **OK**.
7. Klõpsake **rakenduse nõuded**. Siin valida taotluste, mille soovite tagasi märgid, mis on saadetud tagasi rakenduse pärast eduka registreerumise või Logi sisse. Näiteks valige "Kuva nimi", "Identiteedipakkuja", "Sihtnumber", "Kasutaja on uus" ja "Kasutaja objekti ID".
8. Klõpsake nuppu **Loo**. Pange tähele, et äsja loodud poliitika kuvatakse kujul "**B2C_1_SiUpIn**" (selle **B2C\_1\_ ** fragment lisatakse automaatselt) tera **registreerumise või Logi sisse** .
9. Poliitika avamiseks klõpsake "**B2C_1_SiUpIn**".
10. Valige "Contoso B2C app" **rakenduste** ripploendis ja `https://localhost:44321/` sisse selle **vastus URL-i / ümber suunata URI** drop-down.
11. Klõpsake nuppu **Käivita kohe**. Avatakse brauseris uus vahekaart ja käivitada registreerumise või sisselogimise tarbija kogemuse kaudu konfigureeritud.

    > [AZURE.NOTE]
    See kulub minutiks poliitika loomine ja Värskenduste jõustumiseks.

## <a name="create-a-profile-editing-policy"></a>Poliitika redigeerimine profiili loomine

Kasutajaprofiili rakenduse redigeerimise lubamiseks peate profiili redigeerimine poliitika loomiseks. Selle poliitika kirjeldatakse kogemusi, mis tarbijad läheb läbi profiili redigeerimine ja sisu sõned saadud rakenduse eduka lõpetamise ajal.

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake nuppu **profiili redigeerimine poliitika**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. **Nime** määratleb profiili redigeerimine poliitika nimi oma rakendus kasutab. Sisestage näiteks "SiPe".
5. Klõpsake **identiteedipakkujad** ja valige "E-posti aadress". Soovi korral võite valida social identiteedipakkujad, ka juhul, kui juba konfigureeritud. Klõpsake nuppu **OK**.
6. Klõpsake nuppu **profiili atribuute**. Siin saate valida atribuute, mida tarbija saab vaadata ja redigeerida. Näiteks valige "Riik/regioon", "Kuvatav nimi" ja "Sihtnumber". Klõpsake nuppu **OK**.
7. Klõpsake **rakenduse nõuded**. Siin valida taotluste, mida soovite tagastatud märgid, mis on saadetud tagasi pärast eduka profiili redigeerida rakenduse sisse. Näiteks valige "Kuva nimi" ja "Sihtnumber".
8. Klõpsake nuppu **Loo**. Pange tähele, et äsja loodud poliitika kuvatakse kujul "**B2C_1_SiPe**" (selle **B2C\_1\_ ** fragment lisatakse automaatselt) **profiili redigeerimine poliitikate** tera.
9. Poliitika avamiseks klõpsake "**B2C_1_SiPe**".
10. Valige "Contoso B2C app" **rakenduste** ripploendis ja `https://localhost:44321/` sisse selle **vastus URL-i / ümber suunata URI** drop-down.
11. Klõpsake nuppu **Käivita kohe**. Avatakse brauseris uus vahekaart ja redigeerida tarbija ning rakenduse profiili kaudu käivitada.

    > [AZURE.NOTE]
    See kulub minutiks poliitika loomine ja Värskenduste jõustumiseks.
    
## <a name="create-a-password-reset-policy"></a>Parooli lähtestamine poliitika loomine

Kohandatud parool lähtestada oma taotlus lubamiseks peate parooli lähtestamine poliitika loomiseks. Teate, et kogu rentniku parooli lähtestamise suvand määratud [siin](active-directory-b2c-reference-sspr.md) kehtib endiselt poliitikate Logi sisse. Selle poliitika kirjeldatakse kogemusi, mis tarbijate läheb läbi parooli lähtestamine ja sisu sõned saadud rakenduse eduka lõpetamise ajal.

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake nuppu **Lähtesta parool poliitika**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. **Nime** määratleb parool lähtestada oma rakendus kasutab poliitika nimi. Sisestage näiteks "SSPR".
5. Klõpsake **identiteedipakkujad** ja valige "Taasta parool e-posti aadressi". Klõpsake nuppu **OK**.
6. Klõpsake **rakenduse nõuded**. Siin valida taotluste, mida soovite tagastatud märgid, mis on saadetud tagasi rakenduse pärast eduka parooli lähtestamise kogemus sisse. Näiteks valige "Kasutaja objekti ID".
7. Klõpsake nuppu **Loo**. Pange tähele, et äsja loodud poliitika kuvatakse kujul "**B2C_1_SSPR**" (selle **B2C\_1\_ ** fragment lisatakse automaatselt) **parooli lähtestamise poliitikate** tera.
8. Poliitika avamiseks klõpsake "**B2C_1_SSPR**".
9. Valige "Contoso B2C app" **rakenduste** ripploendis ja `https://localhost:44321/` sisse selle **vastus URL-i / ümber suunata URI** drop-down.
10. Klõpsake nuppu **Käivita kohe**. Avatakse brauseris uus vahekaart ja saate oma rakenduse parooli lähtestamine tarbija kogemuse kaudu käivitada.

    > [AZURE.NOTE]
    See kulub minutiks poliitika loomine ja Värskenduste jõustumiseks.

## <a name="additional-resources"></a>Lisaressursid

- [Luba, seansi ja ühekordse sisselogimise konfigureerimine](active-directory-b2c-token-session-sso.md).
