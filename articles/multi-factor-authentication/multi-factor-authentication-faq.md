<properties
    pageTitle="Azure'i Mitmikautentimise KKK"
    description="Korduma kippuvad küsimused ja vastused, mis on seotud Azure'i Mitmikautentimise loetletakse. Mitmikautentimise on kasutaja identiteedi, mis nõuab rohkem kui kasutajanimi ja parool. See pakub väärtpaberi kasutaja sisselogimine ja tehingud."
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
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Azure'i Mitmikautentimise KKK


Sellest artiklist leiate vastused levinud küsimustele Azure'i Mitmikautentimise ja Mitmikautentimise teenus, sh küsimused arvelduse mudel ja kasutatavuse abil.

## <a name="general"></a>Üldine

**Q: kuidas Azure'i mitmekordne autentimine serveri toime kasutaja andmeid?**

Mitme teguriga autentimine serveriga, kasutaja andmed on salvestatud ainult kohapealsed serverid. Püsivate kasutaja andmeid talletatakse pilves. Kui kasutaja sooritab kaheastmelise kontrollimise, saadetakse mitme teguri autentimise Server Azure'i Mitmikautentimise pilveteenuses autentimise andmed. Mitme teguri autentimise Server ja Mitmikautentimise pilveteenuses kasutab turvasoklite kiht (SSL) või transpordikihi Turve (TLS) üle pordi 443 Väljamineva meili.

Aruannete taotluste autentimise saatmise pilveteenusesse, andmed kogutakse autentimise ja kasutamist. Andmevälju sisaldab kaheastmelise kontrollimise logid on järgmised:

- **Ainuidentifikaator** (kas kasutaja nimi või asutusesisese mitmekordne autentimine serveri ID)
- **Ees- ja perekonnanimi** (valikuline)
- **E-posti aadress** (valikuline)
- **Telefoninumber** (kui kasutate mõnda telefonikõne või SMS-autentimine)
- **Seadme luba** (kui mobiilirakenduse autentimist kasutades)
- **Autentimisrežiim**
- **Autentimise tulem**
- **Mitmikautentimise serveri nimi**
- **Mitmikautentimise serveri IP**
- **Kliendi IP** (kui see on saadaval)

Valikuline väljad saab konfigureerida mitme teguriga autentimine serveris.

Kontrollimist tulemus (edu või eitamine) ja kui see on keelatud, põhjus on talletatud andmetega autentimise ja on saadaval autentimis-ja kasutusaruannete.


## <a name="billing"></a>Arveldamine

