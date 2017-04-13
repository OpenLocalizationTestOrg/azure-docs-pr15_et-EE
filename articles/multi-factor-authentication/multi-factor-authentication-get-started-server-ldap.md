<properties 
    pageTitle="LDAP autentimis- ja Azure Mitmikautentimise Server"
    description="See on Azure mitmekordne autentimine leht, mis aitab LDAP autentimis- ja Azure mitmekordne autentimine serveri juurutamine."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP autentimis- ja Azure Mitmikautentimise Server


Vaikimisi Azure'i mitme teguriga autentimine Server on konfigureeritud importida või kasutajate Active Directory sünkroonimine. Siiski saate konfigureeritud eri LDAP kataloogid, nagu ADAM kataloogi või kindla Active Directory domeenikontrolleri siduda. Ühenduse kaudu Lightweight Directory konfigureerimisel saab konfigureerida Server Azure'i mitmekordne autentimine LDAP-puhverserver teha isikutuvastusi tegutseda. See võimaldab kasutamine LDAP siduda RADIUS target, kasutajate IIS-i autentimist kasutades või esmane autentimine Azure'i mitme teguri autentimist kasutaja portaalis.

Azure'i Mitmikautentimise kasutamisel on LDAP puhverserveri nimega lisatakse Server Azure'i mitmekordne autentimine mitmikautentimise lisamiseks LDAP kliendi (nt VPN seadme, rakenduse) ja Lightweight directory serveri vahel. Azure'i Mitmikautentimise funktsiooni, tuleb konfigureerida Server Azure'i mitmekordne autentimine suhelda nii kliendi serverite ja Lightweight directory. Selle konfiguratsiooni puhul Server Azure'i mitme teguriga autentimine aktsepteerib LDAP-päringud kliendi serveritest ja rakendused ja edastab need target Lightweight directory server esmane mandaadi kinnitamiseks. Kui vastus Lightweight Directory näitab, et nad esmane identimisteave on lubatud, Azure'i Mitmikautentimise sooritab teise autentimine ja saadab vastuse tagasi LDAP kliendi. Kogu autentimine õnnestub ainult juhul, kui nii autentimise LDAP server ja selle mitmikautentimise õnnestub.





## <a name="ldap-authentication-configuration"></a>LDAP autentimise konfigureerimine


LDAP autentimise konfigureerimine installige Windows Server Server Azure'i mitme teguri autentimist. Kasutage järgmist toimingut:

