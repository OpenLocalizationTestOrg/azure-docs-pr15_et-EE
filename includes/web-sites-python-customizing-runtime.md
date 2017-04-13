Azure'i määrab Python kasutada oma virtuaalse keskkonna järgmised tähtsusega versioon:

1. määratud runtime.txt juurkaustas versioon
1. Python säte web appi konfiguratsioonis määratud versioon ( **sätete** > **Rakenduse sätted** blade Azure'i portaalis veebirakenduse jaoks)
1. Python-2.7 on vaikimisi, kui ükski eespool on määratud

Sobivad väärtused sisu 

    \runtime.txt

on:

- Python-2.7
- Python-3.4

Kui micro versioon (kolmas number) on määratud, seda ignoreeritakse.
