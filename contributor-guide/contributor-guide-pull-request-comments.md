# <a name="pull-request-comment-automation"></a>Tõmmata taotluse kommentaari automatiseerimine

Kasutame pull taotluse kommentaarid lubamiseks osaliste kommentaari automatiseerimine ja autorid määrata sildid, et juhtida pull taotluse läbivaatamine.

| Kommentaari | Mida see tähendab? | Kättesaadavus|
| -------- |-------------|-------------|
|#Logi välja | Kui autor artikli tipib kommentaar **#sign välja** voo kommentaar, **valmis-Ühenda** silt on määratud. | Avalike ja privaatvõ|
|#Logi välja | Kui kaasautor, kes pole loetletud Autor üritab logige välja avaliku pull taotluse abil **#sign välja** kommentaari, sõnumi kirjutatakse pull taotluse näitava sildi saab määrata ainult autori. | Avaliku |
|#Hoidke väljalülitamine | Kui tipite **#hold välja** tõmmata taotluse kommentaari, eemaldab **valmis ja ühendamine** sildi - juhuks, kui muudate meelt või eksikombel. Privaatne repo, seda määrab **ära ei ole-ühendamine** silt. | Avalike ja privaatvõ |
| #Palun Sule | Autorite tippida **#please Sule** kommentaari kommentaari voo pull taotluse sulgemiseks, kui te ei soovi tehtud muudatused, mis on ühendatud. | Avaliku |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Tõrkeotsingu Logi leidmine avaliku repo

Avaliku repo Logi välja automatiseerimine on lubatud ainult autor logige välja. Mõned käsitsi erandi töötlemine võib olla vajalik:

- **Artikli autorid**: avaliku hoidla kommentaari automatiseerimise kasutamiseks tegelik GitHub konto peab täpselt vastama artiklis metaandmete GitHub kontonime. Suurtähestuse konto küsimustes. Kui teil on blokeeritud probleemi tõttu logida, saata e-posti azdocprs alias (pseudonüüm).

- **Teistele töötajatele**: kui olete töötaja, kes on logida nimel autori ja teil on blokeerinud automaatika, pöörduge azdocprs pull taotluse link. Saate määrata, kes te olete--PMs sama toote meeskond, kolleegid kirjalikult meeskond ja meeskonnatöö haldurid kirjutamine käsitletakse usaldusväärsest allikast.



## <a name="related"></a>Seotud

- [Pull taotluse etikett ja Microsoft osaliste head tavad](contributor-guide-pull-request-etiquette.md)

- [Kvaliteedi kriteeriumid pull taotlus läbi vaadata.](contributor-guide-pr-criteria.md)