1. Jooksul Server Azure'i mitmekordne autentimine vasakpoolsest menüüst ikooni LDAP autentimist.
2. Märkige ruut Luba LDAP autentimist.![LDAP autentimine](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Menüü kliendid muuta TCP port ja SSL-i pordi kui Azure mitmekordne autentimine LDAP teenust tuleks siduda mittestandardsed pordid kuulata LDAP päringute kliendid, mis konfigureeritakse.
4. Kui kavatsete kasutada LDAPS Azure'i mitme teguriga autentimine serveriga klient, SSL-serdi peab olema installitud Server Server töötab. Klõpsake nuppu Sirvi... SSL-i serdi karp- ja valige installitud sert, mida kasutatakse turvalist ühendust kõrval nuppu.
5. Klõpsake nuppu Lisa... nupp.
6. Sisestage dialoogiboksis lisamine LDAP kliendi seadme, server või rakendus, mis kuvatakse autentida IP-aadress Server ja rakenduse nimi (valikuline). Rakenduse nimi kuvatakse Azure'i Mitmikautentimise aruandeid ja võidakse kuvada SMS või mobiilirakenduse autentimise sõnumeid.
7. Märkige ruut nõua Azure'i Mitmikautentimise kasutaja match, kui kõik kasutajad on või imporditakse server ja mutli autentimine. Kui palju kasutajad pole veel imporditud Server ja/või vabastamine mutli autentimine, jätke ruut märkimata. Vt selle funktsiooni kohta lisateabe saamiseks spikrifaili.
8. Võib-olla korrake toiminguid 5 – 7 lisada täiendavad LDAP kliendid.
9. Azure'i Mitmikautentimise konfigureerimisel LDAP isikutuvastusi vastuvõtmiseks tuleb puhverserveri nende isikutuvastusi Lightweight Directory. Seetõttu on üks, kuvab ainult menüü Target tuhm võimalus kasutada mõne LDAP Target (sihtkoht). Lightweight directory ühenduse konfigureerimiseks klõpsake kataloogi integreerimise ikooni.
10. Valige vahekaardil Sätted raadionuppu Kasuta teatud LDAP konfigureerimine.
11. Klõpsake nuppu Redigeeri... nupp.
12. Dialoogiboksis Redigeeri LDAP konfiguratsiooni väljad asustada Lightweight directory ühenduse loomiseks vajalik teave. Väljade kirjelduste sisalduvad järgmises tabelis. Märkus: See teave on ka Azure mitmekordne autentimine serveri spikrifaili.![Kataloogi integreerimise](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. LDAP ühenduse testimiseks klõpsake nuppu testi.
14. Kui LDAP ühenduse test õnnestus, klõpsake nuppu OK.
15. Klõpsake vahekaardil Filtrid. Server on eelkonfigureeritud laadimiseks ümbriste, turberühmad ja kasutajate Active Directoryst. Kui sidumine eri Lightweight directory, peate tõenäoliselt filtreid, kuvatakse redigeerida. Filtrite kohta lisateabe saamiseks linki spikker.
16. Klõpsake vahekaarti atribuudid. Server on eelkonfigureeritud vastendamiseks atribuutide Active Directoryst.
17. Kui Köitmine erinevate Lightweight directory või muuta eelnevalt konfigureeritud atribuudi vastendused, klõpsake nuppu Redigeeri... nupp.
18. Muutke dialoogiboksis Redigeeri atribuute LDAP atribuudi vastendused kataloogi jaoks. Atribuut nimesid saate tipitud või valitud, klõpsates soovitud... iga välja kõrval olevat nuppu.
19. Atribuutide kohta lisateabe saamiseks linki spikker.
20. Klõpsake nuppu OK.
21. Klõpsake ikooni ettevõtte sätted ja valige vahekaart kasutajanimi eraldusvõimet.
22. Kui ühenduse Active Directory domeeni ühendatud serverist, peaks oskama Kasuta Windowsi turvalisus identifikaatorite (sid) sobitamiseks valitud kasutajanimede raadionupp jätta. Muul juhul valige kattuvad kasutajanimed nupp atribuut kasutamine Lightweight kordumatut tunnust. Kui see on valitud, proovib Server Azure'i mitme teguriga autentimine lahendamiseks iga kasutajanimi Lightweight Directory kordumatu. LDAP-otsing tehakse kasutajanimi kataloogi integreerimise määratletud atribuute -> atribuudid vahekaardil. Kui kasutaja autendib, kasutajanimi on otsustanud ainuidentifikaator Lightweight Directory ja kordumatut tunnust kasutatakse sobitamine kasutaja Azure'i Mitmikautentimise faili. See võimaldab väiketähed võrdlustes nagu pikk ja lühike kasutajanimi vormindab selle lõpetab Azure'i mitmekordne autentimine serveri konfiguratsioon. Server on nüüd kuulamise konfigureeritud pordid LDAP Accessi päringute konfigureeritud klientide ja nende taotluste autentimise Lightweight directory puhverserveri määratud.


## <a name="ldap-client-configuration"></a>LDAP kliendi konfiguratsiooni

LDAP kliendi konfigureerimiseks silmas juhiseid.

- Konfigureerige oma seade, server või rakenduse kaudu LDAP server Azure'i mitmekordne autentimine autentida, nagu oleks teie Lightweight directory. Kasutage samu sätteid, mis Lightweight directory, välja arvatud serveri nimi või IP-aadress, mis on, mis Server Azure'i mitmekordne autentimine loomiseks kasutada.
- Konfigureerige LDAP ajalõpp 30 – 60 sekundi nii, et on valideerimiseks kasutajatunnust Lightweight Directory, teise teguri autentida, saata oma vastus ja seejärel LDAP juurdepääsutaotluse aega.
- Kui LDAPS, seadme või serveri LDAP päringute tegemise peab usalda installitud Server Azure'i mitmekordne autentimine SSL-sert.
