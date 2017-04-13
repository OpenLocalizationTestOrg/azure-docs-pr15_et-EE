<properties
    pageTitle="Azure'i AD-ühenduse mitu domeeni"
    description="Selles dokumendis kirjeldatakse häälestamise ja konfigureerimise mitu ülataseme domeeni O365 ja Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Mitme domeeni tugi Azure AD ühendamine
Järgmised dokumendid juhised kasutama mitme üladomeenide ja alamdomeenide ühendamine teenusekomplektiga Office 365 või Azure AD domeenid.

## <a name="multiple-top-level-domain-support"></a>Mitme domeeni tugi
Mitme ühendamine, üladomeenide Azure AD nõuab täiendavat konfiguratsiooni, mis ei ole vajalik kui ühendamine ühe ülataseme domeeniga.

Kui domeen on ühendatud Azure AD, mitu atribuudid on määratud Azure domeenis.  Üks on IssuerUri.  See on URI, mida kasutatakse Azure AD tuvastada domeeni, mis on seotud luba.  URI ei pea lahendamiseks midagi, kuid seda peab olema lubatud URI.  Vaikimisi Azure AD määrab see väärtus federation teenuse identifikaator oma asutusesiseses AD FS-i konfigureerimine.

>[AZURE.NOTE]Federation teenuse identifikaator on URI, mis tuvastab kordumatult federation teenus.  Federation teenus on selle funktsioonide AD FS-i eksemplari turvalisus Turbeloa teenus. 

Vew IssuerUri PowerShelli käsu abil saate `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Probleem tekib siis, kui soovime lisada rohkem kui üks kõrgeima taseme Domeen.  Näiteks Oletame, et teil on häälestamise federation vahel Azure AD ja kohapealse keskkonna.  Selle dokumendi jaoks kasutan bmcontoso.com.  Nüüd, kui mul on lisatud teise, kõrgeima taseme Domeen bmfabrikam.com.

![Domeenide](./media/active-directory-multiple-domains/domains.png)

Kui soovime proovivad teisendada meie bmfabrikam.com domeeni, et ühendada, saame tõrke.  Selle põhjuseks on Azure AD on piirang, mis ei luba IssuerUri atribuut on mitu domeeni sama väärtuse.  
  

![Federation tõrge](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parameeter.

Et lahendus, tuleb lisada erinevaid IssuerUri, mida saab teha, kasutades funktsiooni `-SupportMultipleDomain` parameeter.  Seda parameetrit kasutatakse koos järgmised cmdlet-käsud:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Selle parameetri muudab Azure AD konfigureerida selle IssuerUri nii, et see põhineb domeeni nimi.  See on kordumatu üle kataloogide Azure AD.  Parameetriga võimaldab PowerShelli käsu lõpule viia.

![Federation tõrge](./media/active-directory-multiple-domains/convert.png)

Vaadates meie uut bmfabrikam.com domeeni sätted, näete järgmist:

![Federation tõrge](./media/active-directory-multiple-domains/settings.png)

Pange tähele, et `-SupportMultipleDomain` muuta muid lõpp-punktid, mis on konfigureeritud endiselt meie adfs.bmcontoso.com federation teenuse osutamiseks.

Teine asi mis `-SupportMultipleDomain` ei on, et see tagab, et AD FS-i süsteemis sisaldab proper väljaandja väärtust sõned välja Azure AD. See selle kasutaja UPN-i domeeni osa ja väärtuseks IssuerUri, st https://{upn järelliite domeeni} / ADFS-i/teenuste/usalda. 

Seega autentimisel Azure AD või Office 365 kasutaja luba IssuerUri elemendi kasutatakse Azure AD domeeni leidmiseks.  Kui vastet ei leitud autentimine nurjub. 

Näiteks juhul, kui kasutaja UPN-i on bsimon@bmcontoso.com, luba AD FS-i probleemid IssuerUri elemendi seatakse http://bmcontoso.com/adfs/services/trust. See sobib Azure AD konfiguratsiooni ja õnnestub autentimist.

Kohandatud taotluste reeglit, mida rakendatakse see loogika on järgmine:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Kasutage parameetrit - SupportMultipleDomain, kui proovite lisada uusi või teisendada juba lisanud domeeni, peate on häälestatud teie ühendatud usalda, et neid algselt toetavad.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Kuidas värskendada usalda AD FS-i ja Azure AD vahel
Kui häälestate ei ühendatud usalda AD FS-i ja oma eksemplari Azure AD vahel, peate selle usalda uuesti luua.  See on, sest, kui see on algselt ilma häälestus on `-SupportMultipleDomain` parameeter, on seatud soovitud IssuerUri vaikeväärtus.  Saate pildil näete selle IssuerUri on seatud https://adfs.bmcontoso.com/adfs/services/trust.

Nüüd, kui on edukalt lisasime Azure AD portaalis on uus domeen ja seejärel teisendage see abil püüab `Convert-MsolDomaintoFederated -DomainName <your domain>`, meil kuvatakse järgmine tõrketeade.

![Federation tõrge](./media/active-directory-multiple-domains/trust1.png)

Kui proovite lisada soovitud `-SupportMultipleDomain` vahetamise meil kuvatakse järgmine tõrketeade:

![Federation tõrge](./media/active-directory-multiple-domains/trust2.png)

Lihtsalt proovite käivitada `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` algse domeeni tulemuseks viga.

![Federation tõrge](./media/active-directory-multiple-domains/trust3.png)

Kõrgeima taseme täiendava domeeni lisamiseks kasutage alltoodud juhiseid.  Kui olete domeeni juba lisanud ja ei kasuta selle `-SupportMultipleDomain` parameetri alustada eemaldamine ja värskendamise algse domeeni juhised.  Kui olete lisanud kõrgema taseme Domeen veel alustada abil PowerShelli Azure AD ühenduse domeeni lisamise juhiseid.

Järgmiste juhiste abil eemaldada Microsoft Online'i usalda ja värskendage oma algse domeeni.

2.  Teie avatud AD FS liiduserveri **AD FS-i halduse.** 
2.  Klõpsake vasakul laiendamine **Usaldada seosed** ja **Tuginedes tootja loodab**
3.  **Microsoft Office 365 identiteedi platvormi** kirje kustutamiseks klõpsake paremal.
![Microsoft Online eemaldamine](./media/active-directory-multiple-domains/trust4.png)
1.  Käivitage arvutis, mis on [Azure Active Directory moodul Windows PowerShelli jaoks](https://msdn.microsoft.com/library/azure/jj151815.aspx) installitud järgmine: `$cred=Get-Credential`.  
2.  Sisestage kasutajanimi ja parool üldadministraator on ühendamine, kus Azure AD domeeni
2.  Sisestage väljale PowerShelli`Connect-MsolService -Credential $cred`
4.  Sisestage väljale PowerShelli `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  See on algse domeeni.  Nii kasutades ülaltoodud domeenid on:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Järgmiste juhiste abil saate lisada uus kõrgeima taseme Domeen PowerShelli abil

