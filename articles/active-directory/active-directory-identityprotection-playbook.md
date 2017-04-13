<properties
    pageTitle="Azure Active Directory identiteedi kaitse playbook | Microsoft Azure'i"
    description="Siit saate teada, kuidas Azure'i AD identiteedi kaitse võimaldab piirata ründaja rikutud identiteedi või seadme ja secure identiteedi või seadme, mis on varem arvatav või teadaolevad konfliktid."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika haldamine"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identiteedi kaitse playbook 

See playbook abil saate:

- Andmete kaitse keskkonnas asustada simuleerib risk sündmused ja nõrkade kohtade
- Juurdepääsu riski põhjal poliitikate häälestamine ja testimine nende mõju


## <a name="simulating-risk-events"></a>Simuleerida Risk sündmused

Selles jaotises antakse teile juhistega simuleerida risk sündmuse järgmist tüüpi:

- Sisselogimist anonüümse IP-aadresside (lihtne)
- Sisselogimist võõras asukohtadest (keskmine)
- Võimalik reisimise ebatüüpiliste asukohtadesse (raske)

Risk muid sündmusi ei saa modelleerida turvaline viisil.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Sisselogimist anonüümse IP-aadressid

Risk sündmuse tüüp tuvastab kasutajad, kes on edukalt sisse logitud, IP-aadress, mis on määratletud anonüümse puhverserveri IP-aadress. Inimesed, kellele soovite peita oma seadme IP-aadress ja võib kasutada pahatahtlikul eesmärgil kasutatakse neid puhvrid.

**Simuleerida sisselogimine anonüümse IP, tehke järgmist**.

