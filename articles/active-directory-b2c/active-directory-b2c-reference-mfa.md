<properties
    pageTitle="Azure Active Directory B2C: Mitmikautentimise | Microsoft Azure'i"
    description="Mitmikautentimise tarbija suunatud rakendustes tagatud Azure Active Directory B2C lubamine"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Lubamine Mitmikautentimise oma kliendile suunatud rakendused

Azure Active Directory (Azure AD) B2C ühendab otse [Azure'i Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication.md) nii, et saate lisada teise kiht turvalisus kogemusi registreerumise ja logige sisse oma kliendile suunatud rakendused. Ja seda kõike ilma üks rida koodi kirjutamist. Praegu toetame telefonikõne ja teksti sõnumi kinnitamine. Kui olete juba loonud registreerumise ja Logi sisse, saate ikkagi Mitmikautentimise lubada.

> [AZURE.NOTE]
Mitmikautentimise ka olema lubatud, kui loote registreerumise ja logige sisse poliitikad, mitte ainult olemasoleva poliitika redigeerimine.

See funktsioon aitab rakenduste toime stsenaariumid, näiteks järgmisi:

- Te ei vaja Mitmikautentimise ühe rakenduse avamiseks, kuid vajate seda veel üks juurdepääsu. Näiteks tarbija saate kindlustusavalduse automaatne social või kohaliku kontoga sisse logida, kuid peab kontrollima telefoninumbrit enne juurdepääs ning home on käsitletavad samas kaustas.
- Te ei vaja juurde pääseda rakenduse üldiselt Mitmikautentimise, kuid nõuavad juurdepääsu tundliku loomuga osade sees. Näiteks tarbija panga rakenduse social või kohaliku konto ja märkige ruut konto saldo saab sisse logida, kuid peab kontrollima telefoninumbrit enne, kui proovite kaabel edastamine.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Teie registreerumise poliitika lubamiseks Mitmikautentimise muutmine

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake **registreerumise poliitika**.
3. Klõpsake oma registreerumise poliitika (nt "B2C_1_SiUp") selle avamiseks.
4. Klõpsake **mitmikautentimise** ja lülitage **olekusse** **sees**. Klõpsake nuppu **OK**.
5. Klõpsake nuppu **Salvesta** tera ülaosas.

Saate kasutada funktsiooni "Run nüüd" poliitika tarbijate kogemusi kinnitamiseks. Kontrollige järgmist.

Tavakasutaja konto luuakse kataloogi enne esimest Mitmikautentimise etappi. Selle toimingu käigus tarbija palutakse sisestada oma telefoninumber ja kinnitage see. Kui kinnitamine õnnestus, telefoninumber on lisatud tavakasutajakonto hilisemaks kasutamiseks. Juhul, kui tarbija tühistab või langeb, ta saab palutakse kinnitamiseks uuesti ajal järgmise sisselogimine telefoninumber (koos lubatud Mitmikautentimise).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Teie sisselogimise poliitika lubamiseks Mitmikautentimise muutmine

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake nuppu **Logi sisse poliitikate**.
3. Klõpsake oma sisselogimise poliitika (nt "B2C_1_SiIn") selle avamiseks. Klõpsake nuppu **Redigeeri** tera ülaosas.
4. Klõpsake **mitmikautentimise** ja lülitage **olekusse** **sees**. Klõpsake nuppu **OK**.
5. Klõpsake nuppu **Salvesta** tera ülaosas.

Saate kasutada funktsiooni "Run nüüd" poliitika tarbijate kogemusi kinnitamiseks. Kontrollige järgmist.

Kui tarbija märgid (abil social või kohaliku konto), kui kinnitatud telefoninumber on seotud tavakasutaja konto, ta palutakse kinnitada. Kui pole telefoninumber on kinnitatud, palutakse tarbija ühe ja kinnitage see. Klõpsake kinnitamise, telefoninumber on lisatud tavakasutajakonto hilisemaks kasutamiseks.

## <a name="multi-factor-authentication-on-other-policies"></a>Mitmikautentimise muu poliitika kohta

Registreerumise & sisselogimise poliitikate eespool kirjeldatud samuti on võimalik lubamiseks klõpsake registreerumise mitmikautentimise või sisselogimise poliitika ja parooli lähtestamiseks poliitika. Oleks saadaval kiiresti kasutajaprofiili poliitikate redigeerimine.
