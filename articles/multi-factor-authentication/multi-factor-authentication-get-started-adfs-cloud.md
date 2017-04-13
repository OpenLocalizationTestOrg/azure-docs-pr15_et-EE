<properties
    pageTitle="Azure'i MFA ja AD FS-i Secure cloud ressursid"
    description="See on Azure mitmekordne autentimine leht, mis kirjeldab, kuidas alustada Azure'i MFA ja AD FS-i pilveteenuses."
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

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Pilveteenuse ressursid Azure'i Mitmikautentimise ja AD FS-i turvamine

Kui teie asutus on ühendatud Azure Active Directory, kasutage secure ressursse, mis on Azure AD Azure'i Mitmikautentimise või Active Directory Federation Services. Azure Active Directory ressursse Azure'i Mitmikautentimise või Active Directory Federation Services abil tagamiseks tehke järgmist.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Turvaline Azure AD ressursid AD FS-i abil

Oma pilveteenuses ressursi turvamiseks esmalt konto kasutajatele lubada ja seejärel taotluste reegli häälestamine. Järgige sammult:

1. [Pöörde kohta mitmikautentimise kasutajatele](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) kirjeldatud juhised abil konto.
2. Käivitage AD FS halduskonsool.
![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Liikuge **Tuginedes tootja loodab** ja paremklõpsake tuginedes poole usaldada. Valige **Redigeeri nõude reeglid...**
4. Klõpsake nuppu **Lisa reegel...**
5. Ripploendist, valige **saada taotluste kohandatud reegli abil** ja klõpsake nuppu **edasi**.
6. Sisestage taotluste reegli nimi.
7. Kohandatud reegli alusel: lisage järgmine tekst:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Nõude:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Klõpsake nuppu **OK** ja seejärel **valmis**. Sulgege AD FS halduskonsool.

Kasutajad saavad seejärel teostada sisselogimisel kohapealse meetodil (nt kiipkaardi).

## <a name="trusted-ips-for-federated-users"></a>Välise kasutajate jaoks usaldusväärne IP-d
Usaldusväärsete IP-d luba administraatorid by-pass kaheastmelise kontrollimise teatud IP-aadresside või väliskasutajad, mis on pärit oma sisevõrgu taotlused. Järgmistes jaotistes on kirjeldatud, kuidas konfigureerida Azure mitmekordne autentimine usaldusväärsed IP-d väliskasutajad ja by-pass kaheastmelise kontrollimise, kui väliskasutajad sisevõrgu sees pärineb taotluse. See on saavutada konfigureerimine AD FS-i kasutamine on läbiv või filtreerimiseks on sissetulevate taotluste malli sees ettevõtte võrgus taotluse tüüp.

Selles näites kasutatakse Office 365 jaoks meie tuginedes tootja loodab.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfigureerige AD FS-i nõuete reeglid

Kõigepealt tuleb teha on konfigureerimine AD FS-i nõuded. Loome kaks taotluste reeglid, üks sees ettevõtte võrgus taotluse tüüp ja täiendavate sortimisloendeid pidamise sisse logitud kasutajatele.

1. Avatud AD FS-i haldus.
2. Valige vasakul, **Tuginedes tootja loodab**.
3. **Microsoft Office 365 identiteedi** platvormi paremklõpsake ja valige käsk **Redigeeri nõude reeglid...** 
 ![Pilveteenuses](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Emissiooni muuta reegleid, klõpsake nuppu **Lisa reegel.** 
 ![Pilveteenuses](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Valige ripploendist soovitud **kaudu edastavad või Filter on sissetulevate taotluste** muuta taotluste reegli viisardit, ja klõpsake nuppu **edasi**.
![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Taotluste reegli nimi väljale Tippige oma reegli nimi. Näide: InsideCorpNet.
7. Rippmenüü, kõrval sissetulevad taotlemine tüüp, valige **Sees ettevõtte võrgus**.
![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klõpsake nuppu **valmis**.
9. Emissiooni muuta reegleid, klõpsake nuppu **Lisa reegel**.
10. Valige rippmenüüst **saada taotluste kohandatud reegli abil** muuta taotluste reegli viisardit, ja klõpsake nuppu **edasi**.
11. Jaotises taotluste reegli nimi väljale: Sisestage *hoida kasutajad sisse loginud*.
12. Sisestage väljale kohandatud reegli:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klõpsake nuppu **valmis**.
14. Klõpsake nuppu **Rakenda**.
15. Klõpsake nuppu **Ok**.
16. Sulgege AD FS-i haldus.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigureerida Azure mitmekordne autentimine usaldusväärsete IP-d ühendatud kasutajatega
Nüüd, kui nõuded on olemas, saame seadistada usaldusväärse IP-d.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2. Klõpsake vasakul nuppu **Active Directory**.
3. Valige kaust, kausta, kuhu soovite häälestada usaldusväärsete IP-d.
4. Kataloogi valitud, klõpsake nuppu **Konfigureeri**.
5. Klõpsake jaotises mitmikautentimise **haldamine haldusteenuste sätete**.
6. Valige lehel sätted jaotises usaldusväärsete IP-d, **vahele jätta – mitmikautentimine taotluste välise kasutajate minu sisevõrgus.** 
 ![Pilveteenuses](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klõpsake nuppu **Salvesta**.
8. Kui värskendused on rakendatud, klõpsake nuppu **Sule**.


See on õige! Selles etapis väliskasutajad Office 365 ainult peaks olema kasutada MFA väljaspool ettevõtte sisevõrgu pärineb nõude.
