<properties
    pageTitle="Domeeninime bloobimälu salvestusruumi lõpp-punkti konfigureerimine | Microsoft Azure'i"
    description="Saate teada, kuidas kohandatud domeeni vastendamine bloobimälu salvestusruumi lõpp-punkti Azure'i klassikaline portaalis Azure storage konto jaoks."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Kohandatud domeeninime bloobimälu salvestusruumi lõpp-punkti konfigureerimine

## <a name="overview"></a>Ülevaade

Saate konfigureerida kohandatud domeeni bloobimälu andmete Azure storage konto. Vaikimisi lõpp-punkti jaoks bloobimälu `<storage-account-name>.blob.core.windows.net`. Kui vastendate kohandatud domeeni ja alamdomeeni, nt **www.contoso.com** bloobimälu lõpp-punkti salvestusruumi konto, siis teie kasutajad saavad ka Accessi bloobimälu andmed teie salvestusruumi konto abil domeeni.

>[AZURE.IMPORTANT] Ei toeta Azure Storage HTTPS veel kohandatud domeenide. Oleme meeles, et kliendid huvi selle funktsiooni ja seda kasutada tulevikus.

On kaks võimalust oma kohandatud domeeni osutamiseks bloobimälu lõpp-punkti salvestusruumi konto jaoks. Lihtsaim viis on CNAME-kirje, vastendades oma kohandatud domeeni ja alamdomeeni bloobimälu lõpp-punkti loomiseks. CNAME-kirje on DNS-i funktsioon, mis kaartide allika domeeni sihtkoha domeeni. Sel juhul allika domeen on teie kohandatud domeeni ja alamdomeeni--Pange tähele, et alamdomeen on alati vaja. Sihtdomeeni on bloobimälu teenuse lõpp-punkti.

Protsessi vastendus kohandatud domeeni oma bloobimälu lõpp-punkti, saate siiski tulemuseks tööseisakute domeeni lühike samal ajal, kui teil on registreerimisel domeeni [Azure klassikaline portaali](https://manage.windowsazure.com). Kui kohandatud domeeni toetab praegu taotluse ja teenusetaseme leping (SLA), mis eeldab, et seal pole tööseisakute olla, saate Azure **asverify** alamdomeen on vahe registreerimise samm anda, et kasutajad saaksid juurdepääsu oma domeeni DNS-i vastendamise toimub ajal kasutada.

Järgmine tabel näitab valimi URL-ide bloobimälu andmete salvestusruumi konto nimega **mystorageaccount**. Kohandatud domeeni, mis on registreeritud salvestusruumi konto on **www.contoso.com**.

Ressursi tüüp|URL-i vormingud
---|---
Salvestusruumi konto|**Vaikimisi URL:** http://mystorageaccount.blob.core.windows.net<p>**Kohandatud domeeni URL:** http://www.contoso.com</td>
Bloobimälu|**Vaikimisi URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Kohandatud domeeni URL:** http://www.contoso.com/mycontainer/myblob
Root container|**Vaikimisi URL:** http://mystorageaccount.blob.core.windows.net/myblob või http://mystorageaccount.blob.core.windows.net/$ root/myblob<p>**Kohandatud domeeni URL:** http://www.contoso.com/myblob või http://www.contoso.com/$ root/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Kohandatud domeeni salvestusruumi konto registreerimine

Selle toimingu abil saate oma kohandatud domeeni registreerimist, kui teil on probleeme kohta on selle domeeni kasutajad lühidalt pole saadaval või kohandatud domeeni on praegu hosting rakenduse.

Kui teie kohandatud domeeni toetab praegu rakendus, mis ei tohi olla mis tahes, siis kasutage esitatud <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">kohandatud domeeni kasutamise vahendaja asverify alamdomeen salvestusruumi konto registreerimine</a>.

Saate konfigureerida kohandatud domeeninime, peate looma oma domeeniregistraatori uus CNAME-kirje. CNAME-kirje täpsustab pseudonüümi domeeninime; Sel juhul kaardid aadress kohandatud domeeni bloobimälu salvestusruumi lõpp-punkti salvestusruumi konto jaoks.

Iga domeeniregistraatori on sarnane, kuid veidi teistsugused meetod täpsustades CNAME-kirje, kuid on sama. Pange tähele, et palju lihtsa domeeni registreerimist pakette pakuvad DNS-i konfigureerimine nii, et võib-olla peate täiendama oma domeeni registreerimist paketi, CNAME-kirje loomiseks.

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com), liikuge menüü **salvestusruumi** .

