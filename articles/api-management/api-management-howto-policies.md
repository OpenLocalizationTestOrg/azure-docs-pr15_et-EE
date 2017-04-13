<properties 
    pageTitle="Azure'i API haldamise poliitikate | Microsoft Azure'i" 
    description="Saate teada, kuidas luua, redigeerida ja API haldamise poliitikate konfigureerimine." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


#<a name="policies-in-azure-api-management"></a>Azure'i API haldamise poliitikate

Azure'i API haldus poliitika on võimas võimalus süsteemi, mis võimaldavad avaldaja, kelle soovite muuta serveri käitumist API konfigureerimise kaudu. Poliitika on kogumi laused, mis täidetakse järjest taotluse või vastuse API. Populaarsed kaasata vorming teisendamise XML-ist JSON ja kõne määr piiramise piiramise sissetulevate kõnede arendaja summa. Palju veel poliitikad on saadaval välja kasti.

[Poliitika viide][] poliitika laused ja nende sätete täieliku loendi leiate teemast.

Lüüsi, mis asub API tarbija hallatav API sees on rakendatud poliitika. Lüüsi saab kõik kutsed ja tavaliselt edastab need aluseks API muutmata kujul. Siiski saate taotluse sissetuleva ja väljamineva vastuse poliitika muudatusi rakendada.

Poliitika avaldiste saate kasutada atribuudiväärtused ega tekstväärtusi mis tahes API teabehalduspoliitikate juhul, kui poliitika määrab teisiti. Mõned poliitika, näiteks [Control flow][] ning [määrata muutuja][] poliitikate põhinevad poliitika avaldised. Lisateavet leiate teemast [Täpsemad poliitika][] ja [poliitika avaldised][].

## <a name="scopes"> </a>Poliitikate konfigureerimine
Poliitika saab konfigureerida globaalselt või [toode][], [API][] või [toimingu][]ulatust. Liikuge poliitika konfigureerimiseks poliitikate redaktori Publisheri portaalis.

![Poliitika menüü][policies-menu]

Poliitika redaktori koosneb kolmest põhipeatükki: poliitika ulatuse (üleval), kui poliitika on redigeeritud (vasakul) ja aruannete loend (paremal) poliitika määratluse:

![Poliitika redaktor][policies-editor]

Poliitika konfigureerimise alustamiseks valige esmalt ulatus, mille poliitika rakendada. Pildil **Starter** toode on valitud. Pange tähele, et poliitika nime kõrval ruudu sümbol näitab, et poliitika on juba rakendatud samal tasemel.

![Ulatus][policies-scope]

Kuna juba rakendatud poliitika, kuvatakse konfiguratsiooni vaates määratlus.

![Konfigureerimine][policies-configure]

Poliitika kuvatakse kirjutuskaitstud alguses. Redigeerimiseks klõpsake määratluse **Konfigureerimine poliitika** toimingut.

![Redigeerimine][policies-edit]

Poliitika määratlus on lihtne XML-dokumendi kirjeldav sissetulevate ja väljaminevate laused jada. XML-i saab redigeerida aknas määratlus. Loendi laused on esitatud paremale ja kohaldatavaid praegune ulatus on lubatud ja esile tõstetud; nagu pildil oleva **Limiit kõne määr** teatise.

Klõpsake lauset lubatud lisada vastav XML-i määratlus vaates kursori asukoha. 

>[AZURE.NOTE] Kui poliitika, mille soovite lisada pole lubatud, veenduge, et teil on õige ulatuse poliitika. Iga poliitika lause on mõeldud kasutamiseks teatud otsinguulatuste ja poliitika jaotised. Poliitika jaotised ja otsinguulatuste poliitika läbi lugege **kasutus** jaotises poliitika [Poliitika viide][].

Poliitika laused täieliku loendi ja nende sätteid on saadaval [Poliitika viide][].

Näiteks lisada uue lause piirata sissetulevad taotlused määratud IP-aadressid, viige kursor lihtsalt sees sisu on `inbound` XML-elemendi ja klõpsake nuppu **piira helistaja IP-d** lause.

![Piirang poliitika][policies-restrict]

Selle mõne XML-i koodilõigu, et selle `inbound` element, mis pakub juhised selle kohta, kuidas konfigureerida lause.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Kui soovite piirata sissetulevad taotlused ja nõustuge ainult need 1.2.3.4 IP-aadress kaudu muuta XML-i järgmiselt:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Salvestamine][policies-save]

Kui täielik konfigureerimise laused poliitika, klõpsake nuppu **Salvesta** ja muudatused kuvatakse olevatesse API Management gateway kohe.

##<a name="sections"> </a>Mõistmine poliitika konfigureerimine

Poliitika on laused, mis selleks, et päringu ja vastuse käivitada rida. Konfiguratsiooni on jagatud õigesti `inbound`, `backend`, `outbound`, ja `on-error` sections, nagu on näidatud järgmisel konfiguratsioon.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Kui ilmneb tõrge taotluse töötlemise ajal, mis tahes ülejäänud etappide on `inbound`, `backend`, või `outbound` jaotised on vahele ja täitmise viib oleva funktsiooni `on-error` jaotis. Poliitika laused sisse pannes selle `on-error` jaotise abil saate vaadata tõrke soovitud `context.LastError` atribuudi, lähemalt uurida ja kohandada tõrge vastuse abil soovitud `set-body` poliitika, ja konfigureerida, mis juhtub, kui ilmneb tõrge. On tõrkekoodid sisseehitatud juhised ja tõrkeid, mis võivad tekkida poliitika laused töötlemise ajal. Lisateabe saamiseks vt [API teabehalduspoliitikate käsitsemine tõrge](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Poliitika saab määrata eri tasanditel (üld-, toode, api ja käitamine) konfiguratsioon võimaldab teil määrata järjestuses, milles poliitika määratlus laused käivitada ema poliitikale. 

Poliitika otsinguulatuste hinnatakse järgmises järjestuses.

1. Globaalne ulatus
2. Toote ulatus
3. API ulatus
4. Toiming ulatus

Laused neis hinnatakse paigutamine vastavalt selle `base` element, kui see on olemas.

Näiteks, kui teil on konfigureeritud lubama API poliitika ja globaalse tasemel poliitika, siis iga kord, kui kasutatakse selle kindla API nii poliitikate rakendatakse. API haldus võimaldab tarkadeks tellimine ja ühendatud poliitika base elemendi kaudu. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

Eeltoodud näites poliitika määratlemisel on `cross-domain` lause soovite käivitada enne kõrgema poliitikad, mis omakorda järgneb selle `find-and-replace` poliitika.

Kui sama poliitika kuvatakse kaks korda poliitika lause, kõige viimati hinnatud poliitika rakendamist. Selle abil saate alistada poliitika, mis on määratletud kõrgema ulatuses. Poliitikate praeguses ulatuses poliitika redaktoris kuvamiseks klõpsake **ümberarvutamine poliitikat valitud ulatust**.

Pange tähele, et globaalne poliitika pole ema poliitika ja abil soovitud `<base>` elemendi see ei mõjuta. 

## <a name="next-steps"></a>Järgmised sammud

Vaadake videot poliitika avaldisi pärast.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Poliitika viide]: api-management-policy-reference.md
[Toote]: api-management-howto-add-products.md
[API-GA]: api-management-howto-add-products.md#add-apis 
[Toiming]: api-management-howto-add-operations.md

[Täiustatud poliitika]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Muutuja seadmine]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Poliitika avaldised]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
