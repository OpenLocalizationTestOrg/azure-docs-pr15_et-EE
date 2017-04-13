<properties
    pageTitle="Azure Active Directory B2C: Parooli iselähtestamine | Microsoft Azure'i"
    description="Mis näitavad, kuidas häälestada Iseteeninduslik parooli lähtestamine tarbijatele Azure Active Directory B2C teema"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Iseteeninduslik parooli lähtestamine tarbijatele häälestamine

Iseteeninduslik parooli lähtestamise funktsiooni abil saate tarbijatele, (kes on registreerunud kohalikud kontod) oma paroolide lähtestamiseks. See vähendab koormust oma töötajate, eriti siis, kui teie taotlus on miljonid tarbijad seda regulaarselt kasutama. Praegu toetame ainult kasutamine kinnitatud meiliaadress taastamise meetod. Lisame tulevikus täiendavad taastamine võimalused (kinnitatud telefoninumber, turvaküsimused jne).

> [AZURE.NOTE]
See artikkel kehtib Iseteeninduslik parooli lähtestamine kasutatakse sisselogimise poliitika kontekstis. Kui teil on vaja täielikult kohandatavat parooli lähtestamise kasutada rakenduste poliitikad, lugege [artiklit](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Vaikimisi ei ole kataloogi Iseteeninduslik parooli lähtestamine sisse lülitatud. Järgmiste juhiste abil saate sisse lülitada:

1. Logige sisse [Azure klassikaline portaali](https://manage.windowsazure.com/) tellimuse administraatorina. See on sama töö või kooli kontoga või kataloogi loomiseks kasutatud sama Microsofti kontoga.
2. Liikuge Active Directory laiend navigeerimisriba vasakul pool.
3. Otsige vahekaardil **Directory** kataloogi ja klõpsake seda.
4. Klõpsake vahekaarti **konfigureerimine** .
5. Liikuge kerides jaotiseni **kasutaja parooli lähtestamine poliitika** ja määrake **kasutajatele lubatud parooli lähtestamiseks** võimalus **Jah**. Pange tähele, et suvand **Alternatiivne meiliaadress** on märgitud; kui see on, jätke see.

    ![Iseteeninduslik parooli lähtestamine](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Klõpsake lehe allosas **salvestada** . Olete valmis!

Testimiseks kasutada funktsiooni "Käivita kohe" mis tahes sisselogimise poliitika, mis sisaldab kohalikud kontod identiteedi pakkuja. Kohaliku kontoga sisselogimine lehel (kui sisestate meiliaadressi ja parooli või kasutajanime ja parooli), klõpsake **ei pääse oma kontole juurde?** tarbijate kogemusi kinnitamiseks.

> [AZURE.NOTE]
Iseteeninduslik parooli lähtestamine lehti saab kohandada [ettevõtte tootemargi funktsioonide](../active-directory/active-directory-add-company-branding.md)abil.
