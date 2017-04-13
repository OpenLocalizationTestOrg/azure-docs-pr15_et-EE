<properties
    pageTitle="Azure'i BizTalki teenuste Azure portaali loomine | Microsoft Azure'i"
    description="Saate teada, kuidas ettevalmistamise või looge Azure BizTalki teenuste Azure portaali; MABS WABS"
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Azure'i portaalis BizTalki teenuste loomine

Azure'i BizTalki teenuste Azure portaali loomine

> [AZURE.TIP] Azure'i portaali sisse logida, peate Azure'i konto ja Azure tellimust. Kui teil pole kontot, saate luua tasuta prooviversiooni konto mõne minuti jooksul. Leiate [Azure'i tasuta prooviversioon](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>BizTalki teenuse loomine
Sõltuvalt väljaande valida, mitte kõik BizTalki Teenusesätted võib olla saadaval.

1. [Azure'i portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige navigeerimispaanil all **Uus**.  
![Valige nupp uus][NEWButton]

3. Valige **rakenduse teenused** > **BIZTALKI teenuse** > **kohandatud loomine**:  
![Valige BizTalki teenuse ja kohandatud loomine][NewBizTalkService]

4. Sisestage BizTalki Teenusesätted.

    <table border="1">
    <tr>
    <td><strong>BizTalki teenuse nimi</strong></td>
    <td>Saate sisestada suvalise nime, kuid olge täpne. Mõned näited:<br/><br/>
    <em>MyCompany</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" lisatakse automaatselt teie sisestatud nimi. See loob URL, mis kasutatakse juurdepääsuks BizTalki teenust, nt <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Väljaande</strong></td>
    <td>Kui teil on testimine arengu etapp, valige <strong>arendaja</strong>. Kui olete valides, kasutage funktsiooni <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalki teenuste: väljaanded diagrammi</a> kas <strong>Premium</strong>, <strong>Standard</strong>või <strong>lihtsa</strong> on õige valik oma äri stsenaariumi.
    </td>
    </tr>
    <tr>
    <td><strong>Piirkond</strong></td>
    <td>Valige asukohaandmete piirkond BizTalki teenust majutada.</td>
    </tr>
    <tr>
    <td><strong>Domeeni URL</strong></td>
    <td><strong>Valikuline</strong>. Vaikimisi on domeeni URL-i <em>YourBizTalkServiceName</em>. biztalk.windows.net. Saate sisestada ka kohandatud domeeni. Näiteks kui teie domeen on <em>contoso</em>, saate sisestada: <br/><br/>
    <em>MyCompany</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Valige järgmine noolt.

5. Sisestage talletamist ja andmebaasi sätted:

    <table border="1">
    <tr>
    <td><strong>Jälgimine arhiivimise salvestusruumi konto</strong></td>
    <td>Valige salvestusruumi konto või salvestusruumi uue konto loomine. <br/><br/>Kui loote uue salvestusruumi konto, sisestage <strong>Salvestusruumikonto nimi</strong>.</td>
    </tr>
    <tr>
    <td><strong>Andmebaasi jälgimine</strong></td>
    <td>Kui kasutate olemasoleva SQL Azure'i andmebaasi, ei saa kasutada mõnes muus BizTalki teenuses. Vajaliku kasutajanime ja parooli sisestamise kui Azure SQL-andmebaasi serveri loodud.<br/><br/><strong>Näpunäide.</strong> Piirkonna BizTalki teenuste jälitus andmebaas ja jälgimis/arhiivimise salvestusruumi konto loomine</td>
    </tr>
    </table>
Valige järgmine noolt.

6. Sisestage andmebaasi sätted:

    <table border="1">
    <tr>
    <td><strong>Nimi</strong></td>
    <td><strong>Loo uus eksemplar SQL-andmebaasi</strong> valimisel eelmisele kuvale saadaval.
    <br/><br/>
   Sisestage kasutatav BizTalki teenust SQL-andmebaasi nimi.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td><strong>Loo uus eksemplar SQL-andmebaasi</strong> valimisel eelmisele kuvale saadaval.
    <br/><br/>
   Valige olemasoleva SQL serveri andmebaasi või looge uus SQL-andmebaasi server.</td>
    </tr>
    <tr>
    <td><strong>Serveri sisselogimisnimi</strong></td>
    <td>Sisestage kasutaja sisselogimisnimi.</td>
    </tr>
    <tr>
    <td><strong>Serveri sisselogimise parool</strong></td>
    <td>Sisestage sisselogimise parool.</td>
    </tr>
    <tr>
    <td><strong>Piirkond</strong></td>
    <td><strong>Loo uus eksemplar SQL-andmebaasi</strong> valimisel saadaval. Valige SQL-andmebaasi majutamiseks geograafilised piirkond.</td>
    </tr>
    </table>

Valige viisardi märke. Edenemise ikoon kuvatakse:  
![Edenemise ikoon kuvatakse, kui olete lõpetanud][ProgressComplete]

Kui olete lõpetanud, Azure'i BizTalki teenus on loodud ja olete valmis oma rakendusi. Vaikimisi on piisavalt. Kui soovite muuta sätteid, **BIZTALKI teenused** klõpsake vasakpoolsel navigeerimispaanil ja valige BizTalki teenust. Täiendavad sätted kuvatakse [vahekaardid armatuurlaud, jälgimine, ja skaala](biztalk-dashboard-monitor-scale-tabs.md) ülaosas.

Sõltuvalt teenuse BizTalki, on mõned toimingud, mis ei saa lõpetada. Nende toimingute loendit, minge [BizTalki teenuste oleku diagrammi](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Pärast ettevalmistamise järgmiselt.

-  [Kohalikus arvutis serdi installimine](#InstallCert)
-  [Tootmise valmis serdi lisamine](#AddCert)
-  [Saada nimeruumi juurdepääsu reguleerimine](#ACS)

#### <a name="InstallCert"></a>Kohalikus arvutis serdi installimine
BizTalki teenuse ettevalmistamise osana iseallkirjastatud serdi luuakse ja BizTalki teenuse tellimusega seostatud. Peate selle serdi allalaadimine ja installimine arvutisse, kus saate juurutada BizTalki teenuserakenduste või sõnumeid saata BizTalki teenuse lõpp-punkti.

1. [Azure'i portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige vasakpoolsel navigeerimispaanil **BIZTALKI teenuste** ja valige tellimuse BizTalki teenus.
3. Valige vahekaart **armatuurlaud** .
4. Valige **allalaadimine SSL-sert**.  
![SSL-serdi muutmine][QuickGlance]
5. Topeltklõpsake serti ja käivitage viisardiga serdi installimiseks. Veenduge, et installite all hoida **Trusted Root sertimiskeskuste** sert.

#### <a name="AddCert"></a>Tootmise valmis serdi lisamine
Luuakse automaatselt, kui loomise BizTalki teenused on mõeldud kasutamiseks arengu ainult iseallkirjastatud sert. Peamised, asendage see sertifikaadiga tootmise valmis.

1. Valige vahekaart **armatuurlaud** **Update SSL-sert**.
2. Otsige sirvides üles oma isiklike SSL-sert (*CertificateName*pfx), mis sisaldab teie BizTalki teenuse nimi, sisestage parool ja klõpsake märkeruutu.

#### <a name="ACS"></a>Saada nimeruumi juurdepääsu reguleerimine

1. [Azure'i portaali](http://go.microsoft.com/fwlink/p/?LinkID=213885)sisse logida.
2. Valige vasakpoolsel navigeerimispaanil **BIZTALKI teenuste** ja valige BizTalki teenust.
3. Valige tööülesande **Ühenduseteavet**.  
![Valige ühenduse teave][ACSConnectInfo]

4. Kopeerige väärtused juurdepääsu reguleerimine.

Visual Studio projekti BizTalki teenuse juurutamisel sisestate selle nimeruumi juurdepääsu reguleerimine. Juurdepääsu reguleerimine nimeruumi luuakse automaatselt teie BizTalki teenuse jaoks.

Juurdepääsu reguleerimine väärtusi saab kasutada mistahes rakendusega. Azure'i BizTalki teenuste loomisel selle juurdepääsu reguleerimine nimeruumi juhtelemendid autentimise juurutamise BizTalki teenuse abil. Kui soovite muuta tellimuse või hallata nimeruumi, valige Vasakpoolsel navigeerimispaanil **ACTIVE DIRECTORY** ja seejärel valige oma nimeruum. Tegumiribal asuv loetletud koosolekute korraldamiseks vajalikke suvandeid.

Klõpsake nuppu **Halda** avatakse haldusportaali juurdepääsu juhtimine. Juurdepääsu juhtimine haldusportaal BizTalki teenuse kasutab **teenuse identiteedid**.  
![Juurdepääs ACS teenuse identiteedid juhtida haldusportaal][ACSServiceIdentities]

Juurdepääsu reguleerimine teenus on kogumi rakenduste või kliendid otse kaudu juurdepääsu reguleerimine autentimiseks ja vastu võtta märgiks üksust.

> [AZURE.IMPORTANT]BizTalki teenuse kasutab **omanik** teenuse vaikeidentiteedi ja **parooli** väärtuse. Kui kasutate asemel parooli väärtus sümmeetriline võti väärtus, võib ilmneda järgmine tõrge.<br/><br/>*Ühendust ei saa määratud mandaadiga juurdepääsu juhtimine halduse teenuse konto*

[Hallata oma ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) on loetletud mõned juhised ja soovitused.

## <a name="requirements-explained"></a>Nõuete ülevaade

Nendele nõuetele ei kehti tasuta Edition.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Mida on vaja</strong></td>
        <td><strong>Miks seda vaja</strong></td>
</tr>
<tr>
<td>Azure'i tellimus</td>
<td>Tellimuse määratleb, kes saab Azure portaali sisse logida. Konto omanik loob <a HREF="https://account.windowsazure.com/Subscriptions">Azure'i tellimused</a>tellimust.
<br/><br/>
Azure'i konto võib olla mitu tellimust ja saate hallata igaüks, kellel on lubatud. Näiteks oma Azure'i konto omanik nimega <em>BizTalkServiceSubscription</em> tellimuse loob ja annab teie ettevõtte administraatorid BizTalki (nt ContosoBTSAdmins@live.com) juurdepääsu selle tellimuse. Selle stsenaariumi korral BizTalki administraatorid Azure portaali sisse logida ja on täielik administraatori õigused majutatud teenuste tellimus, sh Azure BizTalki teenused. BizTalki administraatorid ei ole Azure'i konto omanikele ja seetõttu ei pääse juurde mis tahes arveldusteabe.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Tellimuste haldamine ja salvestusruumi kontod Azure'i portaalis</a> annab Lisateavet.
</td>
</tr>
<tr>
<td>Azure'i SQL-andmebaas</td>
<td>Tabelite, vaadete ja salvestatud toimingute kasutavad BizTalki teenuse, sealhulgas jälgimise andmed talletatakse.
<br/><br/>
BizTalki teenuse loomisel saate kasutada mõne olemasoleva Azure SQL Server Azure'i SQL-andmebaasi või luua uue andmebaasi või serveri automaatselt.
<br/><br/>
SQL-andmebaasi skaala on konfigureeritud automaatselt. Tavaliselt on vaikimisi skaala BizTalki teenuse jaoks piisavad. Skaala muutmine mõjutab hinnad. Lugege teemat <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">kontod ja arveldamine Azure'i SQL-andmebaas</a>
<br/><br/>
<strong>Märkmete</strong>
<br/>
<ul>
<li> Kui loote uue Azure SQL Server ja andmebaas, Azure'i teenuste automaatselt lubatud. BizTalki teenus nõuab Azure'i teenuste olema lubatud.</li>
<li>Kui loote uue SQL Azure'i andmebaasi olemasoleva Azure SQL serveri, tulemüüri reeglid server ei muudeta. Seetõttu on võimalik muude teenuste Azure'i ei pääse serveri andmebaasiga.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure'i juurdepääsu reguleerimine nimeruum</td>
<td>Autendib Azure BizTalki teenustega. Visual Studio projekti BizTalki teenuse juurutamisel sisestate selle nimeruumi juurdepääsu reguleerimine. Kui loote BizTalki teenus, luuakse automaatselt nimeruumi juurdepääsu reguleerimine.</td>
</tr>

<tr>
<td>Azure'i salvestusruumi konto</td>
<td>Annab juurdepääsu tabelite, plekid ja järjekorrad BizTalki teenust kasutada salvestamiseks järgmist:

<ul>
<li>Saate jälgida hooldust BizTalki logifailid. Jälgimise väljund kuvatakse ka **jälgimis** menüüs Azure'i portaalis.</li>
<li>Luues mõne X12 või AS2 lepingu partnerite vahel, saate lubada arhiivimise funktsiooni sõnumi atribuutide talletamiseks. Andmed salvestatakse salvestusruumi konto.</li>
</ul>
<br/>
Kui loote BizTalki teenus, saate kasutada olemasoleva salvestusruumi konto või automaatselt uue salvestusruumi konto loomine.
<br/><br/>
Vaikimisi seatud on piisavalt BizTalki teenuse.
<br/><br/>
Salvestusruumi konto loomisel primaarvõtme ja teisese võtme luuakse automaatselt. Järgmiste klahvide salvestusruumi konto juurdepääsu reguleerimine. BizTalki teenuse kasutab automaatselt primaarvõtme.
<br/><br/>
Lisateabe saamiseks vaadake <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">salvestusruumi</a> .
</td>
</tr>

<tr>
<td>Privaatne SSL-sert</td>
<td>
Azure'i BizTalki teenuse loomisel luuakse ka HTTPS-URL, mis sisaldab teie BizTalki teenuse nimi. See URL on konfigureeritud automaatselt kasutada iseallkirjastatud serdi loomine ainult. Tekitamiseks, peate privaatne SSL-sert.
<br/><br/>
<strong>Teavet olulised SSL-i serdi kohta.</strong>

<ul>
<li>Serdi aegumiskuupäeva peab olema väiksem kui 5 aastat.</li>
<li>Parooli nõudmine kõik privaatne serdid. Teate selle parooli ja hea tava, jagada seda parooli oma administraatorid.</li>
<li>Test/arenduskeskkond kasutatakse iseallkirjastatud serdid. Kasutamisel iseallkirjastatud serdid importida serdi oma isikliku serdi talletamine ja Trusted Root sertimiskeskuste sert pood.</li>
</ul>
<br/>Kui tootmise serdi taotlus saata oma sertimiskeskus, andke serdi järgmised atribuudid.
<br/>

<ul>
<li><strong>Täiustatud võtme kasutamine</strong>: vähemalt, Azure BizTalki teenuste nõuab autentimist.</li>
<li><strong>Levinud nimi</strong>: sisestage oma Azure'i BizTalki teenuse URL-i täielik domeeninimi (FQDN). Selle artikli teemast <a HREF="#BizTalk">loomine BizTalki teenus</a> .</li>
</ul>
<br/>
Uue või senisest erineva serdi saab lisada BizTalki teenuse loomise järel.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hübriidjuurutuse ühendused

Azure'i BizTalki teenuse loomisel **Hü ühendused** vahekaart on saadaval:

![Hübriidjuurutuse vahekaarti ühendused][HybridConnectionTab]

Azure'i veebisaidi- või Azure mobiilsideteenuse ühenduse mis tahes kohapealse ressurss, mis kasutab staatiline TCP pordi, nt SQL Server, MySQL-i, HTTP Web API-d, Mobile teenuste ja enamik kohandatud veebiteenuste kasutatakse hübriidjuurutuse ühendusi.  Hübriidjuurutuse ühendused ja BizTalki adapterit teenus on erinevad. BizTalki adapterit Service kasutatakse ühenduse Azure'i BizTalki Services kohapealse rea Business (LOB) süsteemi.

 Vt lisateavet, sh loomise ja haldamise hübriid ühendused [Hübriid ühendused](integration-hybrid-connection-overview.md) .


## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui BizTalki teenus on loodud, tutvuge erinevate [BizTalki teenuste: vahekaardid armatuurlaud, jälgimine ja skaala](biztalk-dashboard-monitor-scale-tabs.md). BizTalki teenust on valmis rakendusi. Rakenduste loomise alustamiseks avage [Azure'i BizTalki teenuseid](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Vt ka
- [BizTalki Services: Väljaanded diagramm](biztalk-editions-feature-chart.md)<br/>
- [BizTalki Services: Maakond diagrammi](biztalk-service-state-chart.md)<br/>
- [BizTalki Services: Varundus ja taaste](biztalk-backup-restore.md)<br/>
- [BizTalki teenuste: ahendamine](biztalk-throttling-thresholds.md)<br/>
- [BizTalki Services: Väljaandja nimi ja väljaandja võti](biztalk-issuer-name-issuer-key.md)<br/>
- [Kuidas alustada abil Azure'i BizTalki teenuste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hübriidjuurutuse ühendused](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