2.  Klõpsake vahekaarti **salvestusruumi** salvestusruumi konto jaoks, mida soovite vastendada kohandatud domeeni nime.

3.  Klõpsake vahekaarti **konfigureerimine** .

4.  Ekraani allservas nuppu **Manage Domain** kuvada dialoogiboksi **Kohandatud domeenide haldamine** . Dialoogiboksi ülaosas teksti, kuvatakse teave CNAME-kirje loomise kohta. Selle toimingu jaoks Ignoreeri teksti, mis viitab **asverify** alamdomeen.

5.  Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Seda võib leida jaotises, nt **Domeeninime**, **DNS-i**või **Name Server Management**.

6.  Otsige üles jaotis CNAMEs haldamine. Kui teil on täpsemate sätete lehe avamiseks ja otsida sõnu **CNAME**, **pseudonüümi**või **alamdomeene**.

7.  Uus CNAME-kirje ja sisestage alamdomeeni pseudonüümi, nt **www-d** ja **fotosid**. Seejärel sisestage hosti nimi, mis on teie bloobimälu teenuse lõpp-punkti, klõpsake vorming **mystorageaccount.blob.core.windows.net** (kus **mystorageaccount** on teie salvestusruumi konto nimi). Hosti nimi, mida kasutada on mõeldud teie tekst dialoogiboksi **Kohandatud domeenide haldamine** .

8.  Pärast CNAME-kirje loomist naaske dialoogiboksi **Kohandatud domeenide haldamine** ja sisestage oma kohandatud domeeni alamdomeen, sh **Kohandatud domeeni nimi** väljale nimi. Näiteks kui teie domeen on **contoso.com** ja teie alamdomeen on **www-d**, sisestage **www.contoso.com**; Kui teie alamdomeen on **fotod**, sisestage **photos.contoso.com**. Pange tähele, et alamdomeen pole vaja.

9. Nuppu **Registreeri** kohandatud domeeni registreerimiseks.

    Kui registreerimine õnnestub, kuvatakse sõnumi **oma kohandatud domeeni on aktiivne**. Kasutajad saavad nüüd andmete kuvamine bloobimälu oma kohandatud domeeni, kui neil on vajalikud õigused.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Kohandatud domeeni kasutamise vahendaja asverify alamdomeen salvestusruumi konto registreerimine

Selle toiminguga saate registreerida oma kohandatud domeeni, kui teie kohandatud domeeni toetab praegu SLA, mis eeldab, et rakendus olemas olla pole tööseisakute. Luues CNAME, mis osutab asverify. &lt;alamdomeen&gt;. &lt;customdomain&gt; asverify abil. &lt;storageaccount&gt;. blob.core.windows.net, saate oma domeeni Azure'i eelnevalt registreerida. Seejärel saate luua teine CNAME-i, mis osutab kaudu &lt;alamdomeen&gt;. &lt;customdomain&gt; et &lt;storageaccount&gt;. blob.core.windows.net, misjärel liikluse oma kohandatud domeeni suunatakse teie bloobimälu lõpp-punkti.

Asverify alamdomeen on Azure tuvasta teisiti alamdomeen. Prepending **asverify** abil oma alamdomeen, mida te lubate Azure'i tuvastama oma kohandatud domeeni DNS-i kirje domeeni muutmata. Kui teie domeeni DNS-i kirje muutmiseks vastendatakse see pole tööseisakute bloobimälu lõpp-punkti.

