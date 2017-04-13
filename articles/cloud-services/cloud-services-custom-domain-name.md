<properties
    pageTitle="Konfigureerida kohandatud domeeninime pilveteenustega | Microsoft Azure'i"
    description="Saate teada, kuidas seada oma Azure'i rakendust või andmete kohandatud domeeni DNS-i sätete konfigureerimisega."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Kohandatud domeeni nimi on Azure pilveteenuses konfigureerimine

> [AZURE.SELECTOR]
- [Azure'i portaal](cloud-services-custom-domain-name-portal.md)
- [Azure'i klassikaline portaal](cloud-services-custom-domain-name.md)


Kui loote mõnda pilveteenusesse, määratakse Azure'i cloudapp.net alamdomeen. Näiteks kui teie pilveteenuses nimi on "contoso", kasutajate saab teie taotlus on nt http://contoso.cloudapp.net URL-i kohta. Azure'i määrab ka virtuaalse IP-aadressi.

Siiski saate ka seada rakenduse oma domeeninime, nt contoso.com. Selles artiklis selgitatakse, kuidas reserveerida või konfigureerida kohandatud domeeninime pilveteenuses web rollid.

Kas teil on juba undestand mis A ja CNAME-kirjed on? [Viimase soovitud seletamist hüpata](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Alustamine kiiremini! Kasutage Azure [juhendamisega ülevaade](http://support.microsoft.com/kb/2990804). See muudab seostada kohandatud domeeninime ja turvaliseks (SSL) suhtlemine Azure'i pilveteenustega või Azure veebisaitide snap.

<p/>

> [AZURE.NOTE]
> Selles ülesandes toiminguid rakendada Azure'i pilveteenustega. Rakenduse teenused, leiate [selle](../app-service-web/web-sites-custom-domain-name.md). Salvestusruumi kontod, leiate [selle](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>A- ja CNAME-kirjeid mõistmine

CNAME (või alias (pseudonüüm) kirjete) ja mõlemad kirjed võimaldavad domeeninime seostamine konkreetse server (või teenuse sel juhul), kuid nad toimida teisiti. On ka teatud kindlate kaalutluste kasutamisel on kirjete Azure'i pilveteenustega, mida peaks kaaluma enne kasutamiseks.

### <a name="cname-or-alias-record"></a>Alias (pseudonüüm)- või CNAME-kirje

CNAME-kirje kaardid *eraldi* domeen, nt **contoso.com** või **www.contoso.com**, kanoonilise domeeni nimi. Sel juhul kanoonilise domeeninimi on selle **[myapp] .cloudapp .net** domeeninime oma Azure majutatud rakenduse. Kui loodud, loob CNAME alias on **[myapp] .cloudapp .net**. CNAME-kirje on IP-aadressi lahendamiseks oma **[myapp] .cloudapp .net** teenuse automaatselt, et kui pilveteenusesse IP-aadress muutub, te ei pea vastu võtta.

> [AZURE.NOTE]
> Mõned domeeniregistraatori võimaldavad ainult vastendamine alamdomeene kasutamisel CNAME-kirje (nt www.contoso.com) ja mitte juurkausta nimed, nt contoso.com. CNAME-kirje kohta lisateabe saamiseks dokumentatsioonist oma domeeniregistraatori, [Wikipedia kirje CNAME-kirje](http://en.wikipedia.org/wiki/CNAME_record)või dokumendi [IETF domeeninimede - rakendamise ja määratlus](http://tools.ietf.org/html/rfc1035) .

### <a name="a-record"></a>Kirje

A-kirje kaartide domeeni, nt **contoso.com** või **www.contoso.com**, *või metamärkide domeeni* nagu ** \*. contoso.com**, IP-aadressi. Puhul Azure pilveteenuse, virtuaalse IP teenuse. Nii, et peamine kasu A-kirje üle CNAME-kirje on, et teil on üks kirje, mis kasutab metamärke, näiteks \* **. contoso.com**, mis käsitleks taotlused mitme alamdomeenide, nt **mail.contoso.com**, **login.contoso.com**või **www.contso.com**.

> [AZURE.NOTE]
> Kuna A-kirje on vastendatud staatiline IP-aadress, ei saa automaatselt lahendada muudatusi teie pilveteenuses IP-aadress. Kasutada oma pilveteenuses IP-aadress on eraldatud esimest korda juurutamist tühi pesa (tootmise või lavastus.) Kui kustutate juurutamise pesa, Azure'i välja IP-aadress ja mis tahes tulevaste juurutuste pesa võib anda uue IP-aadress.
>
> Mugavalt, on antud juurutamise pesa (tootmise või lavastus) IP-aadress püsis kui lavastus ja tootmise kasutuselevõttu või olemasoleva juurutuse, kohapealne värskendamisel vahel vahetamine. Nende toimingute kohta lisateabe saamiseks vaadake, [Kuidas hallata pilveteenustega](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Kohandatud domeeni CNAME-kirje lisamine

CNAME-kirje loomiseks peate lisama uue kirje tabelis DNS-i oma kohandatud domeeni esitatud oma domeeniregistraatori tööriistade abil. Iga domeeniregistraatori on sarnane, kuid veidi teistsugused meetod täpsustades CNAME-kirje, kuid mõisted on samad.

1. Üht järgmistest meetoditest leidmiseks kasutada funktsiooni **. cloudapp.net** määratud pilveteenusesse oma domeeninimi.

    * [Azure'i klassikaline portaalis]oma pilveteenuses, valige **armatuurlaud**, ja seejärel otsige **Saidi URL-i** kirje **kiire ülevaade** jaotises Valige Logi sisse.
    
        ![Kiire ülevaade jaotises nähtaval saidi URL-i][csurl]
    
        **VÕI**  
    
    * Installida ja konfigureerida [Azure Powershell](../powershell-install-configure.md)ja seejärel kasutage järgmine käsk:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Salvestage kasutatav mõlema meetodi tagastatud URL-i, peate selle CNAME-kirje loomisel domeeninimi.

1.  Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad.

2.  Nüüd asub, kus saate valida või sisestada CNAME's. Kui teil valida kirje tüüp: drop alla või avada täpsemate sätete lehe. Sõnade **CNAME**, **pseudonüümi**või **alamdomeene**peaks välja nägema.

3.  CNAME-i, nt **www** jaoks domeeni või alamdomeeni pseudonüüm peab võimaldama kui soovite luua pseudonüümi **www.customdomain.com**. Kui soovite luua pseudonüümi, et juurdomeeni, võib olla loendis selle funktsiooni '**@**' oma domeeniregistraatori DNS-i tööriistade sümbol.

4. Seejärel kanoonilise hosti nimi, mis on rakenduse **cloudapp.net** domeeni sel juhul peate sisestama.

Näiteks järgmised CNAME-kirje edastab kogu liikluse kaudu **www.contoso.com** **contoso.cloudapp.net**, kohandatud domeeninime kasutada rakenduse:

| Alias (pseudonüüm) / hosti nimi/alamdomeeni | Kanoonilise domeeni     |
| ------------------------- | -------------------- |
| www                       | contoso.cloudapp.net |

**Www.contoso.com** külastajale ei näe true host (contoso.cloudapp.net), et edastamise protsess on nähtamatu lõppkasutaja.

> [AZURE.NOTE]
> Ülaltoodud näites kehtib ainult liikluse alamdomeen **www** . Kuna te ei saa kasutada metamärke CNAME-kirjet, peate looma ühe CNAME iga domeeni ja alamdomeeni jaoks. Kui soovite, mis suunaks liikluse aadressilt alamdomeene, näiteks \*. contoso.com, aadressile cloudapp.net saate konfigureerida **URL-i ümber suunata** või **Edasi URL-i** kirje oma DNS-i sätete või A-kirje loomine.


## <a name="add-an-a-record-for-your-custom-domain"></a>Kohandatud domeeni A-kirje lisamine

A-kirje loomiseks peate esmalt leida oma pilveteenuses virtuaalse IP-aadress. Lisage uus kirje oma kohandatud domeeni DNS-i tabelis esitatud oma domeeniregistraatori tööriistade abil. Iga domeeniregistraatori on sarnane, kuid veidi teistsugused meetodit, A-kirje, kuid mõisted on samad.

1. Kasutage üht järgmistest meetoditest, et leida oma pilveteenuses IP-aadress.
    
    * [Azure'i klassikaline portaalis]oma pilveteenuses, valige **armatuurlaud**, ja seejärel otsige **avaliku virtuaalse IP (VIP) aadressi** kirje **kiire ülevaade** jaotises Valige Logi sisse.
    
        ![Kiire ülevaade jaotises nähtaval VIP][vip]
    
        **VÕI**  
    
    * Installida ja konfigureerida [Azure Powershell](../powershell-install-configure.md)ja seejärel kasutage järgmine käsk:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Kui teil on seostatud teie pilveteenuses mitme lõpp-punktid, saate mitu rida, mis sisaldab IP-aadress, kuid kõik peaks olema kuvatud sama aadress.
    
    Kui teil on A-kirje loomisel, salvestage IP-aadress.

1.  Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Otsige linke või saidi **Domeeninime**, **DNS-i**või **Name Server Management**märgistatud alad.

2.  Nüüd asub, kus saate valida või sisestage soovitud kirje. Kui teil valida kirje tüüp: drop alla või avada täpsemate sätete lehe.

3. Valige või sisestage domeeni või alamdomeen, mille kasutatakse A-kirje. Näiteks valige **www-d** , kui soovite luua pseudonüümi **www.customdomain.com**. Kui soovite kõik alamdomeenide metamärkide kirje loomine, sisestage "__*__". See hõlmab kõiki alamdomeenide, nt **mail.customdomain.com**, **login.customdomain.com**ja **www.customdomain.com**.

    Kui soovite, et juurdomeeni A-kirje loomiseks, selle võib loetleda soovitud '**@**' oma domeeniregistraatori DNS-i tööriistade sümbol.

4. Sisestage esitatud väljale oma pilveteenuses IP-aadress. See seostab domeeni kirjet kasutatakse A-kirje oma pilvepõhise teenuse juurutuse IP-aadressiga.

Näiteks järgmine kirje edastab kogu liikluse **contoso.com** **137.135.70.239**, juurutatud rakenduse IP-aadress:

| Hosti nimi/alamdomeeni | IP-aadress     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



Selles näites näitab, et juurdomeeni A-kirje loomise. Kui soovite kõik alamdomeene katmiseks metamärkide kirje loomine, sisestage '__*__' alamdomeen nimega.

>[AZURE.WARNING]
>IP-aadresside Azure on vaikimisi dünaamiline. Tõenäoliselt soovite kasutada [reserveeritud IP-aadress](../virtual-network/virtual-networks-reserved-public-ip.md) tagamaks, et IP-aadressi ei muutu.

## <a name="next-steps"></a>Järgmised sammud

* [Kuidas hallata pilveteenustega](cloud-services-how-to-manage.md)
* [Kuidas CDN sisu vastendamiseks kohandatud domeeni](../cdn/cdn-map-content-to-custom-domain.md)
* [Üldine konfigureerimine oma pilveteenuses](cloud-services-how-to-configure.md).
* Siit saate teada, juurutamise [mõnda pilveteenusesse](cloud-services-how-to-create-deploy.md).
* [Ssl-sertide](cloud-services-configure-ssl-certificate.md)konfigureerimine.




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 