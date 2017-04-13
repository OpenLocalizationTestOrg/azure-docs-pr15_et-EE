<properties
  pageTitle="Azure'i asjade komplekti ja Azure Active Directory | Microsoft Azure'i"
  description="Kirjeldab, kuidas Azure'i asjade komplekti kasutab Azure Active Directory õiguste haldamiseks."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Azureiotsuite.com saidiõigused

## <a name="what-happens-when-you-sign-in"></a>Mis juhtub, kui logite sisse

Esmakordsel sisselogimisel veebisaidil [azureiotsuite.com][lnk-azureiotsuite], saidi määratleb õigusetasemed, mis on praegu valitud Azure Active Directory (AAD) rentniku ja Azure tellimuse põhjal.

1.  Saidi esmalt leiab Azure'i mis AAD rentnikud kuulute selleks, et asustada rentnikud sisse loginud kasutajanimi näha loendit. Sel ajal, saidi ainult hankida kasutaja sõned ühe rentniku korraga. Selle tulemusena paremas ülanurgas ripploendi kasutamine eri rentniku vahetamisel saidi uuesti logib teid sellele rentnikule märkide saamiseks selle rentniku jaoks.

2.  Järgmiseks saidi leiab, millised tellimused on seostatud valitud rentniku Azure. Kuvatakse saadaolevate tellimuste, kui loote uue eelkonfigureeritud lahenduse.

3.  Lõpuks saidi toob kõik tellimused ja ressursside rühmas ressursside sildistatud eelkonfigureeritud lahendusi ja kuvab avalehel paanid.

Järgmistes jaotistes kirjeldatakse rollid, mis juhib juurdepääsu eelkonfigureeritud lahendusi.

## <a name="aad-roles"></a>AAD rollid

AAD rollid määrata võimalus sätte eelkonfigureeritud lahenduste ja eelkonfigureeritud lahenduse kasutajate haldamine.

Leiate lisateavet administraatorirollid AAD rakenduses [administraatorirollide Azure AD][lnk-aad-admin], kuid see artikkel keskendub peamiselt **Üldadministraator** ja **Domeeni kasutaja/liige** rollid, mis eelkonfigureeritud lahenduste.

**Globaalne administraator:** Ei saa mitme globaalne administraatorite AAD rentniku kohta. Kui loote rentnikku AAD, olete vaikimisi globaalne administraator selle rentniku. Globaalne administraator saab ettevalmistamise eelkonfigureeritud lahenduse ja on määratud **administraatorirolli rakenduse oma AAD rentniku sees** . Kui teise kasutaja AAD samal rentnikukontol loob rakenduse, vaikimisi roll globaalne administraator on andnud aga **Ainult peidetud lugeda**. Globaalse administraatorid saavad määrata rollid rakenduste [Azure klassikaline portaali][lnk-classic-portal].

**Domeeni kasutaja/liige:** Ei saa mitme domeeni kasutajad/liikmete AAD rentniku kohta. Domeeni kasutaja saab ettevalmistamise eelkonfigureeritud lahenduse [azureiotsuite.com] [ lnk-azureiotsuite] saidi. Need on antud need ettevalmistamise rakenduse vaikimisi on **administraator**. Nad saavad luua kasutav build.cmd skripti [asjade azure remote jälgimise] [ lnk-rm-github-repo] või [azure-asjade Interneti-sõnastikupõhise-hooldustööd] [ lnk-pm-github-repo] hoidla, kuid need on antud roll vaikimisi on **Peidetud KIRJUTUSKAITSTUD**, nagu nad ei saa õigusi määrata rollid. Kui teise kasutaja AAD rentniku loob rakenduse, nad on määratud **Peidetud KIRJUTUSKAITSTUD** roll vaikimisi rakenduse. Neil on võimalus määrata rakenduste jaoks; Seetõttu ei saa lisada kasutaja või kasutajate rollid kasutajate jaoks rakenduse isegi juhul, kui nad selle ette valmistatud.

**Külastajate kasutaja/Külaline:** Ei saa palju Külastajate kasutajate/külalisi AAD rentniku kohta. Külalisena kasutajatel on piiratud õiguste kogumi AAD rentnik. Seetõttu ei saa külalisi ette eelkonfigureeritud lahenduse AAD rentnik.

Lisateavet järgmistest teemadest:

