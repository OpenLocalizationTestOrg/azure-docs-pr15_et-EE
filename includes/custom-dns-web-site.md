#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Kohandatud domeeni nimi Azure veebisait konfigureerimine

Veebisaidi loomisel Azure pakub sõbralik alamdomeeni azurewebsites.net domeenis nii, et teie kasutajad pääsevad teie veebisaidile, nt http:// URL-i&lt;minu sait >. azurewebsites.net. Juhul, kui konfigureerite ühiskasutuses või standardrežiimis veebisaidiga, saate vastendada veebisaidi oma domeeni nimega.

Soovi korral saate Azure'i liikluse haldur laadimiseks saldo liiklust veebisaidile. Liikluse haldur veebisaitidega toimimise kohta lisateabe saamiseks vt [Juhtimine Azure veebisaitide liikluse Azure'i liikluse haldur][trafficmanager].

> [AZURE.NOTE] Selles ülesandes toiminguid rakendamine Azure veebisaitide; pilveteenustega, vt <a href="/develop/net/common-tasks/custom-dns/">kohandatud domeeninime Azure konfigureerimine</a>.

> [AZURE.NOTE] Juhised selle toimingu jaoks on vaja konfigureerida veebisaidiga ühiskasutuses või standardrežiimis, mis võivad muutuda, kui palju on arve tellimuse. Lisateabe saamiseks vaadake <a href="/pricing/details/web-sites/">Veebisaitide hinnad üksikasjad</a> .

Selle artikli teemad

