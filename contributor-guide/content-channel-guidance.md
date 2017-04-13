<properties title="" pageTitle="Azure'i tehnilise sisu kanali abi saada?" description="Kirjeldatakse töötajate, partnerite ja ühenduse osaliste tuleks kasutada Azure tehnilise sisu avaldamiseks Microsoft sisu kanaleid." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/06/2015" ms.author="tysonn" />

# <a name="azure-technical-content-channel-guidance"></a>Azure'i tehnilise sisu kanali abi saada?

GitHub on üsna lihtne viis (kui teil üle Git küür) koostamine ja avaldamine tehnilise sisu. Kuid tuleb tagada, et sisu jääb piires tehnilise dokumentatsiooni - on muud tüüpi teavet muudest kanalitest.

##<a name="technical-content-that-belongs-in-the-azure-content-pr-repository"></a>Tehniline sisu, mis kuulub hoidlas azure-sisu-hind

Azure'i – sisu ja azure-sisu-hind hoidlate avaldamiseks http://azure.microsoft.com/documentation/articles kuuluvad **tehnilised artiklid toote kasutamise kohta** . Need peaks sisaldama lisatav mõistmine ja kasutamine toote kontseptuaalne või üksikasjalik teave. Tehniline sisu kanal on tehnilise sisu kuvamine inimesed **Kuidas** midagi teha. Saab rääkida "mida" ja "miks" kliendid soovidele aru, kuid artikleid keskenduvad sisu räägib inimeste ülesanne või tehke seda stsenaariumi.

##<a name="technical-content-that-does-not-belong-in-the-azure-content-pr-repository"></a>Tehniline sisu, mis ei kuulu hoidlas azure-sisu-hind

**Viide sisu**: hallatavate viide, REST API-d, PowerShelli cmdlet-käsu spikker, skeemi viide ja tõrge viide kuulub MSDN-i jäävates Azure teegis (http://msdn.microsoft.com/library/azure/). Klõpsake http://azure.github.io/ kuulub sõlm, Ruby ja muu keele viide sisu.

Järgmist tüüpi sisu edastatakse teiste Azure'i või Microsoft sisu kanalite; Mõnel juhul teatud tüüpi sisu pole osa oma sisu strateegiat.

- **Postitused**: ajaveebipostituste tavaliselt kirjutada esimese inimese hääl ja on tavaliselt seotud teadaanded ja reklaamid. Sellise sisu kuulub tavaliselt Azure ajaveebist. Sügavalt tehnilist või üksikasjalik sisu ei lähe ajaveebist.

- **Juhtumianalüüsid ja kliendi lugude**: juhtumianalüüsid ja kliendi lood on väga kindla projektitulemi, mis läheb läbi turundus, nad on oma protsess ja juhised ja on loodud teatud klientide ja partnerite kokkulepete. Need on avaldatud https://customers.microsoft.com/. Ei sea midagi juhtumianalüüsi või kliendi lugu juhul, kui see on osa ametliku juhul õpperühma protsess ja pole selle avaldada tehnoloogia sisu hoidla.

- **Proovi kood ja projekti**: proovi kood ja projekti avage näidised hoidlate ja valimi galeriis on esiletõstetud.

- **Ühenduse tulipunkt/ühenduse ressursse**: artiklite pakub ühenduse projektide. Repo on tehnilise sisu kohta, kuidas kasutada Microsoft persective toote, mitte selle kohta, kuidas inimesed kasutavad selle toote jaoks. Mis on turundus- või võib-olla ajaveebi sisu. Või lasta kindlaks teha, et see on oma lugu kohtades, et ühenduse meeldib kõige paremini ühenduse!

- **Nõuetele vastavus**: https://www.microsoft.com/en-us/TrustCenter/Compliance?service=Azure#Icons peab postitatakse tööstusstandarditele ja nõuetele vastavus teabe Azure'i teenuste jaoks. See hõlmab sertimine standardid nagu ISO riigi ja valitsuse kinnitamine, panga, seisund või muude kinnitamine.

- **Allalaaditavad failid**: tehniline peaks dokumendid, kui ei allalaaditavad failid. Luua allalaaditavad PDF-ide sisu tehniline sisu hoidla. Muud downloabable sihtkoha allalaadimiskeskus.

- **Tagasiside - palunud tagasiside kaudu e-posti aadressid**: kinnitatud tagasiside teed Azure sisu jaoks kaasata tagasiside link, mis kuvatakse saidi jaluse, rahulolu hinnangute ja sõna-sõnalt kontrolli, Disqus kommentaarid, otsese artiklis osakaalu GitHub pull nõuete ja UserVoice saidi. Ei lisage see hulk kanalite tagasiside e-posti teel saatmine teistele küsides.

- **Tulevikus toote lepingud**: avaldamine laused tulevaste toote lepingute kohta tehnilise dokumentatsiooni. Tehnilised dokumendid tuleb kirjeldada ainult, mis on võimalik välja toote.

- **Juriidilised tingimused**: kõik üles Azure juriidilised terminid: https://azure.microsoft.com/en-us/support/legal/

- **Turunduse sisu**: sisu funktsioon/kasu üksikasjalik kirjeldus, mis pakub või mis lihtsalt loendeid kõrge võimaluste teenus on tõenäoliselt turundus sisu. See kuulub turundus saidile alad. Turundusmaterjalide sisu avaldamiseks faili töö taotluse azure.microsoft.com.

- **Hinnad teave**: saab rääkida mõju tehnilise valikuid üldiselt maksumus on, kuid teha pole hinnapakkumise teatud hinnad tehnika artiklites üksikasjad. Selle asemel ette hinnakirjad lehele lingi räägite teenus.

- **Kursori artiklid allalaadimiseks**: asemel small lehti, mis sisaldavad midagi, kuid allalaadimise link, vaid linkimine allalaadimine asjakohase tehnilise sisu.

- **Privaatsusteave**: on Microsoft Online Services, mis hõlmab kõiki Azure'i jaoks üles kõik privaatsuspoliitika. Kindla teenuse privaatsusteave esitatakse tehnilise sisuna, mitte "privaatsusavaldused". Lugege teemat https://azure.microsoft.com/en-us/support/legal/.

- **Suunake artikleid**: Kui kustutate mõne artikli sisust, jätke artikli link asendamine sisu avaldatud. Kasutage reaal suunata.

- **Väljalaskemärkmed**: kui see on artikli SDK või StorSimple artiklist riistvara värskenduse, sellised andmed tuleks lihtsalt manustatud asjakohase tehnilise sisu või kaasatud teenuste värskenduste kanali.

- **Mis on uut rakenduses väljalaske- või teenuse**: teenusevärskendused kanali minge loendid või mis on uut rakenduses väljalaske- või teenuste kirjeldused.