1.  Käivitage arvutis, mis on [Azure Active Directory moodul Windows PowerShelli jaoks](https://msdn.microsoft.com/library/azure/jj151815.aspx) installitud järgmine: `$cred=Get-Credential`.  
2.  Sisestage kasutajanimi ja parool üldadministraator on ühendamine, kus Azure AD domeeni
2.  Sisestage väljale PowerShelli`Connect-MsolService -Credential $cred`
3.  Sisestage väljale PowerShelli`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Järgmiste juhiste abil saate lisada uue ülataseme domeeni Azure'i AD-ühenduse kaudu.

1.  Käivitage Azure'i AD-ühenduse töölaual või menüü start
2.  Valige "Lisada Azure AD täiendava domeeni" ![lisada täiendava domeeni Azure AD](./media/active-directory-multiple-domains/add1.png)
3.  Sisestage oma Azure AD ja Active Directory mandaat
4.  Valige teise domeeni ühinemise jaoks seadistada.
![Lisage veel Azure AD domeeni](./media/active-directory-multiple-domains/add2.png)
5.  Klõpsake nuppu Installi.


### <a name="verify-the-new-top-level-domain"></a>Veenduge, et uue kõrgema taseme Domeen
PowerShelli käsu abil `Get-MsolDomainFederationSettings - DomainName <your domain>`saate vaadata värskendatud IssuerUri.  Pildil kujutatud federation sätted värskendati meie algse domeeni http://bmcontoso.com/adfs/services/trust kohta

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ja IssuerUri meie uut domeeni kohta on seatud https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Alamdomeenide tugi
Kui lisate alamdomeeni, viis Azure AD käsitleda domeenide tõttu, see pärivad ema sätted.  See tähendab, et soovitud IssuerUri peab vastama vanemad.

Nii saab öelda näiteks, et mul on bmcontoso.com ja seejärel lisage corp.bmcontoso.com.  See tähendab, et IssuerUri corp.bmcontoso.com kasutaja jaoks on vaja teha **http://bmcontoso.com/adfs/services/trust.**  Kuid kohal Azure AD, rakendatakse reegel loob märgiks on väljaandja nimega **http://corp.bmcontoso.com/adfs/services/trust.** mis ei sobi selle domeeni nõutav väärtus ja autentimine nurjub.

### <a name="how-to-enable-support-for-sub-domains"></a>Kuidas lubada alamdomeenide tugi
Selleks, et selle lahendamiseks AD FS-i tuginedes poole usaldus Microsoft Online'i jaoks on vaja värskendada.  Selleks tuleb konfigureerida nii, et see ribad välja mis tahes alamdomeenide kaudu kasutaja UPN-i järelliite, kui ehitamine kohandatud väljaandja väärtus kohandatud taotluste reegli. 

Järgmine väide tehke järgmist.

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Järgmiste juhiste abil saate lisada kohandatud taotluste toetamiseks alamdomeenide.

1.  Avatud AD FS-i haldus
2.  Paremklõpsake Microsoft Online'i RP usalda ja valige käsk Redigeeri nõude reeglid
3.  Valige kolmanda taotluste reegel ja asendada ![Redigeeri taotluste](./media/active-directory-multiple-domains/sub1.png)
4.  Asendage praegune taotluste:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    abil
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Asendage taotluste](./media/active-directory-multiple-domains/sub2.png)
5.  Klõpsake nuppu Ok.  Klõpsake nuppu Rakenda.  Klõpsake nuppu Ok.  Sulgege AD FS-i haldus.

