Interneti-ühendusega arvutid asjade leidmiseks kasutatakse Domain Name System (DNS). Näiteks kui sisestate aadressi brauseris, või klõpsake veebilehe lingi, kasutab DNS-i domeeni tõlkida IP-aadress. IP-aadress on omamoodi nagu füüsilise asukoha aadressi, kuid see ei ole väga inimeste sõbralik. Näiteks, on märksa lihtsam DNS-i nimi, nt **contoso.com** meeles pidada, kui see on meeles pidada, nt 192.168.1.88 või 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP-aadress.

DNS-i süsteemis põhineb *kirjed*. Kirjete teatud *nimi*, nt **contoso.com**, seostada teise DNS-i nimi või IP-aadress. Kui rakendus, näiteks veebibrauser, otsib DNS-i nimi, leiab kirje ja kasutab iganes osutab aadressina. Kui see osutab väärtus on IP-aadress, brauseri kasutab seda väärtust. Kui see osutab teise DNS-i nimi, siis taotlus on eraldusvõime uuesti teha. Kokkuvõttes kõik nimelahendus lõpeb IP-aadress.

Azure'i veebisait loomisel määratakse saidile automaatselt DNS-i nimi. Sellise nimega vormis ** &lt;yoursitename&gt;. azurewebsites.net**. Veebisaidi lisamisel lõpp Azure'i liikluse haldur veebisaidi pääseb siis soovitud ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domeeni.

> [AZURE.NOTE] Kui teie veebisait on konfigureeritud liikluse haldur endpoint, kasutage funktsiooni **. trafficmanager.net** käsitleda kui loote DNS-i kirjed.

> Saate kasutada ainult CNAME-kirje liikluse haldur

On ka mitut tüüpi kirjeid, igal versioonil oma funktsioonid ja piirangud, kuid veebilehed nimega liikluse haldur lõpp-punktid konfigureeritud, ainult teeme ühe; *CNAME-* kirje.

###<a name="cname-or-alias-record"></a>Alias (pseudonüüm)- või CNAME-kirje

CNAME-kirje kaardid *teatud* DNS-i nimi, nt **mail.contoso.com** või **www.contoso.com**, mõne muu (kanoonilise) domeeni nimi. Kanoonilise domeeninimi on puhul Azure veebisaitide abil liikluse haldur, kuvatakse ** &lt;myapp >. trafficmanager.net** domeeninimi halduri liikluse profiili. Kui loodud, loob CNAME alias on ** &lt;myapp >. trafficmanager.net** domeeni nimi. CNAME-kirje on IP-aadressi lahendamiseks oma ** &lt;myapp >. trafficmanager.net** domeeninime automaatselt, et kui veebisaidi IP-aadress muutub, saate ei pea midagi teha.

Kui liikluse saabub veebisaidil liikluse haldur see siis marsruudib liikluse teie veebisaidile, kasutades laadi tasakaalustamiseks meetod, mis see on konfigureeritud. See on teie veebisaidi külastajad täielikult läbipaistev. Need kuvatakse ainult kohandatud domeeninime oma brauseris.

> [AZURE.NOTE] Mõned domeeniregistraatori võimaldavad ainult vastendamine alamdomeene kasutamisel CNAME-kirje (nt **www.contoso.com**) ja mitte juurkausta nimed, nt **contoso.com**. CNAME-kirje kohta lisateabe saamiseks dokumentatsioonist oma domeeniregistraatori, <a href="http://en.wikipedia.org/wiki/CNAME_record">Wikipedia kirje CNAME-kirje</a>või dokumendi <a href="http://tools.ietf.org/html/rfc1035">IETF domeeninimedega - rakendamise ja määratlus</a> .
