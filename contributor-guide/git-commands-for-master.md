<properties pageTitle="Git käske uus artikkel loomise või olemasoleva artikli värskendamine" description="Juhised tehnilise Azure'i töötamise sisu GitHub hoidlate." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Git käske uus artikkel loomise või olemasoleva artikli värskendamine


## <a name="standard-process-working-from-master"></a>Standardne protsess (töötades juhtslaidi)
Järgige selle artikli luua kohaliku töötamise haru arvutis, nii et saate luua uus artikkel tehnilise dokumentatsiooni jaotise azure.microsoft.com või värskendada olemasoleva artikkel.

1. Käivitage Git Bash (või kasutate Git käsurea tööriista).

 **Märkus:** Kui töötate avaliku hoidlas, muuta azure-sisu-hind azure-sisu kõik käsud.

2. Azure'i-sisu-hind muutmiseks tehke järgmist.

        cd azure-content-pr
3. Vaadake juhtslaidi haru:

        git checkout master

4. Värske kohaliku töötamise haru tuletatud juhtslaidi haru loomiseks tehke järgmist.

        git pull upstream master:<working branch>


5. Viimine uue töötamise haru:

        git checkout <working branch>

6. Andke teada loodud kohaliku töötamise haru toidulauani:

        git push origin <working branch>

7. Teie uus artikkel luua või muuta olemasolevaid artikkel. Windows Exploreris abil avada allahindlusest uusi faile luua ja kasutada allahindlusest redaktorina Atomi (http://atom.io). Pärast loodud või muudetud oma artiklis ja pilte, jätkake järgmise juhisega.

8. Kinnitage tehtud muudatused ja lisamine.

        git add .
        git commit –m "<comment>"
        
   Või lisada ainult konkreetset faile saate muuta.

        git add <file path>
        git commit –m "<comment>"

   Kui te kustutatud faile, teil seda kasutama.
   
        git add --all
        git commit -m "<comment>"

9. Värskendage oma kohaliku töötamise haru muudatustega varustava.

        git pull upstream master

10. Tõuketeatised muudatused toidulauani github:

        git push origin <working branch>

12. Kui teil on valmis varustava juhtslaidi haru jaoks, valideerimine, sisu esitamine ja/või avaldada, kasutajaliideses GitHub pull koosolekukutse loomiseks oma toidulaualt juhtslaidi haru.

13. Kui olete töötaja töötavad privaatse hoidla esitamisel muudatused on automaatselt etapiviisilise ja lavastus lingi kirjutatakse pull kutse. Vaadake üle oma etapiviisilise sisu ja logige tõmmata taotluse kommentaaride, lisades kommentaari **#sign välja** .  See näitab, et muudatused on valmis lükata reaalajas.  Kui te ei soovi pull taotlus aktsepteeritakse -, kui te just lavastus muudatused – lisada märkme **#hold välja** tõmmata taotluse.

14. Pull taotluse aktsepteerija vaatab läbi pull kutse, pakub tagasiside ja/või aktsepteerib pull kutse. 

15. Soovi korral kontrollige avaldatud artiklit või muudatused

 http://Azure.microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Avaldamine

- Ilmub umbes 10:00 AM ja 3:00 PM Vaikse ookeani aja esmaspäevast reedeni. Võib kuluda kuni 30 minutit artiklite pärast avaldamine veebis kuvada. Pidage meeles, et tõmmata kutse on andmefailist pull taotluse läbivaataja enne muudatusi saab lisada käivitage järgmine ajastatud avalda. Peate oma pull taotluse läbivaataja ette valmistada tagamaks, et käivitada teatud avaldamiseks on ühendatud pull taotluse töötamiseks. Muul juhul vaadatakse PRs nii, et nad on esitatud.

- Kui olete privaatne hoidla töötaja, kehtivad kõigi pull taotlusi valideerimisreegleid, mida tuleb enne, kui pull taotluse liita. 

## <a name="working-with-release-branches"></a>Väljalaske harude töötamine

Kui töötate väljaanne haru, on parim viis loomine kohaliku töötamise haru väljaanne kontorist kasutada selle käsu süntaks:

    git checkout upstream/<upstream branch name> -b <local working branch name>

See loob kohaliku haru otse varustava haru, kohaliku ühendamise vältimiseks.