-   [A- ja CNAME-kirjeid mõistmine](#understanding-records)
-   [Veebisaitide ühiskasutusega või standard režiimi konfigureerimine](#bkmk_configsharedmode)
-   [Lisage veebisaitide liikluse haldur](#trafficmanager)
-   [Lisada ka oma kohandatud domeeni CNAME](#bkmk_configurecname)
-   [Kohandatud domeeni A-kirje lisamine](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>A- ja CNAME-kirjeid mõistmine</h2>

CNAME (või alias (pseudonüüm) kirjete) ja kirjeid nii võimaldab teil domeeninime seostamine veebisait, kuid need iga toimida teisiti.

###<a name="cname-or-alias-record"></a>Alias (pseudonüüm)- või CNAME-kirje

CNAME-kirje kaardid *eraldi* domeen, nt **contoso.com** või **www.contoso.com**, kanoonilise domeeni nimi. Sel juhul kanoonilise domeeninimi on selle, kas selle ** &lt;myapp >. azurewebsites.net** domeeninime Azure veebisaidi või ** &lt;myapp >. trafficmgr.com** domeeninimi halduri liikluse profiili. Kui loodud, loob CNAME alias on ** &lt;myapp >. azurewebsites.net** või ** &lt;myapp >. trafficmgr.com** domeeni nimi. CNAME-kirje on IP-aadressi lahendamiseks oma ** &lt;myapp >. azurewebsites.net** või ** &lt;myapp >. trafficmgr.com** domeeninime automaatselt, et kui veebisaidi IP-aadress muutub, saate ei pea midagi teha.

> [AZURE.NOTE] Mõned domeeniregistraatori võimaldavad ainult vastendamine alamdomeene kasutamisel CNAME-kirje (nt www.contoso.com) ja mitte juurkausta nimed, nt contoso.com. CNAME-kirje kohta lisateabe saamiseks dokumentatsioonist oma domeeniregistraatori, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia kirje CNAME-kirje</a>või dokumendi <a href="http://tools.ietf.org/html/rfc1035">IETF domeeninimede - rakendamise ja määratlus</a> .

###<a name="a-record"></a>Kirje

A-kirje kaartide domeeni, nt **contoso.com** või **www.contoso.com**, *või metamärkide domeeni* nagu ** \*. contoso.com**, IP-aadressi. Azure'i veebisait puhul teenuse virtuaalse IP- või teatud IP aadress mille ostnud oma veebisaidi jaoks. Nii, et peamine kasu A-kirje üle CNAME-kirje on, et teil on üks kirje, mis kasutab metamärke, näiteks * **. contoso.com**, mis käsitleks taotlused mitme alamdomeenide nagu * *mail.contoso.com**; * *login.contoso.com**, või * *www.contso.com**.

> [AZURE.NOTE] Kuna A-kirje on vastendatud staatiline IP-aadress, ei saa automaatselt lahendada muudatused veebisaidi IP-aadress. IP-aadress soovitud kirjete jaoks on esitatud kohandatud domeeni nime sätete konfigureerimisel veebisaidi; siiski seda väärtust võivad muutuda, kui kustutamine ja looge oma veebisaidi või muuta veebisaidi režiimi vabastamiseks tagasi.

> [AZURE.NOTE] Laadi tasakaalustamiseks liikluse haldur ei saa kasutada soovitud kirjeid. Lisateabe saamiseks lugege teemat [Juhtimine Azure veebisaitide liikluse Azure'i liikluse haldur][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Veebisaidiga ühiskasutusega või standard režiimi konfigureerimine</h2>

Säte kohandatud domeeninime, veebisaidil on saadaval ainult Standard režiimid Azure veebisaitide ja ühiskasutuses. Enne veebisaidi tasuta veebisaidi režiimi ühiskasutuses või veebisait režiimi vahetada, peate esmalt eemaldama kulutuste suurtähelukk kohas tellimuse veebisaidi. Ühiskasutuses ja Standard hinnad režiimi kohta leiate lisateavet teemast [Hinnad üksikasjad][PricingDetails].

1. Avage brauseris [Haldusportaali][portal].
2. Klõpsake vahekaarti **veebisaitide** saidi nimi.

    ![][standardmode1]

3. Klõpsake vahekaarti **skaala** .

    ![][standardmode2]


4. Määrake jaotises **üldist** , klõpsates nuppu **ÜHISKASUTUSES**veebisaidi režiimi.

    ![][standardmode3]

    > [AZURE.NOTE] Kui kasutate liikluse haldur selle veebilehel, peate kasutama valige standardrežiimis asemel ühiskasutuses.

5. Klõpsake nuppu **Salvesta**.
6. Maksumus jagatud režiimi (või kui valite Standard standardrežiimis) suurendada küsimisel nuppu **Jah** , kui nõustute.

    <!--![][standardmode4]-->

    **Märkus**<br />
   Kui kuvatakse tõrketeade "Seadistamine skaala veebisaidi veebisaidi nime nurjus", saate lisateabe saamiseks nuppu üksikasjad.

<a name="trafficmanager"></a><h2>(Valikuline) Lisage oma veebisaitide liikluse haldur</h2>

Kui soovite kasutada oma veebisaidi liikluse haldur, tehke järgmist.

1. Kui teil on juba liikluse haldur profiili, kasutage teavet loomine [liikluse haldur profiili abil kiiresti luua] [ createprofile] loomist. Märkus on **. trafficmgr.com** teie haldur liikluse profiiliga seotud domeeninime. Seda kasutatakse hiljem.

2. Kasutage teabe [lisamine või kustutamine lõpp-punktid] [ addendpoint] lisamiseks veebisaidile lõpp profiili liikluse haldur.

    > [AZURE.NOTE] Kui lõpp lisamisel veebisaidi pole loendis, veenduge, et see on konfigureeritud standardrežiimis. Kasutage veebisaidi standardrežiimis liikluse haldur töötamiseks.

3. Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad.

4. Nüüd asub, kus saate valida või sisestada CNAME-kirje. Kui teil valida kirje tüüp: drop alla või avada täpsemate sätete lehe. Sõnade **CNAME**, **pseudonüümi**või **alamdomeene**peaks välja nägema.

5. Samuti peate sisestama CNAME-i domeeni või alamdomeeni pseudonüüm. Näiteks **www** kui soovite luua pseudonüümi **www.customdomain.com**.

5. Samuti peate sisestama hosti nimi, mis on selles CNAME alias kanoonilise domeeninimi. See on selle **. trafficmgr.com** oma veebisaidi nimi.

Näiteks järgmised CNAME-kirje edastab kogu liikluse kaudu **www.contoso.com** **contoso.trafficmgr.com**, veebisaidi domeeni nimi:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias (pseudonüüm) / hosti nimi/alamdomeeni</strong></td>
<td><strong>Kanoonilise domeeni</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

**Www.contoso.com** külastajale ei näe true host (contoso.azurewebsite.net), et edastamise protsess on nähtamatu lõppkasutaja.

> [AZURE.NOTE] Kui kasutate liikluse haldur veebisaidile, peate juhised leiate järgmistest jaotistest "**Lisa ka oma kohandatud domeeni CNAME**" ja "**Add A-kirje oma kohandatud domeeni**". CNAME-kirje, et eelmiste juhiste järgi loodud marsruutida liiklust-liikluse Manager, mis marsruudib liikluse siis veebisaidi endpoint(s).

<a name="bkmk_configurecname"></a><h2>Lisada ka oma kohandatud domeeni CNAME</h2>

CNAME-kirje loomiseks peate lisama uue kirje tabelis DNS-i oma kohandatud domeeni oma domeeniregistraatori tööriista abil. Iga domeeniregistraatori on sarnane, kuid veidi teistsugused meetod täpsustades CNAME-kirje, kuid mõisted on samad.

1. Üht järgmistest meetoditest otsimiseks kasutada funktsiooni **. azurewebsite.net** määratud veebisaidi domeeni nimi.

    * [Azure'i haldusportaal]login[portal], valige oma veebisaidile, valige **armatuurlaud**, ja seejärel otsige **Saidi URL-i** kirje jaotises **kiire ülevaade** .

    * Installida ja konfigureerida [Azure Powershell](/manage/install-and-configure-windows-powershell/)ja seejärel kasutage järgmine käsk:

            get-azurewebsite yoursitename | select hostnames

    * Installida ja konfigureerida [Azure käsurea liides](/manage/install-and-configure-cli/)ja seejärel kasutage järgmine käsk:

            azure site domain list yoursitename

    Salvestage see **. azurewebsite.net** nimi, kui seda kasutatakse järgmist.

3. Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad.

4. Nüüd asub, kus saate valida või sisestada CNAME-kirje. Kui teil valida kirje tüüp: drop alla või avada täpsemate sätete lehe. Sõnade **CNAME**, **pseudonüümi**või **alamdomeene**peaks välja nägema.

5. Samuti peate sisestama CNAME-i domeeni või alamdomeeni pseudonüüm. Näiteks **www** kui soovite luua pseudonüümi **www.customdomain.com**. Kui soovite luua pseudonüümi, et juurdomeeni, võib olla loendis selle funktsiooni '**@**' oma domeeniregistraatori DNS-i tööriistade sümbol.

5. Samuti peate sisestama hosti nimi, mis on selles CNAME alias kanoonilise domeeninimi. See on selle **. azurewebsite.net** oma veebisaidi nimi.

Näiteks järgmised CNAME-kirje edastab kogu liikluse kaudu **www.contoso.com** **contoso.azurewebsite.net**, veebisaidi domeeni nimi:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias (pseudonüüm) / hosti nimi/alamdomeeni</strong></td>
<td><strong>Kanoonilise domeeni</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.azurewebsite.net</td>
</tr>
</table>

**Www.contoso.com** külastajale ei näe true host (contoso.azurewebsite.net), et edastamise protsess on nähtamatu lõppkasutaja.

> [AZURE.NOTE] Ülaltoodud näites kehtib ainult liikluse alamdomeen __www__ . Kuna te ei saa kasutada metamärke CNAME-kirjet, peate looma ühe CNAME iga domeeni ja alamdomeeni jaoks. Kui soovite, mis suunaks liikluse aadressilt alamdomeene, näiteks *. contoso.com, aadressile azurewebsite.net saate konfigureerida __URL-i ümber suunata__ või __Edasi URL-i__ kirje oma DNS-i sätete või A-kirje loomine.

> [AZURE.NOTE] See võib võtta aega jaoks oma DNS-i süsteemis levitada CNAME. CNAME-i veebisaidi jaoks ei saa määrata, kuni on levitatud CNAME-i. Näiteks <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> teenuse abil saate CNAME-i saadavuse kontrollimiseks.

###<a name="add-the-domain-name-to-your-website"></a>Domeeninime lisamine veebisaidile

Pärast CNAME-kirje domeeni jaoks on levitatud, tuleb see seostada veebisaidi. Saate lisada kohandatud domeeni nimi, mis on määratletud CNAME-kirjet oma veebisaidile, kasutades kas Azure'i käsurea liides (Azure'i CLI) või Azure haldusportaali abil.

**Käsurea tööriistade abil domeeninime lisamine**

Installida ja konfigureerida [Azure käsurea liides](/manage/install-and-configure-cli/)ja seejärel kasutage järgmine käsk:

    azure site domain add customdomain yoursitename

Näiteks järgmine lisab kohandatud domeeninime **www.contoso.com** **contoso.azurewebsite.net** veebisaidi:

    azure site domain add www.contoso.com contoso

Saate kinnitada, et kohandatud domeeninime lisati veebisaidile, kasutades järgmine käsk:

    azure site domain list yoursitename

See käsk tagastatud loend peaks sisaldama kohandatud domeeninime, samuti vaikimisi **. azurewebsite.net** kirje.

**Azure haldusportaali abil domeeninime lisamine**

1. Avage brauseris [Azure'i haldusportaal][portal].

2. **Veebisaitide** vahekaardil, klõpsake saidi nime, valige **armatuurlaud**ja valige lehe allosas **Domeenide haldamine** .

    ![][setcname2]

6. Tippige tekstiväljale **DOMAIN NAMES** domeeninimi, mille olete konfigureerinud.

    ![][setcname3]

6. Klõpsake märkeruutu domeeni nimi.

Kui konfigureerimine on lõppenud, loetletakse **domeeninimede** osas veebisaidi lehe **konfigureerimine** kohandatud domeeni nimi.

<a name="bkmk_configurearecord"></a><h2>Kohandatud domeeni A-kirje lisamine</h2>

A-kirje loomiseks peate esmalt leida oma veebisaidi IP-aadress. Klõpsake kirje lisamine oma domeeni DNS-i tabelis esitatud oma domeeniregistraatori tööriistade abil. Iga domeeniregistraatori on sarnane, kuid veidi teistsugused meetodit, A-kirje, kuid mõisted on samad. Lisaks A-kirje loomist peate looma ka A-kirje kinnitamaks kasutava Azure'i CNAME-kirje.

1. Avage brauseris [Azure'i haldusportaal][portal].

2. **Veebisaitide** vahekaardil, klõpsake saidi nime, valige **armatuurlaud**ja valige ekraani allservast ülespoole **Domeenide haldamine** .

    ![][setcname2]

5. Otsige dialoogiboksis **kohandatud domeenide haldamine** **Kasutada kirjete konfigureerimisel The IP-aadress**. Kopeerige IP-aadress. Seda kasutatakse A-kirje loomisel.

5. Klõpsake dialoogiboksis **kohandatud domeenide haldamine** Märkus awverify domeeninime dialoogiboksi ülaosas teksti lõpus. See peaks olema **awverify.mysite.azurewebsites.net** , kus **saidi minu sait** on oma veebisaidi nimi. Kopeerida see, kui see on kontrollimise CNAME-kirje loomisel kasutada domeeni nimi.

6. Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad.

6. Kus saate valida või sisestada A ja CNAME-kirjete otsimine Kui teil valida kirje tüüp: drop alla või avada täpsemate sätete lehe.

7. Järgmiste toimingute A-kirje loomiseks:

    1. Valige või sisestage domeeni või alamdomeeni, mida kasutatakse A-kirje. Näiteks valige **www-d** , kui soovite luua pseudonüümi **www.customdomain.com**. Kui soovite kõik alamdomeenide metamärkide kirje loomine, sisestage "__*__". See hõlmab kõiki alamdomeenide, nt **mail.customdomain.com**, **login.customdomain.com**ja **www.customdomain.com**.

        Kui soovite, et juurdomeeni A-kirje loomiseks, selle võib loetleda soovitud '**@**' oma domeeniregistraatori DNS-i tööriistade sümbol.

    2. Sisestage esitatud väljale oma pilveteenuses IP-aadress. See seostab domeeni kirjet kasutatakse A-kirje oma pilvepõhise teenuse juurutuse IP-aadressiga.

        Näiteks järgmine kirje edastab kogu liikluse **contoso.com** **137.135.70.239**, meie juurutatud rakendusega IP-aadress:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Hosti nimi/alamdomeeni</strong></td>
        <td><strong>IP-aadress</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        Selles näites näitab, et juurdomeeni A-kirje loomine. Kui soovite kõik alamdomeene katmiseks metamärkide kirje loomine, sisestage '__*__' alamdomeen nimega.

7. Järgmiseks luua pseudonüümi **awverify**ja **awverify.mysite.azurewebsites.net** , mida te saada varem kanoonilise domeeni CNAME-kirje.

    > [AZURE.NOTE] Kuigi awverify pseudonüümi pruugi mõned registraatorit, teised võivad nõuda awverify.www.customdomainname.com või awverify.customdomainname.com pseudonüüm täielik domeeninimi.

    Näiteks järgmine loob CNAME-kirje, mis Azure'i abil saate kontrollida A-kirje konfigureerimine.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias (pseudonüüm) / hosti nimi/alamdomeeni</strong></td>
    <td><strong>Kanoonilise domeeni</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.net</td>
    </tr>
    </table>

> [AZURE.NOTE] See võib võtta aega awverify CNAME DNS-i süsteemis levitamise kohta. Te ei saa määrata kuni awverify CNAME on levitatud A-kirje veebisaidi jaoks määratletud kohandatud domeeni nime. Näiteks <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> teenuse abil saate CNAME-i saadavuse kontrollimiseks.

###<a name="add-the-domain-name-to-your-website"></a>Domeeninime lisamine veebisaidile

Pärast domeeninime **awverify** CNAME-kirje on levitatud, siis saate kohandatud domeeninime, mis on määratletud A-kirje veebisaidiga seostada. Saate lisada kohandatud domeeni nimi, mis on määratletud A-kirje veebisaidile CLI Azure'i abil või Azure haldusportaali abil.

**Lisada domeeni nimi, kasutades Azure käsurea liides (Azure'i CLI)**

Installida ja konfigureerida [Azure CLI](/manage/install-and-configure-cli/), ja seejärel kasutage järgmine käsk:

    azure site domain add customdomain yoursitename

Näiteks järgmine lisab kohandatud domeeninime **contoso.com** **contoso.azurewebsite.net** veebisaidi:

    azure site domain add contoso.com contoso

Saate kinnitada, et kohandatud domeeninime lisati veebisaidile, kasutades järgmine käsk:

    azure site domain list yoursitename

See käsk tagastatud loend peaks sisaldama kohandatud domeeninime, samuti vaikimisi **. azurewebsite.net** kirje.

**Azure haldusportaali abil domeeninime lisamine**

1. Avage brauseris [Azure'i haldusportaal][portal].

2. **Veebisaitide** vahekaardil, klõpsake saidi nime, valige **armatuurlaud**ja valige lehe allosas **Domeenide haldamine** .

    ![][setcname2]

6. Tippige tekstiväljale **DOMAIN NAMES** domeeninimi, mille olete konfigureerinud.

    ![][setcname3]

6. Klõpsake märkeruutu domeeni nimi.

Kui konfigureerimine on lõppenud, loetletakse **domeeninimede** osas veebisaidi lehe **konfigureerimine** kohandatud domeeni nimi.

> [AZURE.NOTE] Kui olete lisanud domeeni nimi, mis on määratletud A-kirje oma veebisaidile, saate eemaldada awverify CNAME-kirje oma domeeniregistraatori esitatud tööriistade abil. Juhul, kui soovite lisada mõne muu A kirje, on teil selle awverify uuesti luua kirje enne uue domeeni nime saate seostada määratletud uue kirje veebisaidil.

## <a name="next-steps"></a>Järgmised sammud

-   [Kuidas veebisaitide haldamine](/manage/services/web-sites/how-to-manage-websites/)

-   [Veebisaitide jaoks SSL-serdi konfigureerimine](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
