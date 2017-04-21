<properties
   pageTitle="Allahindlusest artiklites linkide loomine" description="Selles artiklis kirjeldatakse kood crosslinks allahindlusest sisse." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Linkimise juhised Azure tehnilise sisu

### <a name="links-from-one-acom-article-to-another"></a>Linkide ühest ACOM artiklist teise

Loomiseks on Tekstisisese link ACOM tehniline artikkel teise ACOM tehnilise artikli, kasutage link järgmist süntaksit:  

- Teenuse directory lingid teise artikli sama teenuse kataloogis artikkel:

  `[link text](article-name.md)`

- Mõni artikkel sisaldab linke: teenuse alamkausta juurkaustas artikkel:

  `[link text](../article-name.md)`

- Artikkel root directory lingid teenuse alamkausta artikkel: 

  `[link text](./service-directory/article-name.md)`

- Teenuse alamkausta lingid mõne muu teenuse alamkausta artikkel artikkel:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Ankrud lingid

Teil pole ankrud loomiseks – luuakse automaatselt veebisaidil avaldamise aega H2 kõik pealkirjad. Ainus, mida on vaja teha on luua lingid H2 jaotised.

- Pealkirja sama artikli link:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Kinnitamiseks, mis on sama alamkausta teise artikli link:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Linkimiseks mõne kinnitamiseks klõpsake mõne muu teenuse alamkausta:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Üks võimalus automatiseerida oma artiklid automaatselt loodud Ankru lingid linkide loomine on [MarkdownAnchorLinkGenerator - tööriista loomiseks ankur linkide ACOM õiges vormingus](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Lingid sisaldab

Kuna kaasata failid on teise kataloogi, peate kasutama enam suhtelised teed, nagu allpool näidatud. Artikli linkimiseks kaasamine failist kasutatav vorming:

    [link text](../articles/service-folder/article-name.md)
    
Lisateavet selle kohta, kuidas kasutada [kohandatud allahindlusest laiendid juhised](custom-markdown-extensions.md#includes)kaasa faili.

## <a name="links-in-selectors"></a>Linkide lülitid

Kui teil on mõne kaasa manustatud lülitid, kasutaksite linkimise selline: 

    > [AZURE'I. VAATESELEKTORI-loendi (Dropdown1 | Dropdown2)]     -  [(Tekst1 | Example1)](../articles/service-folder/article-name1.md)
    - [(Tekst1 | Example2)](../articles/service-folder/article-name2.md)
    - [(tekst2 | Example3)](../articles/service-folder/article-name3.md)
    - [(tekst2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Viite laad lingid

Laadi lingid abil saate andmeallika sisu hõlpsamaks lugemiseks. Viite laad lingid asendage tekstisisene link süntaks lihtsustatud süntaks, mis võimaldab teil pikk URL-ide liikumiseks selle artikli lõpus. Siin on näide julge tulekera.

Tekstisisene tekst.

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Viited artikli lõpus link:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Veenduge, et lisate pärast koolon, link enne ruumi. Kui lingite muude tehnilisi artikleid, kui unustate ruumi lisada, katki link avaldatud artiklit. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Link: ACOM lehtedele, mis ei kuulu komplekti dokumentatsioon

Link lehele ACOM (nt hinnakirjad lehe SLA lehe või midagi muud, mis pole dokumentatsiooni artiklis), kasutage absoluutne URL, kuid jätta lokaadi. Eesmärk siin on lingid töötavad GitHub ja sulatatud saidil.

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>MSDN-i või TechNeti link

Kui teil on vaja MSDN-i või TechNeti link, kasutage täielikku lingi teema ja eemaldada en-us keele asukoha link. 

### <a name="use-friendly-link-text-for-all-links"></a>Kõigi Linkide sõbralik lingi tekst kasutamine

Saate lisada lingi sõnad peaks olema sõbralik – Teisisõnu, nad peaksid olema tavaline ingliskeelsete sõnade või lingite lehe tiitel. Ärge kasutage "click here". See on halb SEO ja piisavalt ei kirjelda sihtkohas.

**Paranda:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Vale:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>Turundusmaterjalide juurde suunavad lingid

Vältige azure.microsoft.com sisu turundusmaterjalide juurde suunavad lingid (meie ümbersuunamise süsteem). Neid tuleks kasutada ainult viimase sammuna, kui teil on vaja luua lingi lehe nime, kelle URL-i pole veel teada. Need on tegelikult peaaegu kunagi vaja. ACOM, määratlege faili nime, nii et saate teada, mida ta saab ette valmistada. Teegi teema, mis pole veel avaldatud, saate luua lingi, mis kasutab teema GUID nii, et teil pole kasutada ka FWLink.

Kui kasutate mõnda FWLink veebilehele, järgmised P parameetri oleks püsiv ümbersuunamine.

    http://go.microsoft.com/fwlink/p/?LinkId=389595

URL-i siht kleepimisel tööriista FWLink Ärge unustage lokaadi eemaldada, kui teie target link on ACOM, või MSDN-i või TechNeti teegi.

## <a name="remember-the-azure-library-chrome"></a>Pidage meeles Azure teegi Chrome'i!
Kui soovite link teemale Azure teek, mis asub [selle sõlme](https://msdn.microsoft.com/library/azure)all, ärge unustage määrata selle Azure'i Chrome'i link (/ Azure'i /). Azure'i Chrome'i jagab ACOM Navigeerimissuvandid ja kuvab ainult Azure MSDN-i teegi sisu. Lingi õigesti jäävates näeb välja umbes järgmine:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Muul juhul lehe ES MSDN-i tavavaade kogu MSDN-i puu kuvatakse.

### <a name="contributors-guide-links"></a>Osaliste juhend lingid

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
