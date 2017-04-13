<properties
   pageTitle="Azure'i andmekataloogi korduma kippuvad küsimused | Microsoft Azure'i"
   description="Korduma kippuvad küsimused Azure'i andmekataloogi, sh andmetuvastus allikas, marginaalide lisamise ja haldamise võimalusi."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure'i andmekataloogi korduma kippuvad küsimused

Sellest artiklist leiate vastused korduma kippuvatele küsimustele Microsoft **Azure'i andmekataloogi** teenusega seotud.

## <a name="q-what-is-azure-data-catalog"></a>Q: mis on Azure andmekataloogi?

V: Microsoft Azure'i andmekataloogi on täielikult hallatav teenus Microsoft Azure'i pilves, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteemi majutatud. Azure'i andmekataloogi pakub võimalusi, mis võimaldavad mõni kasutaja – ärianalüütikud andmeteadlaste arendajatele – registreerida, et tuvastada, mõista ja tarbimine andmeallikad.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>Q: millised kliendi probleemid ei Azure'i andmekataloogi lahendada?

Azure'i andmekataloogi aadressid, mis võimaldab kasutajatel mõista enterprise andmeallikate leidmiseks ja allika andmetuvastus ja "tume andmed".

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>Q: kellel on sihtrühmade jaoks Azure'i andmekataloogi?

Azure'i andmekataloogi pakub tehniline ja mittetehnilistest kasutajatele, sealhulgas:

- Andmete arendajate, BI ja Analytics asjatundjatele: eest toodavad andmed ja analüüsi sisu teistele kasutamine
-   Andmete korrapidajate: Neile, kes on teadmisi, andmed, mida see tähendab? ja kuidas see on mõeldud kasutamiseks ja milleks
- Andmete tarbijad: Neile, kes peavad saama kerge vaevaga tuvastada, mõista ja andmetega ühenduse loomine vaja oma tööd oma valitud tööriista abil teha
- Keskse see: neile, kes vajavad sadu andmeallikate hõlpsamaks ettevõtteversiooni kasutajaid ja kellel on vaja kasutada järelevalve üle kuidas kasutatakse andmete ja kes

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>Q: mis on Azure andmekataloogi piirkonna-saadavus?

Azure'i andmekataloogi teenuseid pole praegu saadaval järgmised andmekeskuste:

- Lääne USA.
- Ida-USA
- Lääne Europe
- Põhja-Europe
- Austraalia Ida
- Kagu-Aasia

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>Q: millised piirangud andmete varasid Azure'i andmekataloogi arv?

Azure'i andmete kataloogi tasuta väljaande on piiratud 5000 registreeritud andmete varad.

Azure'i andmete kataloogi Standard Edition toetab kuni 100 000 registreeritud andmete varad.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>Q: millised lähte- ja varade toetatud andmetüübid?

Vaadake [Andmete kataloogi DSR](data-catalog-dsr.md) praegu toetatud andmeallikate loendi.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>Q: Kuidas taotleda, mõne muu andmeallika tugi?