1.  Laadige alla [Tor brauseris](https://www.torproject.org/projects/torbrowser.html.en).
2.  Tor brauseri kaudu, liikuge [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Sisestage mandaat soovite kuvada aruandes **anonüümse IP-aadresside kaudu sisselogimist** konto.

Logi sisse kuvatakse armatuurlaual identiteedi kaitse 5 minuti jooksul. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Võõras asukohtadest sisselogimist

Võõras asukohad risk on reaalajas hindamise leiab viimase sisselogimise asukohad (IP-, laiuskraad / pikkuskraad ja ASN) määratlemiseks uute / võõras asukohad. Süsteem talletab eelmise IP-d, laiuskraad / pikkuskraad ja ASNs kasutaja ja leiab, et neid tuttav asukohad. Sisselogimise kohta käsitletakse võõras, kui soovitud asukohta ei sobi ühegi olemasoleva tuttava asukohad.

Azure Active Directory identiteedi kaitse:  

 - on algse õ tähtaeg 14 päeva jooksul selle lipuga, mis tahes uutesse asukohtadesse nimega võõras asukohad.
 - ignoreerib tuttavad seadmed ja see on geograafiliselt lähedal olemasoleva tuttava asukoha sisselogimist.

Võõras asukohad jäljendamiseks tuleb asukohast ja seadme, mille konto on pole sisse logitud: enne sisse logida. 


**Simuleerida sisselogimine võõras asukohast, tehke järgmist**.

1.  Valige kontoga, millel on vähemalt 14 päeva sisselogimine ajalugu. 

2.  Tehke.
    
    lisamine. Kasutades VPN, liikuge [https://myapps.microsoft.com](https://myapps.microsoft.com) ja sisestage selle konto simuleerida risk sündmuste jaoks.

    b. Paluge mõnes muus asukohas seostada sisselogimine konto mandaadiga (pole soovitatav).

Logi sisse kuvatakse armatuurlaual identiteedi kaitse 5 minuti jooksul.
 
### <a name="impossible-travel-to-atypical-location"></a>Võimalik reisimise ebatüüpiliste asukohta
Simuleerida võimalik reisimise tingimus on keeruline, kuna algoritmi kasutab masina õppida rookima vale-positiivsed nt võimalik reisimise tuttavad seadmetest või teiste kasutajate kataloogis kasutatud VPN kaudu sisselogimist. Lisaks algoritmi vajab 3 14 päeva ajalugu sisselogimise kasutaja, enne selle algust, luues risk sündmuste.

**Simuleerida on võimalik reisimise ebatüüpiliste asukohta, tehke järgmist**.

1.  Standardse brauseri kaudu, liikuge [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Sisestage mandaat konto, mida soovite sündmuse võimalik reisi risk jaoks luua.

3.  Saate muuta oma kasutajaagendi. Saate muuta kasutajaagendi Internet Exploreris tööriistad arendaja või muuta oma kasutajaagendi Firefoxi või Chrome'i kasutajaagendi vahetaja lisandmooduli abil.

4.  Saate muuta oma IP-aadress. Saate muuta VPN, Tor lisandmoodul, kasutades või valmistamata üles uue masina Azure erinevate andmekeskuse IP-aadressi.

5.  Logige sisse [https://myapps.microsoft.com](https://myapps.microsoft.com) sama mandaadiga nagu enne ja pärast eelmise sisselogimine mõne minuti jooksul.

Logi sisse ilmub identiteedi kaitse armatuurlaua 2 – 4 tunni jooksul.<br>
Keerukate masina õppe mudelid kaasatud, kuna on võimalus see ei saada valisin üles.<br> Võite neid juhiseid mitme Azure AD kontode puhul korrata.


## <a name="simulating-vulnerabilities"></a>Nõrkade kohtade simuleerida 
Nõrkade kohtade on nõrkuste Azure AD keskkonnas, kus saate ära halb näitleja. Praegu on 3 tüüpi kohtade uurinud Azure AD identiteedi kaitse, mida kasutada muude funktsioonide Azure AD. Nende nõrkade kohtade kuvatakse armatuurlaual identiteedi kaitse automaatselt kui need funktsioonid on häälestatud.

-   Azure AD [Mitmikautentimise?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure AD [Pilveteenuse rakenduse Discovery](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [õigustega identiteetide haldus](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Kasutaja kompromiss risk

**Kasutaja kompromiss risk, testimiseks tehke järgmist**.

1.  Sisselogimine [https://portal.azure.com](https://portal.azure.com) üldadministraator mandaati oma rentniku jaoks.

2.  Liikuge **identiteedi kaitse**. 

3.  Peamised **Azure AD identiteedi kaitse** enne, klõpsake nuppu **sätted**. 

4.  Enne **Videoportaali sätted** , **turvalisuse reeglid**, klõpsake **kasutaja kahjustada risk**. 

5.  Enne **sisselogimine Risk** **lubamine reegel** välja lülitada ja seejärel klõpsake nuppu **Salvesta** sätted.

6.  Antud kasutajakonto simuleerida on võõras asukohtadele või anonüümse IP risk sündmus. See tõstab kasutaja on kasutaja taseme **Keskmine**.

7.  Oodake mõni minut ja seejärel veenduge, et kasutaja taseme oma kasutaja **keskmise**.

8.  Minge tera **Videoportaali sätted** .

9.  **Kasutaja kompromiss Risk** enne, klõpsake jaotises **reegli lubamine**, **klõpsake** nuppu. 

10. Valige üks järgmistest suvanditest:

    lisamine. Blokeerida, valige jaotises **Blokeeri sisselogimine** **keskmise** .

    b. Turvaline parooli muutmine, märkige jaotises **nõua mitmikautentimise** **keskmise** .

13. Klõpsake nuppu **Salvesta**.

14. Nüüd saate juurdepääsu riski põhjal testimiseks sisselogimine abil kasutaja, kus administraatoriõigustes risk. Kui kasutaja on keskmine, teie poliitika konfiguratsioonist, teie sisselogimine on kas blokeerida või olete sunnitud parooli muutmine. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Risk sisselogimine

 
**Risk märk testimiseks tehke järgmist.**

1.  Sisselogimine [https://portal.azure.com](https://portal.azure.com) üldadministraator mandaati oma rentniku jaoks.

2.  Liikuge **identiteedi kaitse**.

3.  Peamised **Azure AD identiteedi kaitse** enne, klõpsake nuppu **sätted**. 

4.  Enne **Videoportaali sätted** , **turvalisuse reeglid**, klõpsake **risk sisse logida**.

5.  Valige enne **Risk sisse logida **, **klõpsake** jaotises **reegli lubamine**. 

7.  Valige üks järgmistest suvanditest:

    lisamine. Valige blokeerimiseks **keskmise** jaotises **Blokeeri sisselogimine**

    b. Turvaline parooli muutmine, märkige jaotises **nõua mitmikautentimise** **keskmise** .

8.  Valige jaotises Blokeeri keskmise blokeerida, logige sisse.

9.  Mitmikautentimise, märkige jaotises **nõua mitmikautentimise** **keskmise** .

10. Klõpsake **Salvesta**.

11. Nüüd saate juurdepääsu riski põhjal jäljendamine võõras asukohtade või anonüümse IP risk sündmusi, kuna need on nii **keskmise** risk üritused testida.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Vt ka

 - [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md)
