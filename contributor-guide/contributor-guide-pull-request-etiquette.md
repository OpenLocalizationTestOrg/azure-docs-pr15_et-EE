# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Microsoft Azure'i dokumentatsiooni panuse tõmba taotluse etikett ja head tavad

Muudatuste avaldamiseks dokumentatsioon, saadate pull taotlusi toidulauani. Iga pull taotlus on enne ühendatavad läbi. Lugege seda artiklit mõista, kuidas peaks töötama pull taotluse läbivaatajad ja kuidas saate luua pull taotlusi mis on lihtsam ja kiirem üle vaadata nii, et tõmmata taotluse järjekorda töötab paremini kõigi kasutajate jaoks.

## <a name="working-with-pull-request-reviewers"></a>Pull taotluse läbivaatajad töötamine

Siit leiate altpoolt peate teadma pull taotluse läbivaatajad töötamise kohta. 

- <b>Mõista pull taotluse läbivaataja roll. Läbivaataja:</b>
  - Tagab põhilised kvaliteedi sisu
  - Takistab regressioon hoidla
  - Annab tagasisidet enne ühendamine

  Pull taotluse läbivaatajad on sisu juhtimise roll. Peamine eesmärk on mitte lihtsalt ühendada, mis on esitatud nii kiiresti kui võimalik. Oodatud tagasiside, mis teil vaja teha värskendusi, eriti artikleid uute ja oluliselt muudetud.

- <b>Plaanima koos oma pull taotluse läbivaataja:</b>
  - Kõrge tähtsusega pull taotluste
  - Pull taotlusi väljaannete ajastatud/kuupäevaga
  - Pull nõuab, et muuta või lisada palju faile

- <b>SLA tõmmake taotlemine läbivaatus</b>

  Privaatne hoidlas, iga kord, kui teie taotlus pull sisestab pull taotluse järjekorda sildiga valmis ja ühendamine meeskond proovib läbi pull taotluse jooksul 12 tööaega (E – R, 8-17.00) ja tagasiside või kui tagasisidet pole on vaja ühendada. See SLA kehtib tegu läbivaatamise PR, ei saa seda ühendada. PRs koostesse kaasata, kui nad vastavad [kriteeriumid ühendamise](contributor-guide-pr-criteria.md). 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Veenduge, et tõmmata taotluse järjekorda paremini kõigi kasutajate jaoks

On kaks tavaline olukorda PR järjekorda.

- Pull taotlusi, mis on väikesed ulatuse ja üsna sarnased muudatused, mis sisaldavad vähem aega üle vaadata. 
- Pull taotlusi, mis on suur ulatus või muu kombineeritud tüüpi muudatused, mis sisaldavad võtta rohkem aega üle vaadata.

Saate aitavad muuta paremini töötada nende heade tavade järgimine pull taotluse järjekorda.

- Eraldi mõnevõrra värskendused olemasolevate artiklite uued artiklid või põhi rewrites. Töö neid muutusi eraldi töötamine. 

- Kui kustutate artikleid või pilte, ei mix soovitud kustutamised täiendused sisu või värskendused. Toime muudatused/uus sisu eraldi töötamise kontoris.

- Väljalasete või refaktooring sisu, plaani edasi oma PR läbivaataja. Peate oma abi väljaanne haru loomiseks või koordineerimiseks Ühenda korda avaldamise korda nii, et sisu on avaldatud õigel ajal.

- Kui proovite koordineerimine värskenduste tehtud muudatustega teete azure-sisu-hind hoidla ACOM repo (st muutub vasakpoolsel navigeerimisribal, kuvataks lehtede, ümbersuunamised või õ kaartide), peab teil koos oma PR läbivaataja koordineerimine töötavad ette valmistada. Vastasel korral saate risk, kellel on palju linkidest.

## <a name="criteria-for-expedited-pull-requests"></a>Kiirete pull taotlusi kriteeriumid

- Võtke ühendust azdocprs kiirendamiseks PRs ainult siis, kui see on tingimata vajalik. Saate taotleda kiirendada PR töötlemise punane Zone, privaatsus, legal ja turvalisus; tõeliselt katkenud klientide kogemusi; ja ametlik eskaleerimisele. 
- Funktsioon väljalasete sisu ei kvalifitseeru kiirete töötlemine - funktsiooni väljaanne sisu jaoks on vaja eelnevalt kavandamise või seda tuleb käsitleda kaudu standard prioriteet järjekorda.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Kiire? Esitage PRs, mida saate aktsepteeritakse automaatselt

PRMerger automaatika reeglite abil saate lisateavet oma igapäevane PRs ühendatud automaatselt.

PRMerger saate aktsepteerida oma PR automaatselt, kui:
* See mõjutab 10 failide või vähem.
* See sisaldab 15 võtab või vähem.
* Väiksem kui 20% teksti muudatused.
* Lülitid/lülititest ei värskendata.
* Failid on kustutatud või lisada.
* Pole pilte on uued, muudetud või kustutatud.

Kui teie taotlus pull ei vasta kriteeriumidele, "PRmerger ei saa Ühenda" sildi määratakse automaatselt nii teate, et see nõuab PR kujutist.

### <a name="need-to-make-a-lot-of-little-changes"></a>Kas vajate palju vähe muudatusi teha?

Võtta teie kii PRMerger automatiseerimine reeglid ülaltoodud ja tehke järgmist.
* Esitada artiklit hele muudatusi PR 10 või vähem failidega.
* Luua eraldi PR artiklite millised pildid või lülitid muutmine. See nõuab inimeste.
* Saate luua uue või kustutatud artiklite eraldi PR. See nõuab inimeste.

## <a name="related"></a>Seotud

- [Kvaliteedi kriteeriumid pull taotlus läbi vaadata.](contributor-guide-pr-criteria.md)

- [Tõmmata taotluse kommentaari automatiseerimine](contributor-guide-pull-request-comments.md)