Enamik arvelduse küsimused saate viidates [mitmekordne autentimise hinnad lehe](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**K: minu asutust telefonikõnede või tekstsõnumi eest tasu kasutatakse minu kasutajate autentimiseks?**

Ettevõtted on eest paigutatakse üksikute helistada või teksti kaudu Azure'i Mitmikautentimise kasutajatele saadetud sõnumid. Telefoni omanikud võib eest telefonikõned või tekstsõnumeid saadetakse, vastavalt oma isiklikku telefoni teenust.

**Q: kas kasutaja arvelduse mudeli tasuta kasutajatele, kellel on konfigureeritud kasutama Mitmikautentimise arv või teha kontrolli kasutajate arvu alusel?**

Arveldamine põhineb konfigureeritud kasutama Mitmikautentimise kasutajate arv.

**Q: kuidas Mitmikautentimise arveldamine toimib?**

Kui kasutate funktsiooni "kasutaja" või "kohta autentimine" mudeli, Azure MFA on tarbimis-põhiste ressurss. Mis tahes kulude eest tuleb tasuda ettevõtte Azure tellimusse nagu virtuaalmasinates, veebisaitide jne.

Litsentsi mudeli kasutamisel Azure'i Mitmikautentimise litsentsi ostetud ja seejärel määratud kasutajatele, nagu Office 365 ja muude tellimuse toodete.

**Q: Kas tasuta versiooni Azure'i Mitmikautentimise administraatorite jaoks?**

Mõnel juhul Jah. Mitmikautentimise Azure administraatoritele pakub tasuta Azure MFA funktsioonide alamhulka. Pakkumine kehtib Azure Active Directory eksemplarid, mis pole seotud tarbimis-põhiste Azure'i Mitmikautentimise pakkuja Azure'i globaalne administraatorite rühma liikmed. Mitmikautentimise pakkuja uuendab kõik administraatorid ja kataloogi kasutajad, kellel on konfigureeritud kasutama Mitmikautentimise Azure'i Mitmikautentimise täisversiooni.

**Q: kas Azure'i Mitmikautentimise tasuta versiooni Office 365 kasutajate jaoks?**

Mõnel juhul Jah. Mitmikautentimise jaoks Office 365 pakub tasuta Azure MFA funktsioonide alamhulka. Pakkumine kehtib kasutajad, kellel on Office 365 litsentsi määratud, kui tarbimis-põhiste Azure'i Mitmikautentimise pakkuja ei olnud seotud vastavate Azure Active Directory eksemplar. Mitmikautentimise pakkuja uuendab kõik administraatorid ja kataloogi kasutajad, kellel on konfigureeritud kasutama Mitmikautentimise Azure'i Mitmikautentimise täisversiooni.

**K: minu asutust ka vaheldumisi kasutada kasutaja autentimise kohta tarbimine arvelduse mudelid ja igal ajal?**

Ettevõtte valib arvelduse mudel loob ressursi. Pärast ressursi on ette valmistatud, ei saa muuta arvelduse mudel. Siiski, saate luua teise Mitmikautentimise ressursi asendamiseks algse. Kasutajasätete ja konfiguratsiooni suvandid ei saa üle uue ressursi.

**Q: Kas minu asutuse vaheldumisi tarbimine arveldus- ja litsentsi mudeli igal ajal?**

Litsentside lisamisel kataloogi, kuhu on juba kasutaja Azure'i Mitmikautentimise pakkuja arveldamine tarbimis-põhine on kahandatud kuuluvad litsentside arv. Kui kõik kasutajad, mis on konfigureeritud kasutama Mitmikautentimise on määratud litsentsid, administraator saab kustutada Azure'i Mitmikautentimise pakkuja.

Ei saa koos kasutada autentimise kohta tarbimine arveldamine litsentsi mudel. Autentimise kohta Mitmikautentimise pakkuja linkimisel kataloog asutuses on arve kõik taotlused Mitmikautentimise kinnitamine, olenemata sellest, mis tahes litsentside kuulub.

**Q: Kas minu ettevõte on kasutada, ja nende sünkroonimine identiteedid Azure'i Mitmikautentimise kasutada?**

Kui mõni ettevõte kasutab tarbimis-põhiste arvelduse mudel, Azure Active Directory ei ole vaja. Mitmikautentimise pakkuja linkimine kataloog pole kohustuslik. Kui teie asutus ei ole seotud kataloogi, saate selle juurutada Azure mitmekordne autentimise Server või Azure mitmekordne autentimise SDK asutusesisene.

Azure Active Directory on vaja litsentsi mudelit, kuna litsentside lisatakse kataloogi, kui ostate ja määrama kasutajatele kataloogis.


## <a name="usability"></a>Kasutatavuse

**Q: mis kasutaja teeb kui nad ei saanud vastust oma telefoni või kui telefon ei ole kasutajale saadaval?**

Kui kasutaja on konfigureeritud varukoopia telefoni, nad proovige uuesti ja valige selle telefon, kui kuvatakse vastav viip, klõpsake lehel sisselogimine. Kui kasutajal pole konfigureeritud mõne muu meetodi, värskendada ettevõtte administraatori määratud kasutaja esmase telefoni number.


**Q: mis ei administraator tegema, kui kasutaja kontaktide administraatori konto, mille kasutaja saab kasutada enam kohta?**

Administraator, saate lähtestada kasutaja kontole, paludes neil läbida registreerimise käigus uuesti. Lisateavet [kasutajate ja Azure Mitmikautentimise pilveteenuses seadme sätete haldamise](multi-factor-authentication-manage-users-and-devices.md)kohta.

**Q: mis ei administraator tegema, kui kasutaja telefon, mis kasutab rakenduse paroolid on kadunud või varastatud?**

Administraator, saate kustutada kõik kasutaja rakenduse paroolid volitamata juurdepääsu vältimiseks. Pärast seda, kui kasutaja on asendamine seade, saate kasutaja paroolid uuesti luua. Lisateavet [kasutajate ja Azure Mitmikautentimise pilveteenuses seadme sätete haldamise](multi-factor-authentication-manage-users-and-devices.md)kohta.

**K: mida teha, kui kasutaja ei saa sisse logida-brauseri rakendusi?**

Kasutaja, kes on konfigureeritud kasutama Mitmikautentimise jaoks on vaja rakenduse parooli mõni brauseris sisse logida. Kasutaja peab tühjendage (Kustuta) sisselogimise teave, taaskäivitage rakendus ja logige sisse oma kasutaja nimi ja rakenduse parooli abil.

Rakenduse paroolid ja muu [abi rakenduse paroolid](multi-factor-authentication-end-user-app-passwords.md)loomise kohta lisateabe saamiseks.


>[AZURE.NOTE] Kaasaegne autentimine Office 2013 klientidele
>
> Office 2013 kliendid (sh Outlook) toetavad uus autentimise protokolle. Saate konfigureerida Office 2013 Mitmikautentimise tugi. Kui olete Office 2013, rakenduse paroolid ei ole vaja Office 2013 kliendid. Lisateavet leiate teemast [Office 2013 kaasaegne autentimise avaliku eelvaate teadaanne](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**Q: mida kasutaja? kui kasutaja ei saa tekstsõnum või kasutaja vastab sõnumile kahesuunaline tekst, kuid ajalõpp kontrollimine**

Teksti sõnumite esitamine ja vastuste kahepoolse SMS ei ole tagatud kuna on kontrollimatu tegureid, mis võivad mõjutada teenuse töökindluse. Teguritest kaasata sihtkoha riik, mobiiltelefoni vedaja ja selle signaal.

Kasutajatele, kellel on raskusi tingimata vastu võtta tekstsõnumeid peaks valige selle asemel Mobile'i rakendus või telefonikõne ajal meetod. Mobiilirakenduse kaudu saate teatisi nii üle mobiilside ja WiFi-ühendus. Lisaks mobiilirakenduse kaudu saate luua kontrollimise koodid isegi siis, kui seade on signaal üldse. Rakendusega Microsoft Authenticator on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073)jaoks.

Kui kasutate tekstsõnumeid, soovitame ühesuunalise SMS, mitte kahesuunalise SMS, kui võimalik. Ühesuunalise SMS on usaldusväärne ja seda ei saa kasutajad tekiks globaalne SMS kulude kaudu vastate sõnumile teksti, mille saatjaks oli teise riigi.


**Q: kasutada riistvara sõned Azure'i mitme teguriga autentimine serveriga?**

Kui kasutate Azure mitmekordne autentimise Server, saate importida kolmanda osapoole avatud autentimine (VANDE) ajapõhiste, ühekordse parooli (TOTP) märkide ja neid kaheastmelise kontrollimise jaoks kasutada.

Saate kasutada ActiveIdentity märgid, mis on VANDE TOTP sõned kui panna salajase võtme faili CSV-faili ja Azure mitmekordne autentimine serverisse importida. Saate kasutada VANDE sõned koos Active Directory Federation Services (ADFS), Remote autentimise sissehelistamise kasutaja teenuse (RADIUS) kui kliendi süsteemi protsess Accessi ülesanne vastuste ja Internet Information Server (IIS) vormipõhist autentimist.

Saate importida kolmanda osa VANDE TOTP sõned koos järgmistes vormingutes:  
- Portable sümmeetriline võtme Container (PSKC)  
- Kui fail sisaldab järjenumbri, salajane võti Base 32-vormingus ja ajaintervalli CSV  

**Q: kas kasutada Azure mitmekordne autentimine serveri turvaline terminaliteenused?**

Jah, kuid kui kasutate Windows Server 2012 R2 või hiljem ainult kasutades kaugtöölaua Gateway (RD lüüs).

Windows Server 2012 R2 turvalisuse muutuste muutnud nii, et Azure'i mitmekordne autentimine Server ühendab kohaliku asutuse (LSA) turvalisus paketi Windows Server 2012 ja varasemate versioonidega. Versioonide terminaliteenused Windows Server 2012 või PowerPointi varasemas versioonis, saate [secure rakenduse Windowsi autentimist](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Kui kasutate opsüsteemi Windows Server 2012 R2, peate Kaugtöölaua lüüs.

**Miks tuleks kasutaja saadud Mitmikautentimise kõne on anonüümne helistaja pärast häälestamist helistaja ID?**

Kui Mitmikautentimise kõned suunatakse üldkasutatava telefonivõrgu kaudu, mõnikord nad suunatakse kaudu carrier, mis ei toeta helistaja ID-ga. Seetõttu helistaja ID pole tagatud, ehkki Mitmikautentimise süsteemi alati saadab.


## <a name="errors"></a>Tõrked

**Q: mis kasutajad toimida juhul, kui need kuvatakse tõrketeade "autentimise taotluse ei ole aktiveeritud konto" mobiilirakenduse teatised kasutamisel?**

Paluge neil mobiilirakenduse kaudu oma kasutajakonto kustutamiseks toimige ja seejärel lisage see uuesti.

1. Minge [portaali Azure profiili](https://account.activedirectory.windowsazure.com/profile/) ja logige sisse oma ettevõtte konto.
2. Valige **täiendavad turvalisus kinnitamine**.
4. Olemasoleva konto eemaldamine mobiilirakenduse kaudu.
5. Klõpsake nuppu **Konfigureeri**ja seejärel järgige juhiseid konfigureerimiseks mobiilirakenduse kaudu.


**Q: mis kasutajad toimida juhul, kui nad näevad 0x800434D4L tõrketeade kuvatakse, kui-brauseri rakendusse sisse logida?**

Praegu kasutaja saate kasutada täiendavaid turvalisus kinnitamine ainult rakendused ja teenused, mida kasutaja pääseb juurde brauseri kaudu. Mitte-brauseri rakendused (nimetatakse ka *rikkaliku klientrakendustes*), mis on installitud kohalikus arvutis, näiteks Windows PowerShelli, ei tööta kontodega, mida suurema turvalisuse tagamiseks kinnitamise nõudmine. Sel juhul kasutaja võidakse kuvada rakenduse luua 0x800434D4L tõrge.

Lahendus on selles, et on eraldi kasutaja moodustab admin seotud ja -administraatori toimingud. Hiljem saate postkasti administraatori konto ja -administraatori konto vahel linkida nii, et te saate sisse logida Outlook-administraatori konto abil. Lugege lisateavet selle kohta, kuidas [anda administraatori võimalus avada ja vaadata kasutaja postkasti sisu](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Järgmised sammud

Kui teie küsimusele pole siin, jätke see kommentaaride lehe allosas. Ja siin on mõned täiendavad suvandid abi saamine:


**Kuidas saan Azure'i Mitmikautentimise abi?**

- Otsige [Microsofti tugiteenuste teabebaasi](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) tehnilise levinud probleemidele lahendusi.

- Otsige ja sirvige tehnilise küsimuste ja vastuste kogukonnast või esitage oma küsimus [Azure Active Directory Foorumid](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Kui olete pärand PhoneFactor klient ja teil on küsimusi või vajate abi parooli lähtestamine, [parooli lähtestamise](mailto:phonefactorsupport@microsoft.com) lingi kaudu avada juhul tugi.

- Tugitöötaja kaudu [Azure'i mitmekordne autentimise Server (PhoneFactor) toe](https://support.microsoft.com/oas/default.aspx?prid=14947)poole. Kui kontaktteave, on kasulik, kui saate lisada nii palju teavet võimalikult probleemi kohta. Teave saate andke sisaldab leht, kus on kuvatud viga, konkreetse tõrkekoodi, teatud seansi ID ja kuvati see tõrge kasutaja ID-d.
