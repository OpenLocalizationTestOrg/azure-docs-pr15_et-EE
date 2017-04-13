<properties 
    pageTitle="Azure'i Mitmikautentimise aruanded"
    description="See kirjeldatakse, kuidas muuta kasutaja sätteid, näiteks sunnib kasutajate tõendada üles protsessi uuesti teha."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Azure'i Mitmikautentimise pilves ja kasutajale sätete haldamine

Administraatorina saate hallata kasutaja ja seadme järgmised sätted.  

- [Kontaktandmete uuesti esitama valitud kasutajad](#require-selected-users-to-provide-contact-methods-again)
- [Rakenduse paroolid olemasoleva kasutaja kustutamine](#delete-users-existing-app-passwords)
- [MFA peatatud kõigis seadmetes, kasutaja taastamine](#restore-mfa-on-all-suspended-devices-for-a-user)






See on kasulik, kui arvutis või seadmes on kadunud või varastatud või peate eemaldama kasutajate juurdepääs.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Kontaktandmete uuesti esitama valitud kasutajad

See säte on jõustab lõpuleviimiseks registreerimise käigus uuesti, kui ta logib kasutaja. Pange tähele, et jätkab-brauseri rakendused töötavad, kui kasutajal on nende jaoks rakenduse paroolid.  Saate kustutada rakenduse kasutajate paroole, valides ka **olemasoleva rakenduse paroole loodud valitud kasutajatele**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Kuidas kasutajad kontaktandmete uuesti esitama




1. Logige sisse Azure klassikaline portaali.
2. Klõpsake vasakul nuppu Active Directory.
3. Jaotises, klõpsake Directory kataloogi soovite esitada oma meetodi uuesti nõuda kasutaja jaoks.
4. Ülaosas nuppu kasutajad.
5. Klõpsake lehe allosas nuppu hallata mitut tegurit Auth. See avab mitmikautentimise lehe.
6. Leidke kasutaja, mida soovite hallata ja märkige nime kõrval asuv ruut. Kui peate vaate ülaosas.
7. See toob **haldamine kasutajasätete** link paremal. Klõpsake seda.
8. Märkige **valitud kasutajad kontakti meetodid uuesti**.
![Kontakti meetodid](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Klõpsake nuppu Salvesta.
11. Klõpsake nuppu Sule.

## <a name="delete-users-existing-app-passwords"></a>Rakenduse paroolid olemasoleva kasutaja kustutamine

See kustutab kõik kasutaja loodud rakenduse paroolid. Mitte-brauseri rakenduste seostatud need rakenduse paroolid eluiga seni, kuni on loodud uue rakenduse parooli.

### <a name="how-to-delete-users-existing-app-passwords"></a>Kuidas rakenduse paroolid olemasoleva kasutaja kustutamine

1. Logige sisse Azure klassikaline portaali.
2. Klõpsake vasakul nuppu Active Directory.
3. Jaotises, klõpsake Directory kataloogi kasutaja kustutada rakenduse paroolid.
4. Ülaosas nuppu kasutajad.
5. Klõpsake lehe allosas nuppu hallata mitut tegurit Auth. See avab mitmikautentimise lehe.
6. Leidke kasutaja, mida soovite hallata ja märkige nime kõrval asuv ruut. Kui peate vaate ülaosas.
7. See toob **haldamine kasutajasätete** link paremal. Klõpsake seda.
8. Märkige **olemasoleva rakenduse paroole loodud valitud kasutajatele**.
![Rakenduse paroolid kustutamine](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Klõpsake nuppu Salvesta.
10. Klõpsake nuppu Sule.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA meeles peetud kõigis seadmetes, kasutaja taastamine

Administraatoritel on võimalus taastamine Mitmikautentimise kasutajate seadmed ja brauserid. Seda tehes see eemaldab meeles MFA kõikides kasutaja seadmetes ja brauserid ja kasutajale tuleb kasutada MFA järgmine kord sisse logides.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA peatatud kõigis seadmetes, kasutaja taastamine

1. Logige sisse Azure klassikaline portaali.
2. Klõpsake vasakul nuppu Active Directory.
3. Jaotises, klõpsake Directory kataloogi soovite taastada mfa kasutaja jaoks.
4. Ülaosas nuppu kasutajad.
5. Klõpsake lehe allosas nuppu mitmekordne Auth. haldamine See avab mitmikautentimise lehe.
6. Leidke kasutaja, mida soovite hallata ja märkige nime kõrval asuv ruut. Kui peate vaate ülaosas.
7. See toob **haldamine kasutajasätete** link paremal. Klõpsake seda.
8. Märkige **taastatava meeles peetud kõigis seadmetes mitmikautentimise**
![kustutamine rakenduse paroolid](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klõpsake nuppu Salvesta.
10. Klõpsake nuppu Sule.
