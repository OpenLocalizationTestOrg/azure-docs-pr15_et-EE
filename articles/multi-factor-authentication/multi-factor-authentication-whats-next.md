<properties
    pageTitle="Azure'i Mitmikautentimise – mis saab edasi"
    description="See on Azure mitmekordne autentimine leht, mida teha järgmisena MFA kirjeldav.  See hõlmab aruanded, pettuse teatise, ühekordse meediumierandi kohandatud Kõnepost, vahemällu talletamine usaldusväärsete IP-d ja rakenduse paroolid."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise konfigureerimine

See artikkel aitab teil hallata Azure Mitmikautentimise nüüd, kui olete valmis ja töötab.  See hõlmab erinevaid teemasid, mis aitavad teil parimal viisil Azure'i Mitmikautentimise.  Kõik need funktsioonid on saadaval iga Azure'i Mitmikautentimise versiooni.

Allpool funktsioonide konfigureerimine leiate Azure'i haldusportaal mitme teguri autentimist. On kaks võimalust millele pääsete juurde MFA haldusportaali, mis on mõlemad tehtud Azure portaali kaudu. Esimene on mitme teguri Auth pakkuja haldamine, kui tarbimis-põhiste MFA kasutamine. Teine on MFA Teenusesätted kaudu. Teine võimalus jaoks on vaja mitut tegurit Auth pakkuja või on Azure MFA, Azure AD Premium või ettevõtte mobiilsus litsents.

MFA haldusportaali Azure mitmekordne Auth pakkuja kaudu juurdepääsu Azure portaali administraatorina sisse logida ja valige suvand Active Directory. Klõpsake vahekaarti **Mitmekordne Auth pakkujate** valige kataloogi ja allosas nuppu **haldamine** .

MFA haldusportaali MFA Teenusesätted lehe kaudu juurdepääsu Azure portaali administraatorina sisse logida ja valige suvand Active Directory. Klõpsake kataloogi ja seejärel klõpsake vahekaarti **konfigureerimine** . Valige jaotises mitmikautentimise **haldamine haldusteenuste sätete**. MFA Teenusesätted lehe allosas linki **portaalis** .