- [Kasutajate loomine ja redigeerimine Azure AD][lnk-create-edit-users]
- [Rakenduse rollide AAD määramine][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure'i tellimuse Administraatorirollid

Azure'i administraatorirollide määrata võimalus Vastendage tellimuse Azure AD rentnikku.

Leiate lisateavet artiklist [Azure'i koostöö administraator, teenuse administraator ja administraatori konto lisamiseks või muutmiseks tehke järgmist]Azure'i koostöö administraator, teenuse administraator ja konto administraatori rollid[lnk-admin-roles].

## <a name="application-roles"></a>Rakenduse rollid

Rakenduse rollide eelkonfigureeritud lahendusse seadmete juurdepääsu reguleerimine.

On kaks määratletud ja ühe peidetud rolliga määratletud rakenduses, mis luuakse siis, kui olete ette eelkonfigureeritud lahenduse.

-   **Administraator:** On täielik kasutusõigus lisada, hallata ja seadmete eemaldamine

-   **KIRJUTUSKAITSTUD:** On võimalus vaadata seadmed

-   **Peidetud KIRJUTUSKAITSTUD:** See on sama mis kirjutuskaitstud, kuid on antud teie AAD rentniku kõigile kasutajatele. Seda tehti mugavuse arendamise käigus. Saate eemaldada selle rolli, muutes [RolePermissions.cs] [ lnk-resource-cs] lähtefail.

### <a name="changing-application-roles-for-a-user"></a>Kasutaja rakenduse rollide muutmine

Saate järgmiste toimingute tegemiseks kasutaja oma Active Directory administraator teie eelkonfigureeritud lahendus.

AAD üldadministraator kasutaja rollid muutmiseks peate olema:

1. Avage [Azure'i klassikaline portaali][lnk-classic-portal].

2. Valige **Active Directory**.

3. Klõpsake selle nime oma AAD rentniku (see on teie valitud azureiotsuite.com, kui teil on ette valmistatud teie lahendus kataloogi).

4. Klõpsake nuppu **rakendused**.

5. Klõpsake rakenduse, mis vastab teie eelkonfigureeritud lahenduse nimi nimi. Kui te ei näe teie rakenduste loendis, aktiveerige **minu ettevõte kuulub rakenduste** **kuvamine** rippmenüüst ja seejärel klõpsake märkeruutu.

7. Klõpsake nuppu **Kasutajad**.

8. Valige kasutaja, millele soovite üle minna rollid.

9. Klõpsake nuppu **Määra** ja valige roll (nt **administraator**), mida soovite määrata kasutajale, seejärel klõpsake märkeruutu.

## <a name="faq"></a>KKK

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ma olen teenuse administraator ja mul soovite muuta oma tellimust ja teatud AAD rentniku directory kaardistamine. Kuidas teha?

1. Minge [Azure'i klassikaline portaali][lnk-classic-portal], klõpsake nuppu **sätted** vasakus servas teenuste loend.

2. Valige tellimus, mida soovite muuta directory vastendus.

3. Klõpsake nuppu **Redigeeri Directory**.

4. Valige rippmenüüst kasutada soovite **kataloogi** . Klõpsake noolt edasi.

5. Veenduge, et kataloogi vastendust ja mõjutatud kaasadministraatorite. Pange tähele, et kui teil liiguvad teise kataloogist, eemaldatakse kõik kaasadministraatorite algse kataloogist.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ma olen domeeni kasutaja/liige AAD rentnikus ja on loodud eelkonfigureeritud lahenduse. Kuidas teha saada määratud rolli minu rakenduse?

Paluge üldadministraator, saate määrata üldadministraatorina AAD rentnikus saada õigusi määrata kasutajatele rollid ise või paluge üldadministraator, saate määrata rolli. Kui soovite muuta AAD rentniku teie eelkonfigureeritud lahendus kasutusele võetud, leiate järgmise küsimuse.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Kuidas aktiveerida AAD rentniku minu järelevalve Remote'i eelkonfigureeritud lahendus ja rakenduse määratakse?

Saate käivitada pilve juurutamise <https://github.com/Azure/azure-iot-remote-monitoring> ja Juurutage uuesti vastloodud AAD rentnikuga. Kuna kasutate vaikimisi üldadministraator, kui loote uue rentniku AAD, on teil juurdepääsu kasutajate lisamine ja nende kasutajate ja rollide määramine.

1. Looge uus AAD kaust [Azure haldusportaali][lnk-classic-portal].

2. Avage <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Käivitage `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (nt `build.cmd cloud debug myRMSolution`)

4. Kui küsitakse, määrake **tenantid** olema oma äsja loodud rentnikukonto asemel teie eelmise rentniku.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Soovin muuta administraator või koostöö administraator, kui organisatsiooni kontoga sisse loginud

Tugiteenuste artiklist [muutmise teenuse administraator ja koostöö administraator, kui organisatsiooni kontoga sisse loginud][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Miks kuvatakse see tõrketeade? "Teie konto on asjakohased õigused lahenduse loomiseks. Kontrollige oma konto poole või proovige mõne muu kontoga."

Heitke pilk alloleval joonisel:

![][img-flowchart]

> [AZURE.NOTE] Kui näete viga peale teie üldadministraator AAD rentnikus ja koostöö administraator tellimust, on teie administraator kasutaja eemaldamine ja uuesti määrata vajalikud õigused selles järjestuses: üldadministraatorina kasutajat lisada, ja seejärel lisage kasutaja Azure tellimuse koostöö administraatorina. Kui probleemid ei kao, pöörduge [Spikker ja tugi][lnk-help-support].

**Miks ma näen selle tõrke korral Azure tellimuse?** *Azure'i tellimuse on vaja luua eelnevalt konfigureeritud lahendusi. Saate luua tasuta prooviversiooni konto vaid paar minutit.*

Kui olete kindel, et teil on Azure tellimuse valideerimiseks vastendamise tellimuse rentniku ja veenduge, et õige rentniku on valitud ripploendi. Kui te olete kinnitatud soovitud Rentnik on õige, järgige ülaltoodud diagrammi ja kinnitage oma tellimust ja selle AAD rentniku vastendust.

## <a name="next-steps"></a>Järgmised sammud

Rohkem õppematerjale asjade komplekti kohta, lugege teemat Kuidas saate [kohandada eelkonfigureeritud lahenduse][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
