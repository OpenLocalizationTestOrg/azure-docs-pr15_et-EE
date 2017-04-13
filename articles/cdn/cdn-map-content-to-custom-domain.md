<properties
     pageTitle="Kuidas Azure'i sisu kohaletoimetamise võrk (CDN) sisu vastendamiseks kohandatud domeeni | Microsoft Azure'i"
     description="Selles teemas näitavad, kuidas kohandatud domeeniga CDN sisu vastendamiseks."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Kohandatud domeeni vastendamine sisu kohaletoimetamise võrk (CDN) lõpp-punkti kohta
Selleks, et kasutada oma domeeninime URL-ide vahemällu talletatud sisu, mitte azureedge.net alamdomeen abil saate vastendada CDN lõpp-punkti kohandatud domeeni.

On kaks võimalust vastendamiseks kohandatud domeeni CDN lõpp-punkti.

1. [CNAME-kirje loomine teie domeeniregistraator ja Vastendage oma kohandatud domeeni ja alamdomeeni CDN lõpp-punkti](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    CNAME-kirje on DNS-i funktsioon, mis kaartide allika domeen, nt `www.contosocdn.com` või `cdn.contoso.com`, sihtkoha domeeni. Sel juhul allika domeen on teie kohandatud domeeni ja subdomain (alamdomeen, nagu **www-d** või **cdn** on alati vaja). Sihtdomeeni on teie CDN lõpp-punkti.  

    Protsessi vastendus kohandatud domeeni oma CDN lõpp-punkti, saate siiski tulemuseks tööseisakute domeeni lühike samal ajal, kui teil on Azure portaalis domeeni registreerimisel.

2. [Lisage koos **cdnverify** samm vahe registreerimine](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Kui kohandatud domeeni toetab praegu taotluse ja teenusetaseme leping (SLA), mis eeldab, et seal pole tööseisakute olla, saate Azure **cdnverify** alamdomeen on vahe registreerimise samm anda, et kasutajad saaksid juurdepääsu oma domeeni DNS-i vastendamise toimub ajal kasutada.  

Pärast registreerimist ülaltoodud protseduuri kasutades kohandatud domeeni, mida soovite [veenduge, et kohandatud alamdomeen viitab teie CDN lõpp-punkti](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Peate looma CNAME-kirje oma domeeniregistraatori domeeni vastendamiseks CDN lõpp-punkti. CNAME-kirje vastendamine teatud alamdomeene nagu `www.contoso.com` või `cdn.contoso.com`. Ei ole võimalik vastendamiseks CNAME-kirje juurdomeeni, näiteks `contoso.com`.
>    
> Alamdomeen saab ainult seostatud ühe CDN lõpp-punkti. CNAME-kirjet, mille loote tee kogu liikluse, mis on adresseeritud määratud lõpp-punkti alamdomeen.  Näiteks kui seote `www.contoso.com` koos oma CDN lõpp-punkti, siis ei saa seostada seda muude Azure lõpp-punktid, nt salvestusruumi konto lõpp-punkti või pilvepõhise teenuse lõpp-punkti. Siiski saate kasutada erinevate alamdomeene sama domeeni jaoks muu teenuse lõpp-punktid. Saate ka vastendada erinevate alamdomeene sama CDN lõpp-punkti.
>
> **Azure'i CDN Verizon** (Standard ja Premium) lõpp-punktide jaoks Pange tähele, et see ei võta **90 minutit** kohandatud domeeni muudatuste levitamine CDN serva sõlmed.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Azure'i CDN lõpp-punkti jaoks kohandatud domeeninime registreerimine

1.  Logige [Azure portaali](https://portal.azure.com/).
2.  Klõpsake nuppu **Sirvi**, seejärel **CDN profiilid**, siis CDN profiili lõpp-punkti, mida soovite vastendada kohandatud domeeni.  
3.  Klõpsake labale **CDN profiil** , millega soovite seostada alamdomeen CDN lõpp-punkti.
4.  Lõpp-punkti tera ülaosas nuppu **Kohandatud domeeni lisamine** .  **Lisada kohandatud domeeni** tera, kuvatakse teie CDN lõpp-punkti, uus CNAME-kirje loomiseks kasutada saadud lõpp-punkti hosti nimi. Hosti nimi aadress vorming kuvatakse ** &lt;EndpointName >. azureedge.net**.  Saate kopeerida See hostinimi CNAME-kirje loomiseks kasutada.  
5.  Liikuge oma domeeniregistraatori veebisaidile ja otsige üles jaotis DNS-i kirjete loomise kohta. Seda võib leida näiteks **Domeeni nimi**, **DNS-i**või **Name Server Management**jaotise.
6.  Otsige üles jaotis CNAMEs haldamine. Kui teil on täpsemate sätete lehe avamiseks ja otsida sõnu, CNAME, pseudonüümi või alamdomeene.
7.  Looge uus CNAME-kirje, mis teie valitud alamdomeeni (nt **www-d** või **cdn**) kaartide ette **lisada kohandatud domeeni** tera hostinime.
8.  **Lisada kohandatud domeeni** tera naasmiseks ja sisestage oma kohandatud domeeni, sh alamdomeen, dialoogiboksi. Sisestage domeeni nimi, näiteks vormingus `www.contoso.com` või `cdn.contoso.com`.   

    Azure'i on veenduge, et CNAME-kirje olemas sisestatud domeeni nime. Kui CNAME-i, oma kohandatud domeen on kinnitatud.  **Azure'i CDN Verizon** (Standard ja Premium) lõpp-punktide jaoks võib kuluda kuni 90 minutit kajastuma kõik CDN serva sõlmed, kuid kohandatud domeeni sätted.  

    Pange tähele, et mõnel juhul võib kuluda aega CNAME-kirje kajastuma nimeserverite Internetis. Teie domeen on kinnitatud kohe, kui arvate, et see on õige CNAME-kirjet, siis oodake mõni minut ja proovige uuesti.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Azure'i CDN lõpp-punkti vahendaja cdnverify alamdomeen kasutades kohandatud domeeni registreerimine  

1. Logige [Azure portaali](https://portal.azure.com/).
2. Klõpsake nuppu **Sirvi**, seejärel **CDN profiilid**, siis CDN profiili lõpp-punkti, mida soovite vastendada kohandatud domeeni.  
3. Klõpsake labale **CDN profiil** , millega soovite seostada alamdomeen CDN lõpp-punkti.
4. Lõpp-punkti tera ülaosas nuppu **Kohandatud domeeni lisamine** .  **Lisada kohandatud domeeni** tera, kuvatakse teie CDN lõpp-punkti, uus CNAME-kirje loomiseks kasutada saadud lõpp-punkti hosti nimi. Hosti nimi aadress vorming kuvatakse ** &lt;EndpointName >. azureedge.net**.  Saate kopeerida See hostinimi CNAME-kirje loomiseks kasutada.
5. Liikuge oma domeeniregistraatori veebisaidile ja otsige üles jaotis DNS-i kirjete loomise kohta. Seda võib leida jaotises, nt **Domeeninime**, **DNS-i**või **Name Server Management**.
6. Otsige üles jaotis CNAMEs haldamine. Kui teil avada täpsemate sätete lehe ja otsida sõnu **CNAME**, **pseudonüümi**või **alamdomeene**.
7. Uus CNAME-kirje ja sisestage alamdomeeni pseudonüümi, mis sisaldab **cdnverify** alamdomeen. Näiteks on alamdomeen, mille teie määratud vormingu **cdnverify.www** või **cdnverify.cdn**. Sisestage hosti nimi, mis on teie CDN lõpp-punkti, vormingus **cdnverify.&lt; EndpointName >. azureedge.net**.
8. **Lisada kohandatud domeeni** tera naasmiseks ja sisestage oma kohandatud domeeni, sh alamdomeen, dialoogiboksi. Sisestage domeeni nimi, näiteks vormingus `www.contoso.com` või `cdn.contoso.com`. Pange tähele, et selles etapis tuleb teil pole vaja eessõna alamdomeen **cdnverify**abil.  

    Azure'i on veenduge, et CNAME-kirje olemas on sisestatud cdnverify domeeni nime.
9. Selles etapis oma kohandatud domeen on kinnitatud Azure, kuid liikluse teie domeeni pole veel marsruuditakse teie CDN lõpp-punkti. Pärast ootel kaua lubada kohandatud domeeni sätteid kajastuma CDN serva sõlmed (90 minutit **Azure'i CDN Verizon**, 1-2 minutit **Akamai: Azure'i CDN-ID**), naaske oma DNS-i domeeniregistraatori veebisaidile ja teise CNAME-kirje mis kaardid oma alamdomeen oma CDN lõpp-punkti. Näiteks määrata **www-d** või **CDN-ID**ja hostname nimega alamdomeen ** &lt;EndpointName >. azureedge.net**. Selle toiminguga oma kohandatud domeeni registreerimist on lõpule viidud.
10. Lisaks saate kustutada CNAME-kirje, mis on loodud, kasutades **cdnverify**, nagu see oli vajalik ainult nimega mõõdaksid.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Veenduge, et kohandatud alamdomeen viitab teie CDN lõpp-punkti

- Kui olete lõpetanud oma kohandatud domeeni registreerimist, pääsete sisu, mis on vahemälus talletatud teie CDN algusega, kasutades kohandatud domeeni.
Esmalt veenduge, et on avalik sisu vahemällu lõpp-punkti. Kui teie CDN lõpp-punkti salvestusruumi kontoga seostatud, vahemälu on CDN avaliku bloobimälu ümbriste sisu. Kohandatud domeeni testimiseks tagada avaliku juurdepääsu lubamiseks on määratud oma container ning et see sisaldaks vähemalt üks bloobimälu.
- Liikuge brauseris bloobimälu, kasutades kohandatud domeeni aadress. Kui teie kohandatud domeen on näiteks `cdn.contoso.com`, URL-i vahemällu talletatud bloobimälu on sarnane järgmine URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Vt ka

[Kuidas lubada Azure Sisuedastusvõrgud (CDN)](./cdn-create-new-endpoint.md)  