Soovitada funktsioone ja muid tähelepanekuid saab esitada [Azure'i andmekataloogi Foorum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>Q: Kuidas alustada Azure'i andmekataloogi?

Parim koht alustamiseks on [Andmekataloogis andmetega töötamise alustamine](data-catalog-get-started.md)juhiseid järgides. Selles artiklis on teenuses võimaluste ülevaade lõpuni.

## <a name="q-how-do-i-register-my-data"></a>Q: Kuidas registreeruda minu andmeid?

Azure'i andmekataloogis andmete registreerimiseks käivitage tööriista Azure'i andmekataloogi registreerimise "Avalda" ala Azure'i andmekataloogi portaali kaudu. Azure'i andmekataloogi avaldamise rakenduse, logige sisse sama mandaadi abil saate portaali Azure'i andmekataloogi abil ja valige andmeallika ja soovite registreerida teatud varasid.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>Q: millised atribuudid ekstraktitakse andmete varade, mis on registreeritud?

Teatud atribuudid erinevad andmeallika andmeallikas, kuid üldiselt Azure'i andmekataloogi avaldamise teenuse väljavõte järgmine teave:

- Vara nimi
- Varade tüüp
- Varade kirjeldus
- Atribuut/veergude nimed
- Atribuut/veeru andmetüübid
- Atribuut/veeru kirjeldus

> [AZURE.IMPORTANT] Andmete varad registreerumist Azure'i andmekataloogi teisaldamine või kopeerimine andmete pilve. Varade andmeallika registreerimisel kopeerida varade metaandmete Azure, kuid andmed jäävad olemasoleva andmeallika asukoht. Selle reegli erandiks on, kui kasutaja valib eelvaade kirjeid või profiili andmete üleslaadimiseks varad registreerimisel. Kui kuni 20 kirjete eelvaade, sh kopeeritakse iga vara ja Azure andmekataloogis hetktõmmisena talletatakse. Kui sh andmete profiili, arvutatud liitväärtuse teabe (nt protsent tühjad väärtused veerus tabelite ja veergude väärtused miinimum, maksimum ja Keskmine suurus) ja kaasatud talletatud kataloogi metaandmed.

<br/>

> [AZURE.NOTE] Andmeallikate nagu SQL Server Analysis Services esimese klassi **Kirjeldus** atribuudi avaldamise rakenduse Azure'i andmekataloogi väljavõte selle atribuudi väärtust. Jaoks SQL serveri relatsiooniandmebaasid, mis ei ole esimese klassi **Kirjeldus** atribuut, Azure'i andmekataloogi avaldamise rakenduse väljavõte väärtus ms_description, laiendatud atribuudi objektid ja veerge. Lisateavet leiate teemast TechNeti [Laiendatud atribuutide kasutamine andmebaasiobjektide](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>K: kaua tuleks kulub äsja registreeritud vara Azure'i andmekataloogi kuvada?

Pärast varade registreerida Azure'i andmekataloogi võib 5 – 10 sekundit enne need kuvataks Azure'i andmekataloogi portaalis.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Q: kuidas marginaalida ja rikastamine minu registreeritud andmete varade metaandmeid?

Lihtsaim viis anda metaandmete registreeritud vara on vara valimine Azure'i andmekataloogi portaalis ja sisestage metaandmete väärtused paanil atribuudid või skeemi paanil valitud objekti.

Mõned metaandmed, näiteks asjatundjate ja sildid, saate sisestada ka registreerimise käigus. Avaldamine teenuses Azure andmekataloogi antud väärtused rakenduvad kogu vara sel ajal registreeritud. Viimati registreeritud objektide jaoks täiendavate marginaali portaalis vaatamiseks nuppu **Kuvamine portaalis** Azure andmekataloogi avaldamise taotluse lõpliku ekraanil.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>Q: kuidas minu registreeritud andmeobjektid kustutada?

Saate kustutada objekti Azure'i andmekataloogi, valides objekti portaalis, ja seejärel klõpsake nuppu **Kustuta** . Seda objekti metaandmete eemaldamine Azure'i andmekataloogi, kuid tegelik aluseks oleva andmeallika ei mõjuta.

## <a name="q-what-is-an-expert"></a>Q: mis on ekspert?

Ekspert on isik, kes on ka kursis püsida perspektiivi andmete objekti kohta. Objekti võib olla mitu eksperdid. Ekspert ei pea olema "omanik" objekti; ekspert on lihtsalt keegi, kes teab, kuidas andmeid saab ja tuleks kasutada.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>Q: kuidas jagada teavet Azure'i andmekataloogi meeskond kui mul on probleeme?

Kasutage Azure'i andmekataloogi Foorum probleemidest, teabe jagamiseks ja esitada küsimusi. Http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409 leiate Foorum

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>Q: kas Azure andmekataloogi töötada muude andmeallikale huvitab?
Töötame Azure'i andmekataloogi rohkemate andmeallikatega lisamise kohta. Kui seal on andmeallika, mida soovite vaadata toetatud, palun soovita seda (või teie tugi kõneposti, kui see on juba soovitatud) [Azure'i andmekataloogi Foorum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Q: kuidas Azure'i andmekataloogi seotud andmekataloogi Power BI for Office 365?

Mõelge Azure'i andmekataloogi nimega andmekataloogi areng. Azure'i andmekataloogi pakub sarnaseid võimalusi andmete allikas avaldamine ja discovery, kuid on tähelepanu laiema stsenaariumid ja ei sõltu Office 365. Varsti pärast Azure andmekataloogi muutub üldiselt kättesaadav kaks kataloogid ühendamine ühte teenust.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>Q: milliseid õigusi vajab kasutaja Azure'i andmekataloogi varad registreeruda?

Azure'i andmekataloogi registreerimise tööriista kasutaja peab andmeallikas, mis võimaldab tal lugeda metaandmete lähtekoha õigused. Kui kasutaja valib ka eelvaateks, siis kasutaja olema ka õigused, mis võimaldab tal lugeda, andmeid registreeritud objektid.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Q: Azure andmekataloogi tehakse kättesaadavaks ka kohapealse juurutuse jaoks?

Azure'i andmekataloogi on pilveteenuses, kus saate töötada nii pilve ja kohapealsete andmeallikate hübriid andmete allikas discovery lahenduse pakkuda. Praegu ei ole kavas kohapealse töötavad Azure andmekataloogi teenuse versioon.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>Q: Kas me rohkem / rikkalikumat metaandmete registreerib andmeallikatest ekstrakti?

Töötame Azure'i andmekataloogi võimaluste laiendamiseks. Täiendavad metaandmed, mida soovite näha ekstraktitud andmeallikast registreerimise käigus korral palun näitavad selle (või hääletada seda, kui see on juba soovitatud) [Azure'i andmekataloogi Foorum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Tulevikus võimaldab meil muude tootjate lisada uue andmeallika tüübiga on laiendatavuse API kaudu.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Q: Kuidas piirata nähtavuse registreeritud andmete varade, et ainult teatud inimesed saaksid neid tuvastada?

V: valida Azure'i andmekataloogis andmetega, ja klõpsake nuppu "Võtta omandi". Azure'i andmekataloogis andmetega varade omanikud saate muuta nähtavuse sätteid, kas kõigil kasutajatel kataloogi avastamine kuuluv varad või piiratud nähtavus kindlatele kasutajatele.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Q: Kuidas värskendada andmeid vara registreerimine, mis muudab andmeallika kajastuvad kataloogi?

V: andmete varad, mis on juba registreeritud kataloogi metaandmed värskendamiseks lihtsalt uuesti registreerida vara sisaldavast andmeallikast. Muudatused andmeallikas, nagu näiteks veerud on lisada või eemaldada tabelite või vaadete, värskendatakse ka kataloogi, kuid säilitatakse kõik kasutajate marginaalid.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>K: pole küsimusele siin – mida teha?

Pea üle [Azure'i andmekataloogi Foorum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Küsimustele seal leiavad tee siin.
