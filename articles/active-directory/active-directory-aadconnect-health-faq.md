<properties
    pageTitle="Azure'i AD-ühenduse seisund KKK"
    description="Sellest artiklist leiate vastused küsimustele Azure'i AD-ühenduse seisundi kohta. Korduma Kippuvate hõlmab küsimusi teenuse, sh arvelduse mudel, võimaluste, piirangud ja tugiteenuste kasutamise kohta."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure'i AD-ühenduse seisund korduma kippuvad küsimused (KKK)

Sellest artiklist leiate vastused küsimustele Azure'i AD-ühenduse seisundi kohta. Korduma Kippuvate hõlmab küsimusi teenuse, sh arveldustoe mudeli, võimaluste, piirangud ja tugiteenuste kasutamise kohta.

## <a name="general-questions"></a>Üldised küsimused



**K: hallata mitut Azure AD kausta. Kuidas aktiveerida üks Azure Active Directory Premium?**

Mida saab vahetada erinevaid Azure AD rentnikud valimine kehtib praegu kasutaja nimi, klõpsake paremas ülanurgas ja valides sobiva konto. Kui konto on loendis, valige Logi välja ja seejärel kasutage üldadministraatori identimisteabe kataloogi, mis on Azure Active Directory Premium lubatud sisse logida.

## <a name="installation-questions"></a>Installi küsimused



**Q: mis on selle mõju installimise Azure'i AD-ühenduse seisund Agent üksikute serverites?**

Mõju installimist Microsoft Azure'i AD-ühenduse seisund agentide ADFS-i, Web teenuserakenduse puhverserverite, Azure'i AD-ühenduse (sycn) serverid, domeenikontrollerid on minimaalne CPU, mälu tarbimine võrgu läbilaskevõime ja salvestusruumi suhtes.

Allpool on ühtlustamiseks.

- CPU tarbimine: ~ 1% suurendamine
- Mälu: kuni 10% kokku muutmälu

>[AZURE.NOTE] Korral ei saa suhelda Azure agent, agent salvestab andmed, kuni määratletud. Agent kirjutab "vahemällu talletatud" andmed "vähemalt viimati teenindatud" alusel.

- Azure'i AD-ühenduse seisund agentide kohaliku puhvri salvestusruum: ~ 20 MB
- AD FS server, on soovitatav, et olete ette kettaruumi 1024 MB (1 GB) AD FS-i auditeeritavad kanali Azure'i AD-ühenduse seisund agentide enne selle kirjutatakse üle kõik audit andmete töötlemiseks.

**Q: Kas ma pean taaskäivitage oma serverid Azure'i AD-ühenduse seisund agentide installimise ajal?**

Ei. Installimise agentide taaskäivitamist server ei nõua. Siiski võib installitud mõni eelnevalt nõutud juhiseid vaja taaskäivitage server.

Näiteks Windows Server 2008 R2 installimise .net 4.5 Framework nõuab serveri taaskäivitamisest.


**Q: kas Azure AD ühenduse seisund teenuste läbige on läbiv http-puhverserver**

Jah.  Läheb toimingud, saate konfigureerida seisund Agent suunata väljaminev http päringuid abil HTTP-puhverserver. Lisateavet leiate teemast [konfigureerimine Azure'i AD-ühenduse seisund agentide kasutada HTTP-puhverserver.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Kui teil on vaja konfigureerida proxy Agent registreerimise käigus, peate eelnevalt Internet Exploreri puhverserveri sätteid muuta.
1. Avage Internet Explorer -> Sätted -> Interneti-suvandid > ühendused -> Kohtvõrgu sätted.
2. Valige suvand Kasuta puhverserverit oma Kohtvõrgu jaoks.
3. Kui teil on muu proxy portide HTTP ja HTTPS/Secure, klõpsake vahekaarti Täpsemalt.

**Q: kas Azure AD ühenduse seisund teenuste toetab elementaarautentimine Http puhverserverite ühendamisel?**

Ei. Elementaarautentimine suvalise kasutajanime ja parooli määramine süsteem praegu ei toetata.


**Q: millised AD DS versioon on toetatud AD DS Azure'i AD-ühenduse seisund?**

AD DS jälgimine on toetatud ajal installitud järgmised OS versioone:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Toimingute küsimused



**Q: Kas ma pean luba auditi minu AD FS-i teenuserakenduse puhverserverite või minu Web teenuserakenduse puhverserverite?**

Ei, auditeerimine ei pruugi olla lubatud veebi puhverserver (WAP) serverites. Lubamine AD FS-i serverid.


**Q: Kuidas teha Azure'i AD-ühenduse seisund teatiste saamiseks lahendada?**

Azure'i AD-ühendus seisund teatiste saamiseks lahendada edu tingimuse alusel. Azure'i AD-ühendus seisund agentide tuvastada ja aruande edu tingimused teenuse perioodiliselt. Mõned teatised, kaotamise on ajapõhiste. Teisisõnu, kui kuvatakse sama tõrketeade ei järgita teatiste 72 tunni jooksul, teatise automaatselt lahendada.




**Q: millised tulemüüri portide on vaja avamiseks Azure'i AD-ühenduse seisund agent töötada?**

Peab teil olema TCP/UDP-pordid 443 ja 5671 avatud Azure'i AD-ühenduse seisund Agent saama suhelda Azure AD seisund teenuse lõpp-punktid.


**Miks ma näen kaks serverid sama nimega Azure'i AD-ühenduse seisundi portaali?**

Kui eemaldate agent server, server on ei eemaldata automaatselt portaalist Azure'i AD-ühenduse.  Kui käsitsi eemaldada agent server või server ise eemaldatud, peate käsitsi kustutada server Azure'i AD-ühenduse seisund portaalist. Lisateabe saamiseks minge [kustutamine serveri või teenuse eksemplar.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Kui reimaged server või loonud uue server samade andmetega (nt arvuti nimi) ja ei eemalda juba registreeritud server Azure'i AD-ühenduse seisund portaalist, installitud agent uue server, võidakse kuvada kaks kirjet sama nimega.  
Sel juhul tuleks kustutada käsitsi vanemate server kuuluva kirje. Selle serveri andmed peaks olema aegunud.

## <a name="related-links"></a>Seotud lingid

* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund Agent installimine](active-directory-aadconnect-health-agent-install.md)
* [Azure'i AD-ühenduse seisund toimingud](active-directory-aadconnect-health-operations.md)
* [Kasutades Azure AD kasutajaga AD FS-i seisund](active-directory-aadconnect-health-adfs.md)
* [Azure'i AD-ühenduse seisundi kasutamise sünkroonimine](active-directory-aadconnect-health-sync.md)
* [Kasutades Azure AD kasutajaga AD DS seisund](active-directory-aadconnect-health-adds.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