Funktsioon| Kirjeldus| Mida hõlmab
:------------- | :------------- | :------------- |
[Pettuse teatis](#fraud-alert)|Pettuse teatise saate konfigureeritud ja häälestada nii, et saate oma Kasutajad kurdavad pettusega volitamata juurdepääsu oma ressursse.|Kuidas häälestada, konfigureerimine ja aruande pettuse
[Ühekordse meediumierandi](#one-time-bypass) |Ühekordse meediumierandi saab kasutaja autentida "mööda" mitmikautentimise ühel ajal.|Kuidas häälestada ja konfigureerida ühekordset meediumierandi
[Kohandatud Kõnepost](#custom-voice-messages) |Kohandatud häälsõnumitega saate kasutada oma salvestiste või tervitust mitmikautentimise. |Kuidas häälestada ja konfigureerida kohandatud Tervitused ja sõnumite
[Vahemällu talletamine](#caching-in-azure-multi-factor-authentication)|Vahemällu võimaldab teil määrata teatud aja jooksul nii, et edaspidised autentimise katsed õnnestub automaatselt. |Kuidas häälestada ja konfigureerida autentimine, vahemällu.
[Usaldusväärsete IP-d](#trusted-ips)|Usaldusväärsed IP-d on mitmikautentimise, mis võimaldab hallatavate või ühendatud rentniku administraatorite võimalus mööduda mitmikautentimise kasutajatele, kellel on ettevõtte kohaliku sisevõrgu sisse logida.|Konfigureerimine ja IP-aadressid, mis on välistatud jaoks mitmikautentimise häälestamine
[Rakenduse paroolid](#app-passwords)|Rakenduse parooli võimaldab rakendus, mis ei ole MFA-teadlik mööduda mitmikautentimise ja tööd jätkata.|Teavet rakenduse paroolid.
[Pidage meeles Mitmikautentimise jaoks meeles peetud seadmed ja brauserid](#remember-multi-factor-authentication-for-devices-users-trust)|Võimaldab teil meeles pidada seadmete jaoks teatud arv päeva pärast seda, kui kasutaja on edukalt sisse loginud MFA kasutamine.|Teave selle funktsiooni ja häälestamiseks päevade arv.
[Valitav kontrollimise meetodid](#selectable-verification-methods)|Võimaldab valida autentimismeetodite, mis on saadaval, et kasutajad saaksid kasutada.|Teave lubamine või keelamine teatud autentimismeetodite, nt kõne või tekstsõnumeid.



## <a name="fraud-alert"></a>Pettuse teatis
Pettuse teatise saate konfigureeritud ja häälestada nii, et saate oma Kasutajad kurdavad pettusega volitamata juurdepääsu oma ressursse.  Kasutajad saavad aruande pettuse mobiilirakenduse kaudu või oma telefoni kaudu.

### <a name="to-set-up-and-configure-fraud-alert"></a>Häälestada ja konfigureerida pettuse teatis

1.  Logige sisse http://azure.microsoft.com
2.  Liikuge kohta juhiseid selle lehe ülaosas MFA haldusportaali.
3.  Azure'i mitmekordne autentimine haldusportaal konfigureerimine jaotises nuppu sätted.
4.  Märkige jaotises pettuse Teatiste sätete lehe jaotises esitada pettuse teatiste ruut Luba kasutajatel.
5.  Kui soovite blokeerida pettuse esitatakse kasutajad, märkige Blokeeri kasutaja kui pettuse on teatatud.
6.  Sisestage tekstiväljale **Koodi abil aruande pettuse ajal algsete tervitus** kood, mida saab kasutada ajal kõne kinnitamine. Kui kasutaja sisestab selle koodi pluss # asemel ainult # rakendusse, siis pettuse teatise teatatakse.
7.  Allosas, klõpsake nuppu Salvesta.

>[AZURE.NOTE]
>Microsofti vaikimisi kõneposti tervitust Paluge kasutajatel vajutage 0# pettuse teatise esitada. Kui soovite kasutada koodi peale 0, tuleks salvestada ja üles oma kohandatud kõneposti tervitust asjakohased juhised.


![Pilveteenuse](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Kui soovite aruande pettuse teatis
Pettuse teatise saab esitada kaks võimalust.  Kas mobiilirakenduse või telefoni kaudu.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Kui soovite aruande pettuse teatise mobiilirakenduse abil



1. Kui kontrollimine saadetakse telefoni, valige see alustada rakendusega Microsoft Authenticator.
2. Aruande pettuse, klõpsake nuppu Loobu ja aruande pettuse. Kuvatakse tabeliveergude kast, mis ütleb ettevõtte IT toe töötajate teavitatakse.
3. Klõpsake aruande pettuse.
4. Rakendus, klõpsake nuppu Sule.

![Pilveteenuse](./media/multi-factor-authentication-whats-next/report1.png)


![Pilveteenuse](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Kui soovite aruande pettuse teatise telefoniga

1. Kui kinnitamine kõne tuleb oma telefoni, vastata.  
2. Aruande pettuse, sisestage kood, mis on konfigureeritud aruandlus pettuse telefoni ja seejärel logige # kaudu suhtlemiseks. Kuvab teate, et pettuse teatis on esitatud.
3. Kõne lõpetamiseks.

### <a name="to-view-the-fraud-report"></a>Pettuse aruande vaatamiseks

1. Logige sisse [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Valige vasakul Active Directory.
3. Valige ülaservas mitmekordne Auth pakkujad. Kuvatakse tabeliveergude oma mitme teguri Auth pakkujate loendit.
4. Kui teil on rohkem kui üks mitme teguri Auth pakkuja, valige konfigureeritav pettuse teatiste aruannet ja klõpsake siis nuppu Halda lehe allosas. Kui teil on ainult üks, klõpsake nuppu Halda. Avatakse Azure'i haldusportaal mitme teguri autentimist.
5. Azure'i mitmekordne autentimine haldusportaal, vasakul, klõpsake jaotises Kuva A aruanne, klõpsake nuppu pettuse teatis.
6. Määrake väljal kuupäevavahemik, mida soovite aruandes kuvada. Samuti saate määrata mis tahes teatud kasutajanimed, telefoninumbrid ja kasutaja olek.
7. Klõpsake nuppu Käivita. Kuvatakse tabeliveergude aruande, mis sarnaneb allpool. Kui soovite eksportida aruande, võite klõpsata ka eksportimine CSV-vormingus failide.

## <a name="one-time-bypass"></a>Ühekordse meediumierandi

Ühekordse meediumierandi saab kasutaja autentida "mööda" mitmikautentimise ühel ajal. Funktsiooni meediumierandi on ajutine ja lõpeb pärast määratud sekundite arv.  Nii, et olukordades, kus mobiilirakenduse või telefoni ei saanud teatist või telefonikõne ajal, saate lubada ühekordse meediumierandi nii, et kasutaja saab kasutada soovitud ressurss.

### <a name="to-create-a-one-time-bypass"></a>Ühekordse meediumierandi loomiseks

1.  Http://azure.microsoft.com sisse logida
2.  Liikuge kohta juhiseid selle lehe ülaosas MFA haldusportaali.
3.  Azure'i mitmekordne autentimine haldusportaal, kui näete oma rentniku või Azure MFA pakkuja nimi vasaku on + kõrval, klõpsake selle + teemast erinevate MFA serveri dispersioonanalüüs rühmad ja Azure vaikimisi rühma. Klõpsake soovitud rühma.
4.  Klõpsake jaotises Administreerimine kasutaja **One-Time Meediumierandi**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/create1.png)
5.  Klõpsake lehel One-Time Meediumierandi **Uue One-Time Meediumierandi**.
6.  Sisestage kasutaja kasutajanimi, sekundites, mis on meediumierandi on olemas, põhjuseks on meediumierandi ja klõpsake **mööduda**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/create2.png)
7.  Selles etapis kasutaja peate sisse logima enne ühekordse meediumierandi lõppemist.



### <a name="to-view-the-one-time-bypass-report"></a>Ühekordse meediumierandi aruande vaatamiseks

1. Logige sisse [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Valige vasakul Active Directory.
3. Valige ülaservas mitmekordne Auth pakkujad. Kuvatakse tabeliveergude oma mitme teguri Auth pakkujate loendit.
4. Kui teil on rohkem kui üks mitme teguri Auth pakkuja, valige konfigureeritav pettuse teatiste aruannet ja klõpsake siis nuppu Halda lehe allosas. Kui teil on ainult üks, klõpsake nuppu Halda. Avatakse Azure'i haldusportaal mitme teguri autentimist.
5. Azure'i mitmekordne autentimine haldusportaal, vasakul, klõpsake jaotises Kuva A aruanne, klõpsake nuppu One-Time Meediumierandi.
6. Määrake väljal kuupäevavahemik, mida soovite aruandes kuvada. Samuti saate määrata mis tahes teatud kasutajanimed, telefoninumbrid ja kasutaja olek.
7. Klõpsake nuppu Käivita. Kuvatakse tabeliveergude aruande, mis sarnaneb allpool. Kui soovite eksportida aruande, võite klõpsata ka eksportimine CSV-vormingus failide.

<center>![Pilveteenuse](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Kohandatud Kõnepost

Kohandatud häälsõnumitega saate kasutada oma salvestiste või tervitust mitmikautentimise.  Neid saab kasutada lisaks või asendamine Microsoft kirje.

Enne alustamist arvestage järgmist:

- Praeguse toetatud failivormingud on WAV- ja MP3.
- Faili maht on 5 MB.
- Soovitatav on, et jaoks autentimine sõnumite, et see ei tohi ületada 20 minutit. Kõigele, mis on suurem kui see võib põhjustada kontrolli nurjuda, kuna kasutaja ei pruugi vastata enne sõnumi lõppemist ja kinnitamine ajalõpp.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Kui soovite kohandatud häälsõnumeid Azure'i Mitmikautentimise häälestamine
1.  Luua kohandatud kõnesõnumi, kasutades ühte järgmistest toetatud failivormingud.
2.  Logige sisse http://azure.microsoft.com
3.  Liikuge kohta juhiseid selle lehe ülaosas MFA haldusportaali.
4.  Klõpsake haldusportaali Azure'i mitmekordne autentimine konfigureerimine jaotises Kõnepost.
5.  Klõpsake jaotises Kõnepost **Heli uus sõnum**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Klõpsake soovitud konfigureerimine: uusi kõneposti teateid klõpsake **Heli failide haldamine**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Klõpsake soovitud konfigureerimine: heli failide lehe, klõpsake **Heli faili üles laadida**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Klõpsake soovitud konfigureerimine: heli faili üles laadida, klõpsake nuppu **Sirvi** ja liikuge oma hääle sõnum, klõpsake nuppu **Ava**.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Lisage kirjeldus ja klõpsake käsku Laadi üles.
10. Kui see on lõpule jõudnud, sõnumi kinnitab, et teil on edukalt üles fail.
11. Klõpsake vasakul nuppu Kõnepost.
12. Klõpsake jaotises Kõnepost heli uus sõnum.
13. Valige ripploendist soovitud keel, keel.
14. Kui see sõnum on konkreetse rakenduse jaoks, määrake see väljal rakenduse.
15. Sõnumi tüüp, valige sõnumi soovite meie uut kohandatud teatega eirata.
16. Valige ripploendist soovitud helifaili, helifaili.
17. Klõpsake nuppu **Loo**. Sõnumi kinnitab, et olete loonud kõnesõnumi.
![Pilveteenuse](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Azure'i Mitmikautentimise vahemällu

Vahemällu võimaldab teil määrata teatud aja jooksul nii, et edaspidised autentimise katsed õnnestub automaatselt. Seda kasutatakse peamiselt kui kohapealse süsteemide nagu VPN päringuid mitme kontrollimise ajal esimese kutse on pooleli. See võimaldab edaspidised taotlused õnnestub automaatselt, kui kasutaja on loodud kinnitamine on pooleli. Pange tähele, et vahemällu on mõeldud kasutamiseks sisselogimist Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Kui soovite vahemällu Azure'i Mitmikautentimise häälestamine

1.  Logige sisse http://azure.microsoft.com
2.  Liikuge kohta juhiseid selle lehe ülaosas MFA haldusportaali.
3.  Klõpsake haldusportaali Azure'i mitmekordne autentimine konfigureerimine jaotises vahemälu.
4.  Klõpsake lehe vahemällu Seadista uue vahemälu
5.  Valige vahemälu tüüp ja vahemälu sekundit. Klõpsake nuppu Loo.

<center>![Pilveteenuse](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Usaldusväärsete IP-d

Usaldusväärsed IP-d on mitmikautentimise, mis võimaldab hallatavate või ühendatud rentniku administraatorite võimalus mööduda mitmikautentimise kasutajatele, kellel on ettevõtte kohaliku sisevõrgu sisse logida. Funktsioonid on saadaval Azure AD rentnikud, mis on Azure AD Premium, ettevõtte mobiilsus või Azure'i Mitmikautentimise litsentse.


Azure AD rentniku tüüp| Usaldusväärsed IP saadaolevad suvandid
:------------- | :------------- |
Hallatavate|Teatud IP-aadresside vahemikud – administraatorid saavad määrata vahemik, IP-aadressid, kes pääsevad mitmikautentimise kasutajatele, kellel on ettevõtte sisevõrgu sisse logida.
Ühendatud|<li>Kõik Mikroneesia kasutajad – kõik väliskasutajad, kes on logida sisse kaudu organisatsiooni sees ei pea mitmikautentimise nõude välja AD FS-i abil.</li><li>Teatud IP-aadresside vahemikud – administraatorid saavad määrata vahemik, IP-aadressid, kes pääsevad mitmikautentimise kasutajatele, kellel on ettevõtte sisevõrgu sisse logida.

Saate vältida see toimib ainult kaudu ettevõtte sisevõrgu sees. Nii näiteks, kui valisite kõik väliskasutajad ja kasutaja kaudu väljaspool ettevõtte sise-ja selle kasutaja on abil mitmikautentimise isegi juhul, kui kasutaja esitab ka AD FS-i taotluste. Järgmises tabelis kirjeldatakse, kui mitme teguri autentimis- ja rakenduse paroolid on vaja oma corpnet ja sellest väljaspool teie corpnet, kui usaldusväärsed IP-d on lubatud.


|Usaldusväärsed lubatud IP-d| Usaldusväärsed IP keelatud
:------------- | :------------- | :------------- |
Kollased corpnet|Brauseri puhul, mitmikautentimise pole vajalik.|Mitmikautentimise nõutud brauseri puhul,
|Tavaline paroolide töö rikkaliku kliendi rakendused, kui kasutajal pole loodud mis tahes rakenduse paroolid. Rakenduse parooli loodud rakenduse paroolid on vaja.|Rikkaliku kliendi rakenduste rakenduse paroolid kohustuslikuks
Välise corpnet|Brauseri puhul, peab mitmikautentimise.|Brauseri puhul, peab mitmikautentimise.
|Rikkaliku kliendi rakenduste rakenduse paroolid kohustuslikuks.|Rikkaliku kliendi rakenduste rakenduse paroolid kohustuslikuks.

### <a name="to-enable-trusted-ips"></a>Lubamiseks usaldusväärsed IP-d

1. Logige sisse Azure klassikaline portaali.
2. Klõpsake vasakul nuppu Active Directory.
3. Jaotises, klõpsake Directory soovite häälestada usaldusväärsed IPsing kataloogi.
4. Kataloogi valitud, klõpsake nuppu Konfigureeri.
5. Klõpsake jaotises mitmikautentimise haldamine haldusteenuste sätete.
6. Valige lehel Teenusesätted jaotises usaldusväärsed IP-d, kas:

    - Päringutele ühendatud pärinevate minu sisevõrgu kasutajad – kõik ühendatud kasutajad, kellel on ettevõtte võrku sisse logida ei pea mitmikautentimise nõude välja AD FS-i abil.
    - Sisestage päringutele kindla lahtrivahemiku avaliku IP-IP-aadresside ruute abil CIDR märke. Näide: xxx.xxx.xxx.0/24 IP-aadresside vahemiku xxx.xxx.xxx.1 – xxx.xxx.xxx.254 või xxx.xxx.xxx.xxx/32 ühe IP-aadress. Saate sisestada kuni 50 IP-aadresside vahemikud.

7. Klõpsake nuppu Salvesta.
8. Kui värskendused on rakendatud, klõpsake nuppu Sule.



![Usaldusväärsete IP-d](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Rakenduse paroolid

Mõned rakendused, nagu Office 2010 või vanemad ja Apple Mail ei saa kasutada mitmikautentimise.  Nende rakenduste kasutamiseks peate kasutama "Rakenduse paroolid" asemel traditsiooniline parool.  Rakenduse parool rakendusel mööduda mitmikautentimise ja tööd jätkata.

>[AZURE.NOTE] Kaasaegne autentimine Office 2013 klientidele
>
> Office 2013 kliendid (sh Outlook) nüüd toetavad uus autentimise protokollid ja lubanud Mitmikautentimise tugi.  See tähendab, et kui lubatud, rakenduse paroolid ei ole vaja Office 2013 kliendi jaoks.  Lisateavet leiate teemast [Office 2013 kaasaegne autentimise avaliku eelvaate teatamine](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Mida peaksite teadma rakenduse paroolide kohta

Järgmine on oluline loetelu asju, mida peaks teadma rakenduse paroolid.

- Kasutajate võib olla mitu rakenduse paroolid, mis suurendab varastamine pindala. Kuna rakenduse paroolid on raske meeles pidada, võib see julgustada inimesi Kirjutage järgmine. See on soovitatav ja tuleks vältida, sest ainult üks tegur on vaja rakenduse parooliga sisse logima.
- Rakendused, mis vahemälu paroolid ja seda kasutada kohapealse stsenaariumide võib alustada rakenduse parool pole teada väljaspool ettevõtte ID-d ei ole. Näide on Exchange'i e-kirju, mis on kohapealse, kuid arhiivitud meil on pilves. Sama parool ei tööta.
- Tegelik parooli luuakse automaatselt ja kasutaja ei esitata. See on, kuna automaatselt loodud parool raskem mõelda leidmise ja on turvalisem.
- Praegu on piiratud 40 paroolid kasutaja kohta. Teil palutakse kustutamiseks üks oma rakenduse parooliga uue konto loomiseks.
- Kui kasutajakonto on lubatud mitmikautentimise, rakenduse paroole saab kasutada enamiku-brauseri kliendid, nagu Outlook kui ka Lync, kuid haldustoimingud ei saa teha, kasutades rakenduse paroolid-brauseri rakendusi nagu Windows PowerShelli kaudu isegi siis, kui kasutaja on konto haldus.  Veenduge, et olete teenuse konto PowerShelli skriptide käitamiseks keeruka parooli loomine ja luba selle konto jaoks mitmikautentimise.

>[AZURE.WARNING]  Rakenduse paroolid ei tööta hübriidkeskkonna, kus kliendid asutusesiseselt suhelda ja Pilveteenusest automaattuvastuse lõpp-punktid. Selle põhjuseks domeeni paroolid on nõutav autentida kohapealse ja rakenduse paroolid on nõutav pilvega autentida.


### <a name="naming-guidance-for-app-passwords"></a>Nime panemise juhised rakenduse paroolid
Soovitatav on, et rakenduse parooli nimed peaks kajastavad neid kasutatakse seade. Näiteks, kui teil on sülearvuti, mis sisaldab-brauseri rakendused, nt Outlook, Word ja Excel, vaid peate nimega süle ühe rakenduse parooli loomine ja selle rakenduse parooli kasutamine kõik need rakendused. Kuigi saate luua eraldi paroolid kõik need rakendused, seda ei ole soovitatav. Funktsiooni soovitada viis on kasutada ühe rakenduse parooli seadme kohta.


<center>![Pilveteenuse](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Ühendatud (SSO) rakenduse paroolid
Azure AD toetab domeeniliitu kohapealse Windows Server Active Directory Domain Services (AD DS). Kui teie ettevõte kasutab federated(SSO) Azure AD ja te ei kavatse kasutada Azure Mitmikautentimise, siis Järgnevalt on oluline teave, mis peaks olema teadlik rakenduse paroolid kasutamisel. See kehtib ainult federated(SSO) kliendid.

- Rakenduse parool on kinnitatud Azure AD ja seega mille liiklus möödub federation. Federation kasutatakse ainult aktiivselt rakenduse parooli häälestamisel.
- Federated(SSO) kasutajad, saame kunagi minge erinevalt passiivne voogu identiteedipakkuja (IdP). Organisatsiooni id on talletatud paroolid. Kui kasutaja lahkub ettevõtte, on seda teavet organisatsiooni id abil DirSync reaalajas jätkata. Konto keelata/kustutamine võib kuluda kuni kolm tundi sünkroonimiseks edasi lükata keelata/kustutamise rakenduse parooli ja Azure AD.
- Kohapealse kliendi juurdepääsu reguleerimine sätted on au rakenduse parooli abil
- Kohapealse Autentimiseta logimine / auditeerimine võimalus on saadaval rakenduse parooli
- Veel lõppkasutaja õppurite jaoks on vaja Microsoft Lync 2013 kliendi. Nõutav juhised leiate teemast rakenduse parool oma meilikonto parooli muuta.
- Teatud täpsemate arhitektuuri kujunduse võib olla nõutav, kasutades kombinatsiooni ettevõtte kasutajanime ja parooli ja rakenduse paroolid klientidega, olenevalt sellest, kus need autentida mitmikautentimise kasutamisel. Kohapealne infrastruktuur autentimiseks klientide puhul kasutaks mõnda ettevõtte kasutajanime ja parooliga. Azure AD autentimiseks klientide puhul kasutage rakenduse parool.

Oletagem näiteks, et teil on arhitektuur, mis koosneb järgmistest:

- Teil on oma kohapealne eksemplari Active Directory Azure AD ühendamine
- Kasutate Exchange Online'i
- Kasutate Lynci, mis on spetsiaalselt kohapealne
- Kasutate Azure Mitmikautentimise


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 Sellisel juhul peate tegema järgmist:

- Kui logida sisse Lynci, kasutage oma organisatsioonide kasutajanime ja parooliga.
- Kui proovite avada aadressiraamatu Outlooki klient, mis ühendab Exchange Online'i kaudu, kasutage rakenduse parooli.

### <a name="allowing-app-password-creation"></a>Võimaldab rakenduse parooli loomine
Vaikimisi kasutajad ei saa luua rakenduse paroolid.  See funktsioon peab olema lubatud.  Selleks et lubada kasutajatel võimalus luua rakenduse paroolid, tehke järgmist.

#### <a name="to-enable-users-to-create-app-passwords"></a>Et kasutajad saaksid rakenduse paroolide loomine



1. Azure'i klassikaline portaali sisse logida.
2. Klõpsake vasakul nuppu Active Directory.
3. Jaotises, klõpsake Directory kataloogi kasutaja soovite lubada.
4. Ülaosas nuppu kasutajad.
5. Klõpsake lehe allosas nuppu hallata mitut tegurit Auth.  
6. Mitmikautentimise lehe ülaservas nuppu sätted.
7. Veenduge, et valitud oleks raadionuppu Luba kasutajatel rakenduse paroolide sisselogimiseks-brauseri rakenduste loomine.


![Rakenduse paroolide loomine](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Rakenduse paroolide loomine
Kasutajad saavad luua rakenduse paroolid oma algse registreerimise käigus.  Need on esitatud registreerimise käigus, mis võimaldab neid nende loomiseks lõpus soovitud suvand.

Lisaks kasutajad saavad luua ka rakenduse paroolid hiljem, muutes nende sätete Azure'i portaalis Office 365 portaalis või

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Office 365 portaalis rakenduse paroolide loomine
--------------------------------------------------------------------------------


1. Office 365 portaali sisselogimine
2. Valige paremas ülanurgas sätted vidin
3. Vasakul, valige täiendavad turvalisus kinnitamine
4. Valige parempoolsel paanil **minu telefoninumbrid kasutada konto turvalisuse värskendamine**
5. Valige lehel proofup kohal rakenduse paroolid
6. Klõpsake nuppu **Loo**
7. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**
8. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.

<center>![Pilveteenuse](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Rakenduse paroolid Azure portaali loomine
--------------------------------------------------------------------------------
1. Azure'i klassikaline portaali sisse logida.
3. Ülaservas, paremklõpsake oma kasutajanimi ja valige täiendavad turvalisus kinnitamine.
5. Valige lehel proofup kohal rakenduse paroolid
6. Klõpsake nuppu **Loo**
7. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**
8. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.


![Rakenduse paroolid](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Kui teil on Office 365 või Azure tellimuse rakenduse paroolid loomiseks
--------------------------------------------------------------------------------
1. Logige sisse [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Valige kohal profiil.
3. Klõpsake oma kasutajanimi ja valige täiendavad turvalisus kinnitamine.
5. Valige lehel proofup kohal rakenduse paroolid
6. Klõpsake nuppu **Loo**
7. Sisestage parool rakenduse nimi ja klõpsake nuppu **edasi**
8. Rakenduse parool kopeerimine lõikelauale ja kleepige see oma rakenduse.

![Rakenduse paroolid](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Mitmikautentimise seadmete kasutajad usalda meeles pidada

Mitmikautentimise uuendatakse seadmed ja brauserid, et kasutajad usalda on tasuta funktsioon MFA kõigi kasutajate jaoks.  Saate anda kasutajatele võimalus by-pass MFA jaoks määramine mitu päeva pärast edukat sisselogimist MFA kasutamine. See võib parandada teie kasutajate jaoks kasutatavuse.

Kuna kasutajad on lubatud meeles pidada MFA usaldusväärsete seadmete jaoks, võib see funktsioon vähendada konto turvalisus. Konto turvalisuse tagamiseks peaksite taastamine Mitmikautentimise nende seadmete jaoks kas järgmistel juhtudel:

- Kui oma ettevõtte konto on saanud kahjustada
- Kui meeles peetud seade on kadunud või varastatud

> [AZURE.NOTE] See funktsioon on rakendada brauseri küpsiste vahemälu. See ei tööta, kui teie brauser ei toeta.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Kuidas lubada või keelata meeles mitmikautentimise

1. Azure'i klassikaline portaali sisse logida.
2. Klõpsake vasakul nuppu Active Directory.
3. Klõpsake Active Directory, pidage meeles Mitmikautentimise häälestamine seadmete jaoks soovite kataloogi.
4. Kataloogi valitud, klõpsake nuppu Konfigureeri.
5. Klõpsake jaotises mitmikautentimise haldamine haldusteenuste sätete.
6. Lehel sätted kasutaja seadme sätete haldamine jaotises Vali/Tühista **kasutajatel on usaldusväärne seadmetes mitmikautentimise meeles pidada**.
![Pidage meeles seadmed](./media/multi-factor-authentication-whats-next/remember.png)
8. Määrake päevade arv, mille soovite lubada peatamise arv. Vaikimisi on 14 päeva jooksul.
9. Klõpsake nuppu Salvesta.
10. Klõpsake nuppu Sule.


## <a name="selectable-verification-methods"></a>Valitav kontrollimise meetodid
Pilves ja kohapealse versiooni, saate valida, millised kontrollimise meetodid on teie kasutajate jaoks saadaval. Järgmises tabelis antakse lühiülevaade iga meetodi.

Kui teie kasutajad registreeruda nende kontod MFA jaoks, valige need oma eelistatud kinnitusmeetod välja lubatud võimalusi. Juhised nende registreerimise käigus käsitletakse [kaheastmelise kontrollimise minu konto häälestamine](multi-factor-authentication-end-user-first-time.md)

Meetod|Kirjeldus
:------------- | :------------- |
Helistage telefoni |  Paigutab automatiseeritud tavakõne telefoni autentimist. Kasutaja kõnele ja ajal # nii nagu klahvistiku autentida. See telefoninumber on sünkroonitud kohapealse Active Directory.
Tekstsõnumi telefoni | Saadab sõnumi teksti sisaldav kinnituskood kasutajale. Kas teksti sõnumi vastuse kinnituskood või sisestada kinnituskood rakendusse sisselogimiseks kasutajaliidese küsitakse kasutaja.
Mobiilirakenduse kaudu | Selles režiimis rakendusega Microsoft Authenticator takistab volitamata juurdepääsu kontode ja lõpetab praktilise. Seda tehakse tõuketeatised teate oma telefoni või seadme registreeritud abil. Teate üksnes vaadata, ja kui see on õigustatud koputate kinnitamine. Muul juhul võib nuppu Keela või valida keelamiseks ja aruande pettusega teatis. Pettusega teatisi aruandlusteenuste kohta teavet teemast Mitmikautentimise Keela ja aruande pettuse funktsioonide kasutamiseks.</br></br>Rakendusega Microsoft Authenticator on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073)jaoks.|
Kinnituskood mobiilirakenduse kaudu | Selles režiimis saab luua ka VANDE kinnituskood on tarkvara märgiks rakendusega Microsoft Authenticator. See kinnituskood saab sisestada siis koos kasutajanime ja parooli, sisestage teine autentimist kujul.</li><br><p> Rakendusega Microsoft Authenticator on saadaval [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Androidi](http://go.microsoft.com/fwlink/?Linkid=825072)ja [iOS-i](http://go.microsoft.com/fwlink/?Linkid=825073)jaoks.

### <a name="how-to-enabledisable-authentication-methods"></a>Kuidas lubada või keelata autentimismeetodite

1. Azure'i klassikaline portaali sisse logida.
2. Klõpsake vasakul nuppu Active Directory.
3. Klõpsake jaotises Active Directory, te soovite lubada või keelata autentimismeetodite kataloogi.
4. Kataloogi valitud, klõpsake nuppu Konfigureeri.
5. Klõpsake jaotises mitmikautentimise haldamine haldusteenuste sätete.
6. Lehel Teenusesätted kinnitamise suvandid, klõpsake jaotises Vali/Tühista suvandid, mida soovite kasutada.</br></br>
![Kinnitamise suvandid](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Klõpsake nuppu Salvesta.
10. Klõpsake nuppu Sule.
