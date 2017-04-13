<properties
    pageTitle="Klahv Vault CLI abil hallata | Microsoft Azure'i"
    description="Selle õpetuse abil CLI abil automatiseerida klahvi Vault tavalised toimingud"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Klahv Vault abil CLI haldamine #
Azure'i klahvi Vault on saadaval enamikes piirkondades. Lisateabe saamiseks lugege teemat [klahvi Vault hinnad lehe](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Sissejuhatus  
Selle õpetuse abil saate alustamine Azure'i klahvi Vault Azure talletamiseks ja haldamiseks cryptographic võtmed ja Azure saladused kõva container (võlvkelder) loomiseks. See juhatab teid läbi protsessi loomine on hoidla, mis sisaldab võti või parool, mida saate kasutada Azure rakendusega Azure'i platvormidel käsurea kasutajaliidese abil. See siis näitab teile, kuidas rakenduse saate selle klahvi või parool.

**Hinnanguline aega:** 20 minutit

>[AZURE.NOTE]  Selle õpetuse juhised, kuidas kirjutada Azure rakenduse üks samme sisaldab, mis näitab, kuidas lubada taotluse kasutada klahvi või salajane võtme võlvkelder ei sisalda.
>
>Praegu ei saa konfigureerida Azure klahvi Vault Azure'i portaalis. Selle asemel kasutage neid juhiseid platvormidel käsurea liides. Või Azure PowerShelli juhised leiate teemast [õppeteema samaväärsed](key-vault-get-started.md).

Azure'i klahvi Vault kohta ülevaate leiate teemast [mis on Azure klahvi Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Eeltingimused
Selle õpetuse lõpuleviimiseks peab olema järgmised:

- Microsoft Azure'i tellimust. Kui te ei ole, mida saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial).
- Käsurea liides versiooni 0.9.1 või uuem versioon. Installige uusim versioon ja ühenduse Azure tellimuse, leiate teemast [installida ja konfigureerida Azure platvormidel käsurea liides](../xplat-cli-install.md).
- Rakendus, mis konfigureeritakse kasutage või parool, mida selles õpetuses loote. Valimi rakendus on saadaval [Microsofti allalaadimiskeskuse](http://www.microsoft.com/download/details.aspx?id=45343)kaudu. Lisateabe saamiseks vaadake lisatud Readme-faili.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure'i platvormidel käsurea liides abi otsimine

Selle õpetuse eeldab, et olete tuttav käsurea liides (Bash, Terminal, Käsuviip)

--Abi või -h parameeter saab teatud käskude ja spikri kuvamiseks. Vaheldumisi azure aidake [käsk] [Valikud] vorming saab kasutada ka sama teabe tagastamiseks. Näiteks kõik järgmised käsud tagastavad sama teabega.

    azure account set --help

    azure account set -h

    azure help account set

Nõutud käsu parameetrite kohta kahtluse korral viidata spikri kasutamine – Spikker, -h või azure abi [käsk].

Samuti saate lugeda järgmised õppetükid tutvuda Azure'i ressursihaldur Azure'i platvormidel käsurea liides:

- [Kuidas installida ja konfigureerida Azure platvormidel käsurea liides](../xplat-cli-install.md)
- [Azure'i ressursihaldur Azure platvormidel käsurea liidest kasutades](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Ühenduse loomine oma tellimused

Sisselogimine ettevõttekontoga, kasutage järgmist käsku:

    azure login -u username -p password

või kui soovite sisse logida, tippides interaktiivseks

    azure login

>[AZURE.NOTE]  Sisselogimise meetod toimib ainult ettevõtte kontoga. Organisatsioonikonto on kasutaja, mis on teie asutuse hallatavate ja ettevõtte Azure Active Directory rentniku määratletud.


Kui teil on praegu organisatsioonikonto ja kasutate Microsofti kontoga sisse logida Azure tellimuse, saate hõlpsalt luua järgmiste juhiste abil.

1.  Logige sisse Login [Azure'i haldusportaal](https://manage.windowsazure.com/), ja klõpsake Active Directory.
2.  Kui kataloogi, valige Loo kataloogi ja sisestage nõutav teave.
3.  Valige kataloogi ja kasutaja lisamine. Uuele kasutajale on organisatsioonikonto. Kasutaja loomise ajal esitatakse meiliaadressiga nii kasutaja ja ajutine parool. Salvestage see teave, kui seda kasutatakse teise etapi.
4.  Portaalist, klõpsake nuppu sätted ja seejärel valige administraatorid. Valige Lisa ja lisage uus kasutaja koostöö administraatorina. See võimaldab ettevõtte konto haldamine tellimuse Azure.
5.  Lõpuks Logi Azure portaali ja seejärel logige uuesti sisse uue ettevõtte konto kaudu. Kui see on esimest korda logimine selle kontoga, palutakse teil parooli muutmine.

Microsoft Azure'i organisatsioonikonto kasutamise kohta leiate lisateavet teemast [Microsoft Azure'i organisatsiooni kasutajaks](../active-directory/sign-up-organization.md).

Kui teil on mitu tellimust ja soovite määrata teatud ühe Azure'i klahvi Vault jaoks, tippige oma konto tellimused kuvamiseks järgmist:

    azure account list

Tellimuse kasutada määramiseks tippige:

    azure account set <subscription name>

Azure'i platvormidel käsurea liides konfigureerimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure platvormidel käsurea liides](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Azure'i ressursihaldur üleminek

Klahv Vault on vaja Azure ressursihaldur, seega tippige järgmine Azure'i ressursihaldur lülitumine.

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Uue ressursirühma loomine

Azure'i ressursihaldur kasutamisel kõik seotud ressursid on loodud ressursirühma sees. Loome uue ressursirühma 'ContosoResourceGroup' selles õpetuses.

    azure group create 'ContosoResourceGroup' 'East Asia'

Esimene parameeter on ressursi rühma nime ja teine parameeter on asukoht. Asukoha, kasutage käsku `azure location list` tuvastamiseks määramine üks selles näites mõnda muusse asukohta. Kui vajate rohkem teavet, tippige:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registri võti Vault ressursi pakkuja
Veenduge, et teie tellimus on registreeritud selle klahvi Vault ressursi pakkuja.

`azure provider register Microsoft.KeyVault`

Seda tuleb teha kui tellimuse kohta.


## <a name="create-a-key-vault"></a>Võtme vault loomine

Kasutage funktsiooni `azure keyvault create` loomiseks olulisi vault käsk. See skript on kolm kohustuslikud parameetrid: ressursi rühma nime, võtme vault nimi ja geograafiline asukoht.

Näiteks kui kasutate ContosoKeyVault vault nime, ContosoResourceGroup ressursi rühma nime ja asukoha Ida-Aasia, tippige:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Selle käsu väljund kuvatakse soovitud klahv hoidla, mis äsja loodud atribuudid. On kaks kõige olulisemad atribuudid.

- **Nimi**: näites on ContosoKeyVault. Kasutate selle klahvi Vault cmdletid nimi.
- **vaultUri**: näites on https://contosokeyvault.vault.azure.net. Rakendustes, mis kasutavad teie vault kaudu oma REST API peate kasutama selle URI.

Azure'i konto on nüüd volitatud toiminguid selle võtme vault. Veel on keegi teine.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Klahv või salajane lisamine võtme hoidlasse

Azure'i klahvi Vault tarkvara kaitstud võtme loomiseks saate soovi korral kasutada funktsiooni `azure key create` ja tippige järgmine käsk:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Juhul, kui teil on mõne olemasoleva vajutamisega .pem faili nimega softkey.pem Azure klahvi Vault üles soovitud faili kohaliku failina salvestatud, tippige järgmine võtme importimise funktsiooni. PEM fail, mida kaitseb võti tarkvara klahvi Vault teenus:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Nüüd saate otsida loodud või üles laadida, kasutades oma URI Azure'i klahvi Vault, võti. Kasutage **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** alati saada praegune versioon ning **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** saada see teatud versioon.

Salajane lisamine soovitud hoidlasse, mis on nimega SQLPassword parooli ja mis on väärtus, Pa$ $w0rd Azure'i klahvi Vault abil, tippige järgmine:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Nüüd saate otsida selle parool, mida lisasite Azure'i klahvi Vault, abil oma URI. Kasutage **https://ContosoVault.vault.azure.net/secrets/SQLPassword** alati saada praegune versioon ning **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** selles teatud versiooni.

Klahv või äsja loodud salajane vaatame vaadata:

- Tootenumbri vaatamiseks tippige:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Tippige oma salajane kuvamiseks:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Azure Active Directory rakenduse registreerimine

Selles etapis tuleb tavaliselt oleksid arendaja, eraldi arvutis. See ei ole eriti Azure'i klahvi Vault, kuid on kaasatud siin täiendavalt.


>[AZURE.IMPORTANT] Õpetuse, oma konto, vault ja selles etapis tuleb registreerida rakenduse lõpuleviimiseks kõik tuleb Azure samas kaustas.

Rakendustes, mis kasutavad võtme vault peab autentida märgiks Azure Active Directory abil. Selle tegemiseks rakenduse omanik peate registreeruma rakendus oma Azure Active Directory. Registreerimise lõpus rakenduse omanik saab järgmised väärtused:


- **Rakenduse ID** (tuntud ka kui kliendi ID) ja **autentimise võti** (nimetatakse ka ühiskasutuses salajane). Rakenduse Azure Active Directory märgiks saamiseks peate esitama nii need väärtused. Kuidas rakendus on konfigureeritud selleks sõltub rakendusest. Klahv Vault valimi rakenduse, rakenduse omanik määrab need väärtused app.config faili.



Azure Active Directory rakenduse registreerimine

1. Azure portaali sisse logida.
2. Klõpsake vasakul nuppu **Active Directory**ja valige, kus registreerimist rakenduse kataloogi. <br> <br> Märkus: Valige sama kataloog, mis sisaldab Azure'i tellimus, millega olete loonud teie olulisi vault. Kui te ei tea, milline directory see on, klõpsake nuppu **sätted**ja tuvastamine, millega olete loonud teie olulisi vault tellimuse kataloogi viimases veerus kuvatakse nimi.

3. Klõpsake nuppu **rakendused**. Kui pole rakendused on lisatud kataloogi, kuvatakse selle lehe ainult linki **Lisa rakendus** . Klõpsake linki või teise võimalusena saate klõpsata funktsiooni **lisamine** käsku.
4.  Viisardi **Lisamine rakenduse** kohta on **mida te soovite teha?** leht, klõpsake käsku **Lisa rakendus minu ettevõte on arendamise**.
5.  Lehel **meile rakenduse kohta** Määrake oma rakenduse nimi ja valige **Rakendus ja/või WEB Veebiteenuste** (vaikesäte). Klõpsake ikooni edasi.
6.  **Rakenduse atribuutide** lehel, määrake **Logi-ON URL-i** ja **Rakenduse ID URI** veebirakenduses. Kui teie rakendus ei saa neid väärtusi, saate neid üles selle etapi (näiteks saate määrata http://test1.contoso.com mõlemale jaoks). Pole tähtis, kui nende saitide olemas; oluline on see, et iga rakenduse ID URI rakendus on kataloogis iga rakenduse jaoks erinevad. Kataloogi kasutab stringile rakenduse tuvastamiseks.
7.  Muudatuste salvestamiseks klõpsake viisardi täieliku ikooni.
8.  Klõpsake lehel Kiirkäivituse **KONFIGUREERIMINE**.
9.  Liikuge kerides jaotisse **klahvid** , valige kestus ja klõpsake siis nuppu **Salvesta**. Lehe värskendab ja nüüd kuvatakse soovitud väärtus. Konfigureerige rakenduse ning selle võtmeväärtuse **Kliendiid** väärtuse. (Juhised selle konfiguratsiooni saab rakenduse kohased.)
10. Kopeerige kliendi ID väärtus sellele lehele, mida te kasutate järgmise juhise juurde oma vault õiguste seadmiseks.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Lubada kasutada klahv või salajane

Lubada juurdepääsu (TAB) või salajane autoriloomingut, kasutage funktsiooni `azure keyvault set-policy` käsk.

Näiteks kui teie vault nimi on ContosoKeyVault ja te soovite lubada rakendus on Kliendiid 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, ja soovite lubada dekrüptida ning logige sisse oma võlvkelder abil käivitage järgmist:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Kui kasutate opsüsteemi Windows Käsuviip, peaksite asendamine ülakomadega jutumärkidega ja pääseda ka sisemise jutumärkidega. Näide: "[\"dekrüptida\",\"Logi\"]".

Kui soovite lubada sama rakenduse lugeda saladusi oma võlvkelder, käivitage järgmised:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Kui soovite kasutada riistvara turvalisus mooduli (HSM) ##

Lisatud assurance, saate importida või riistvara turvalisus moodulid (HSMs), mis jäta HSM äärist võtmed. Funktsiooni HSMs on FIPS 140-2 tase 2 kinnitatud. Kui see nõue ei kehti, selle sammu vahele ja minge [tootenumbri vault kustutamine ja seotud võtmed ja saladused](#delete-the-key-vault-and-associated-keys-and-secrets).

Need HSM kaitstud võtmed loomiseks peab teil olema vault tellimus, mis toetab HSM kaitstud võtmed.

Selle keyvault loomisel lisada parameeter "sku":

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Saate selle vault tarkvara kaitstud klahvid (nagu on näidatud varem) ja HSM kaitstud võtmed lisada. Mõne HSM kaitstud vajutamisega loomiseks sihtkoha parameeter "HSM" seadmiseks tehke järgmist.

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Saate importida klahvi .pem faili arvutisse järgmine käsk. See käsk impordib klahvi Vault teenus HSMs võti:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Järgmise käsu impordi "tuua oma klahv" (BYOK) paketi. Nii saate luua tootevõtit teie kohalikku HSM ja selle üle HSMs klahvi Vault teenus, ilma jättes HSM äärist võti:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Veel üksikasjalikud juhised selle kohta, kuidas luua selle BYOK paketi kohta leiate teemast [Azure klahvi Vault koos HSM-Protected abil](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Võtme vault ja seotud võtmed ja saladusi kustutamine

Kui te enam ei vaja, et klahv vault ja klahvi või salajane, mis sisaldab seda, saate kustutada võtme vault Azure'i keyvault kustutamine käsu abil:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Või saate kustutada kogu Azure ressursi rühma, mis sisaldab olulisi hoidla ja muud ressursid, mida te sellesse rühma.

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Muud Azure platvormidel käsurea liides käsud

Muud käsud, et teil võib kasu haldamise Azure'i klahvi Vault.

See käsk on loetletud tabeli kuvamine kõigi võtmed ja valitud atribuudid:

    azure keyvault key list --vault-name 'ContosoKeyVault'

See käsk kuvab täieliku loendi atribuutide määratud võti:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

See käsk on loetletud tabeli kuvamine kõigi salajane nimed ja valitud atribuudid:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Siin on näide sellest, kuidas eemaldada teatud klahvi:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Siin on näide sellest, kuidas eemaldada teatud salajane:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Järgmised sammud

Viited kavandamiseks leiate [Azure'i klahvi Vault arendaja juhendist](key-vault-developers-guide.md).
