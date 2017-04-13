<properties
   pageTitle="Loogika rakendustes HTTP kuulajale ja Connectori abil | Microsoft Azure'i rakendust Service "
   description="Kuidas luua ja HTTP kuulajale ja HTTP toimingu konnektor või API rakenduse konfigureerimine ja kasutamine loogika rakenduse teenuses Azure rakendus"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>HTTP kuulajale ja HTTP toimingu kasutamise alustamine ja lisada selle oma loogika rakenduse

> [AZURE.NOTE]Me on selle konnektori tugi lõpeb, kuna selle funktsionaalsus on nüüd vaikimisi kaasatud nimega **käsitsi käivitada** uue loogika rakenduste loomisel.  Soovitame kõik teie loogika rakendused, mis on selles kasutamisega uuendada.  
> Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon.

Ühendage otse kuulata HTTP-päringud ja konfigureerida HTTP web nõuab HTTP ressursid. On mõnel juhul, kui võimalik, et peate töötada otse HTTP ühendused, sh:

1.  Arendada loogika rakendus, mis toetab veebis või mobiiltelefoni kasutaja interaktiivsed esiosa.
2.  Saada ja töötlemine andmed veebiteenusest, mis ei sisalda väljale konnektor kontorist väljas.
3.  Toiminguid, mis on juba avatud nimega veebiteenuse, kuid pole saadaval, kui rakendus API.

Stsenaariumi puhul, on kaks võimalust.

1. **HTTP kuulajale**: selle konnektori toimib käivitamiseks ja kuulab konfigureeritud lõpp-punkti HTTP-päringud. Kui kõne on konfigureeritud lõpp-punkti saanud, käivitab uue eksemplari voogu ja edastab taotluse töötlemiseks liikumise saadud andmeid. See saab konfigureerida sissetuleva taotlusele automaatselt vastata, kui voogu on võtnud või teile vastuse paanivoo täitmise põhjal koostada ja saada vastuse funktsiooni helistaja.
2. **HTTP toimingu**: See laseb teil konfigureerida web taotluse mis tahes lõpp-punkti saadaval Internetis, saab tagasi vastuse ja teeb selle kättesaadavaks täiendavad toimingud voogu kasutamine.

Loogika rakenduste käivitab põhjal erinevate andmeallikate ja pakkumise konnektorid saada ja töötlemine andmete voogu osana. Saate lisada töövoog ja protsessi äriteabe osana töövoo loogika rakenduses HTTP konnektor. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Mõne HTTP kuulajale oma loogika rakenduse loomine
Konnektor saate luua loogika rakenduses või Azure'i turuplatsilt. Turuplatsilt konnektori loomiseks tehke järgmist.  

1. Azure'i startboard, valige **turuplatsilt**.
2. Otsige "HTTP", valige HTTP kuulajale ja valige **Loo**.
3.  Konfigureerige HTTP kuulajale järgmiselt:  
![][1]

4.  Paketi sätted häälestamisel kuvatakse järgmine suvand kas kuulajale tuleks automaatselt vastata või nõuavad konkreetsete vastuse saatmiseks. Määrata selle väärtuseks **False** oma vastuse saata.  
![][2]

5.  Klõpsake nuppu **OK** , et luua.
6.  Kui API rakenduse eksemplari on loodud, avage sätete konfigureerimiseks turvalisus. HTTP kuulajale toetab praegu Lihttekstautentimine. Saate konfigureerida selle suvandiga turvalisus HTTP kuulajale avamisel.  
![][3]
  
    **Teadaolev probleem** *The turvalisus sätete kuva "Pole" vaikeväärtus, aga see on määramata. Peate muutma selle sätte Basic ja tagasi pole enne salvestamist HTTP kuulajale on õigesti konfigureeritud.*  

7. Lõpetuseks seatud turbesätted API rakenduse avalik (anonüümne) väliste klientide lõpp-punkti juurdepääsu lubamiseks. See säte on saadaval jaotises "kõik sätted > rakenduse sätted" HTTP kuulajale API rakenduse:![][10]

Pärast seda, saate nüüd luua loogika rakenduse kasutamiseks HTTP kuulajale.

## <a name="using-the-http-listener-in-your-logic-app"></a>HTTP kuulajale loogika rakenduse kasutamine
Kui rakenduse API on loodud, saate kohe kasutada HTTP kuulajale käivitamiseks oma loogika rakenduse. Selle tegemiseks peate:

4.  Saate luua uue loogika rakenduse.
5.  Avage "Päästikute ja toimingute" avamiseks loogika rakenduste Designer ja teie meilivoo konfigureerimine. HTTP kuulajale on loetletud galeriis. Valige see.
6.  Nüüd saate määrata meetod HTTP ja kuulajale käivitamine voogu nõuda suhteline URL.  
![][4]  
![][5]

7.  Kogu URI saamiseks topeltklõpsake selle konfiguratsiooni sätete vaatamine ja kopeerige URL-i jaoks rakenduse API "Host" HTTP kuulajale:  
![][6]
8.  Nüüd saate andmeid saadud muud toimingud voogu HTTP-päringu järgmiselt:  
![][7]  
![][8]
9.  Lõpetuseks vastuse saatmiseks lisage teine HTTP kuulajale ning valige HTTP vastuse saata toiming. Määratud taotlemine ID RequestID, mis on saadud HTTP kuulajale ja asustada vastuse keha ja soovite tagastada tagasi HTTP olek:  
![][9]

## <a name="using-the-http-action"></a>HTTP toimingu abil
HTTP toimingu algupäraselt toetab loogika rakendused ja ei nõua rakenduse API luua esimese saaksid seda kasutada. Saate lisada toimingu HTTP loogika rakenduse mis tahes hetkel ja valige URI, päised ja keha kõne.
HTTP toimingu toetab kliendi side turvalisus mitu võimalust. Vt [Kliendi küljel turbesuvandid](../scheduler/scheduler-outbound-authentication.md).

HTTP toimingu väljund on päised ja keha, mida saate kasutada täpsemaks tagapool sarnane kuidas väljundit muude toimingute ja konnektorid on tarbitud voogu.

## <a name="do-more-with-your-connector"></a>Lisavõimalused oma konnektor
Nüüd, kui konnektor on loodud, saate selle lisada töövoos loogika rakenduse kasutamine. Vt [mis on loogika rakendused?](app-service-logic-what-are-logic-apps.md).

Vaadata ärplema REST API viide [konnektorid ja API rakenduste viide](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Saate vaadata ka jõudluse statistika ja juhtelemendi turvalisus konnektor. Vaadata, [hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Kui soovite alustada Azure'i loogika rakendused enne Azure'i konto kasutajaks, minge [Proovige loogika rakendus](https://tryappservice.azure.com/?appservice=logic). Saate luua kohe lühiajaline starter loogika rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
