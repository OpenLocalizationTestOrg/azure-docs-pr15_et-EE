<properties 
    pageTitle="Azure'i Mitmikautentimise Server PhoneFactor Agent versiooniks"
    description="Selles dokumendis kirjeldatakse alustada Server Azure'i MFA ja vanemad phonefactor agendi versioonile."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Azure'i Mitmikautentimise Server PhoneFactor Agent versiooniks

Täiendamine PhoneFactor Agent v5.x või vanemate Azure'i mitmekordne autentimise Server nõuab PhoneFactor Agent ja seotud komponendid desinstallida enne mitme teguri autentimise Server ja selle liitunud komponentide installimist.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Versioonile PhoneFactor Agent Azure mitme teguriga autentimine serveris
<ol>
<li>Esmalt varundada PhoneFactor andmefaili. Installi vaikeasukoht on C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Kui kasutaja portaali on installitud:</li>
<ol>
<li>Liikuge kausta, installimine ja taas üldisemaks fail. Installi vaikeasukoht on C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Kui olete lisanud kohandatud kujundused portaali, varundage oma kohandatud kausta C:\inetpub\wwwroot\PhoneFactor\App_Themes kataloogi all.</li>


<li>Desinstallige kasutaja portaali PhoneFactor Agent (ainult saadaval, kui installitud samasse serverisse PhoneFactor agendina) või kaudu Windows programmid ja funktsioonid.</li></ol>




<li>Kui veebiteenuse Mobile'i rakendus on installitud:
<ol>
<li>Minge kausta installi ja taas üldisemaks minna fail. Installi vaikeasukohaks on C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Desinstallige Mobile'i rakendus veebiteenuse kaudu Windows programmid ja funktsioonid.</li></ol>

<li>Kui Web teenuse SDK on installitud, desinstallige see PhoneFactor Agent või kaudu Windows programmid ja funktsioonid.

<li>Desinstallige PhoneFactor agendi kaudu Windows programmid ja funktsioonid.

<li>Mitmikautentimise serverit installida. Pange tähele, et Installitee on peale registrist eelmise PhoneFactor Agent installi nii, et see peaksite installima samasse asukohta (nt C:\Program Files\PhoneFactor). Uue installid on erinevate vaikimisi installida tee (nt C:\Program Files\Multi-tegur autentimise Server). Eelmise PhoneFactor agent vasakule andmefaili tuleks uuendada installimisel nii kasutajate kui ka sätted peaksid olema ikka olemas pärast installimist uus mitmekordne autentimise Server.

<li>Kui kuvatakse vastav viip, aktiveerimine mitme teguri autentimise Server ja veenduge, et see on määratud õige dispersioonanalüüs rühma.

<li>Kui Web teenuse SDK kandis varem installitud, installige uus Web SDK, mitme teguriga autentimine serveri kasutajaliidese kaudu. Pange tähele, virtuaalse directory vaikenimi on nüüd "MultiFactorAuthWebServiceSdk" asemel "PhoneFactorWebServiceSdk". Kui soovite kasutada varasema nime, peate muutma nime virtual directory installimise ajal. Muul juhul, kui lubate kasutada uut vaikenime installi, on teil muuta rakendustes, mis viitavad Web teenuse SDK nagu kasutaja portaali ja Mobile rakenduse veebiteenuse osutama õige asukoha URL.

<li>Kui kasutaja portaali oli varem installitud PhoneFactor Agent Server, installige uus mitme teguri autentimist kasutaja portaali mitmekordne autentimine serveri kasutajaliidese kaudu. Pange tähele, virtuaalse directory vaikenimi on nüüd "MultiFactorAuth" asemel "PhoneFactor". Kui soovite kasutada varasema nime, peate muutma nime virtual directory installimise ajal. Muul juhul, kui lubate installimiseks kasutada uut vaikenime, peaks mitme teguriga autentimine serveri kasutaja portaali ikooni ja värskendada kasutaja portaali URL-i vahekaardil sätted.

<li>Kui kasutaja portaali ja/või Web Appi mobiiliteenuse oli varem installitud PhoneFactor agendi eri server:
<ol>
<li>Minge installi asukohta (nt C:\Program Files\PhoneFactor) ja muude server vastav installer(s) kopeerida. On 32-bitine ja 64-bitine paigaldajad kasutaja portaali ja Mobile rakenduse veebiteenuse. Neid nimetatakse MultiFactorAuthenticationUserPortalSetupXX.msi ja MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi vastavalt.</li>
<li>Kasutaja portaali veebiserverisse installimiseks avage administraatorina käsuviiba ja käivitage selle MultiFactorAuthenticationUserPortalSetupXX.msi. Pange tähele, virtuaalse directory vaikenimi on nüüd "MultiFactorAuth" asemel "PhoneFactor". Kui soovite kasutada varasema nime, peate muutma nime virtual directory installimise ajal. Muul juhul kui lubate installimiseks kasutada uut vaikenime, peaks mitme teguri autentimise Server ikooni kasutaja portaali ja värskendada kasutaja portaali URL-i vahekaardil sätted. Olemasolevatele kasutajatele on vaja asjaga kursis püsida uus URL-i.</li>
<li>Minge portaali kasutaja installida asukoht (nt C:\inetpub\wwwroot\MultiFactorAuth) ja redigeerida fail. Kopeerige oma algse fail oli enne uuendamist varundada sisse uus fail appSettings ja applicationSettings jaotised väärtused. Kui uus virtuaalne kaust vaikenimi on hoida Web teenuse SDK installimisel, muuta URL-i applicationSettings jaotises Valige õige koht. Kui mõni muu vaikesätted muudetud eelmise fail, rakendada sama muudatuste uus fail.</li>
<li>Veebiserver veebiteenuse Mobile rakenduse installimiseks avage administraatorina käsuviiba ja käivitage selle MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Pange tähele, virtuaalse directory vaikenimi on nüüd "MultiFactorAuthMobileAppWebService" asemel "PhoneFactorPhoneAppWebService". Kui soovite kasutada varasema nime, peate muutma nime virtual directory installimise ajal. Kui soovite valida lihtsustab lõppkasutajal mobiilsideseadmetes tippige lühem nimi. Muul juhul kui lubate installimiseks kasutada uut vaikenime, peaks mitme teguri autentimise Server ikooni Mobile'i rakendus ja värskendada Mobile'i rakendus veebiteenuste URL.</li>
<li>Avage rakenduse Web mobiilsideteenuse installida asukoht (nt C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) ja redigeerida fail. Kopeerige oma algse fail oli enne uuendamist varundada sisse uus fail appSettings ja applicationSettings jaotised väärtused. Kui uus virtuaalne kaust vaikenimi on hoida Web teenuse SDK installimisel, muuta URL-i applicationSettings jaotises Valige õige koht. Kui mõni muu vaikesätted muudetud eelmise fail, rakendada sama muudatuste uus fail.</li></ol>
