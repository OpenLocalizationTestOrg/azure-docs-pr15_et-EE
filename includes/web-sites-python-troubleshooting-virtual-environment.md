Juurutamise skripti vahele virtuaalse keskkonna loomine Azure kui tuvastab, et ühilduvad virtuaalne keskkond juba olemas.  Kiirendada juurutamise märkimisväärselt.  Paketid, mis on juba installitud jäetakse vahele, pip.

Teatud juhtudel võib-olla soovite kustutada virtuaalse keskkonnas.  Mida soovite teha, kui te otsustate lisada virtuaalse keskkonna oma andmebaasi.  Samuti võite seda teha, kui teil on vaja vabaneda teatud pakettide või testida requirements.txt muudatustest.

On paar võimalust olemasoleva virtuaalse keskkonna Azure haldamiseks.

### <a name="option-1-use-ftp"></a>Variant 1: Kasutada FTP

FTP klient, serveriga ja te saama keskkonna kausta kustutada.  Võtke arvesse, et mõned FTP kliendid (nt veebibrauser) võib olla kirjutuskaitstud ja ei luba kustutada kaustad, seega peaksite veenduge, et kasutada FTP klient selle võimalusega.  FTP hosti nimi ja kasutajale kuvatakse web appi blade [Azure portaali](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Variant 2: Tumblernupu käitusaja

Siin on alternatiiv, mis kasutab ära juurutamise skripti kustutab see ei vasta soovitud versiooni Python keskkonna kausta asjaolu.  See tõhus kustutada olemasolevate keskkonna ja looge uus.

1. Aktiveerige muu versioon, Python (runtime.txt või **Rakenduse sätted** tera Azure'i portaalis) kaudu
1. git push mõned muudatused (pip installi vigu ignoreerida, kui mõni)
1. Aktiveerige uuesti Python Algne versioon
1. git push mõned muudatused uuesti

### <a name="option-3-customize-deployment-script"></a>Võimalus 3: Kohandamine juurutamise skripti

Kui olete kohandatud juurutamise skripti, saate muuta koodi deploy.cmd abil keskkonna kausta kustutada.
