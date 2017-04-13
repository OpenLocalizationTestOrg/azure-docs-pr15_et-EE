DNS-i tsooni kasutatakse domeeni DNS-i kirjeid. Selleks, et alustada oma domeeni, peate looma DNS-i tsooni. Mis tahes DNS-i kirje loomine domeeni saab domeeni DNS-i alal. 

Näiteks "contoso.com" domeeni võib sisaldada arvu DNS-i kirjeid, nt "mail.contoso.com (meiliserver) jaoks" ja "www.contoso.com" (web site) jaoks. 


## <a name="names"></a>DNS-i tsooni nimed
 
- Tsooni nimi peab olema kordumatu rühmas ressursi ja tsooni peab on juba olemas. Muul juhul toiming nurjub.

- Sama tsooni nimi võib olla erinevad ressursirühm või mõnda muud Azure tellimust uuesti kasutada. 

- Kui mitme tsoonid sama nime anda ühiskasutusse, iga eksemplari korral määratakse erinevate nimeserverite aadressid ja ainult üks näiteks saate delegeeritud ema domeeni. Lisateabe saamiseks vt [volitatud esindaja Azure'i DNS-i domeeni](../articles/dns/dns-domain-delegation.md).