1.  [Azure'i klassikaline portaali](https://manage.windowsazure.com), liikuge menüü **salvestusruumi** .

2.  Klõpsake vahekaarti **salvestusruumi** salvestusruumi konto jaoks, mida soovite vastendada kohandatud domeeni nime.

3.  Klõpsake vahekaarti **konfigureerimine** .

4.  Ekraani allservas nuppu **Manage Domain** kuvada dialoogiboksi **Kohandatud domeenide haldamine** . Dialoogiboksi ülaosas teksti, kuvatakse teabe abil **asverify** alamdomeen CNAME-kirje loomise kohta.

5.  Logige sisse oma DNS-i domeeniregistraatori veebisaidile ja minge DNS-i haldamise leht. Seda võib leida jaotises, nt **Domeeninime**, **DNS-i**või **Name Server Management**.

6.  Otsige üles jaotis CNAMEs haldamine. Kui teil on täpsemate sätete lehe avamiseks ja otsida sõnu **CNAME**, **pseudonüümi**või **alamdomeene**.

7.  Uus CNAME-kirje ja sisestage alamdomeeni pseudonüümi, mis sisaldab asverify alamdomeen. Näiteks saate määrata alamdomeen paigutub vorming **asverify.www** või **asverify.photos**. Seejärel sisestage hosti nimi, mis on teie bloobimälu teenuse lõpp-punkti, klõpsake vorming **asverify.mystorageaccount.blob.core.windows.net** (kus **mystorageaccount** on teie salvestusruumi konto nimi). Hosti nimi, mida kasutada on mõeldud teie tekst dialoogiboksi **Kohandatud domeenide haldamine** .

8.  Pärast CNAME-kirje loomist **Kohandatud domeenide haldamine** dialoogiboksi naasmiseks ja oma kohandatud domeeni nimi, sisestage väljale **Kohandatud domeeni nimi** . Näiteks kui teie domeen on **contoso.com** ja teie alamdomeen on **www-d**, sisestage **www.contoso.com**; Kui teie alamdomeen on **fotod**, sisestage **photos.contoso.com**. Pange tähele, et alamdomeen pole vaja.

9.  Märkige ruut, mis ütleb, et **Täpsemalt: 'asverify' alamdomeen abil saate oma kohandatud domeeni preregister**.

10. Klõpsake nuppu **Registreeri** preregister kohandatud domeeni.

    Kui st õnnestub, kuvatakse sõnumi **oma kohandatud domeeni on aktiivne**.

11. Selles etapis oma kohandatud domeen on kinnitatud Azure, kuid liikluse teie domeeni pole veel marsruuditakse salvestusruumi kontole. Protsessi lõpuleviimiseks tagasi oma DNS-i domeeniregistraatori veebisaidile ja teise CNAME-kirje, et teie alamdomeen kaarte bloobimälu teenuse lõpp-punkti. Näiteks saate määrata alamdomeen **www-d** või **fotosid**ja hostname nimega **mystorageaccount.blob.core.windows.net** (kus **mystorageaccount** on teie salvestusruumi konto nimi). Selle toiminguga oma kohandatud domeeni registreerimist on lõpule viidud.

12. Lisaks saate kustutada CNAME-kirje, mis on loodud, kasutades **asverify**, nagu see oli vajalik ainult nimega mõõdaksid.

Kasutajad saavad nüüd andmete kuvamine bloobimälu oma kohandatud domeeni, kui neil on vajalikud õigused.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Veenduge, et kohandatud domeeni viitab bloobimälu teenuse lõpp-punkti

Veenduge, et kohandatud domeeni vastendatakse tõepoolest bloobimälu teenuse lõpp-punkti, luua avaliku ümbrises kontol salvestusruumi lisamine bloobimälu. Seejärel veebibrauseris, kasutage URI järgmises vormingus juurdepääsuks on bloobimälu:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Näiteks võib kasutada järgmist URI juurdepääsu veebivormi kaudu **photos.contoso.com** kohandatud alamdomeeni, mis on bloobimälu oma **myforms** ümbrises kaardid:

-   http://photos.contoso.com/myForms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Kohandatud domeeni salvestusruumi kontolt unregister

Kohandatud domeeni registreerimise tühistamiseks tehke järgmist. 

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logima. 

2. Klõpsake navigeerimispaanil **salvestusruumi**. 

3. Klõpsake lehel **salvestusruumi** armatuurlaud kuvamiseks salvestusruumi konto nimi. 

5. Klõpsake lindil nuppu **Manage Domain**. 

6. Klõpsake dialoogiboksis **Kohandatud domeenide haldamine** **registreerimise tühistamise**. 


## <a name="additional-resources"></a>Lisaressursid

-   [Kohandatud domeeni vastendamine sisu kohaletoimetamise võrk (CDN) lõpp-punkti kohta](../cdn/cdn-map-content-to-custom-domain.md)
