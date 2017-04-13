Mõned paketid ei pruugita installida, kui saan Azure pip abil.  Lihtsalt võib olla, et paketi pole saadaval Python paketi registri.  On võimalik, et on koostaja pole vaja (mõne koostaja pole saadaval, arvutis töötab veebirakenduse Azure'i rakendust Service).

Selles jaotises vaatame probleemi lahendusvõimaluste kohta.

### <a name="request-wheels"></a>Rataste taotlemine

Kui paketi installimiseks on koostaja, peaksite proovima paketi omanik taotleda, et rataste kättesaadav paketi poole pöördumist.

[Microsoft Visual C++ koostaja Python 2.7][]tehtud saadavusega on hõlpsam luua paketid, mis on Python 2,7 kohalikke kood.

### <a name="build-wheels-requires-windows"></a>Koostage rataste (nõuab Windows)

Märkus: Kui kasutate seda suvandit, veenduge, et paketi Python keskkond, mis vastab platvormi/arhitektuur/versioon, mis on kasutatud web Appile teenuses Azure rakenduse abil koostada (Windows/32-bit/2.7 või 3.4).

Kui paketi ei saa installida, kuna see nõuab on koostaja, saate installida koostaja oma kohalikus arvutis ja ratta paketi, mis sisaldavad teie siis oma hoidlas koostamine.

Mac/Linux kasutajad: Kui teil pole juurdepääsu Windowsi arvutisse, lugege teemat [loomine virtuaalse masina opsüsteemi Windows][] kohta, kuidas luua VM Azure.  Rataste koostamine, lisamine hoidla ja Hülga VM soovi korral saate seda kasutada. 

Python 2,7, saate installida [Microsoft Visual C++ koostaja Python 2.7][].

Python 3.4, saate installida [Microsoft Visual C++ 2010 kiire][].

Koostamiseks rataste, peate ratta pakkimine.

    env\scripts\pip install wheel

Saate kasutada `pip wheel` sõltuvus koostada:

    env\scripts\pip wheel azure==0.8.4

See loob .whl faili \wheelhouse kausta.  \Wheelhouse kausta ja ratta faile lisada oma andmebaasi.

Redigeeri oma requirements.txt lisamiseks soovitud `--find-links` ülaservas suvand. See ütleb pip enne python paketi registri kohalikus kaustas täpne vaste otsimiseks.

    --find-links wheelhouse
    azure==0.8.4

Kui soovite kaasata kõik teie sõltuvused \wheelhouse kausta ja kasutage python paketi registri üldse, saate jõustada pip paketi registri ignoreerida, lisades `--no-index` oma requirements.txt üles.

    --no-index

### <a name="customize-installation"></a>Installi kohandada

Saate kohandada juurutamise skripti abil alternatiivse installer, nagu näiteks lihtne virtuaalkeskkonnas paketi installimiseks\_installida.  Lugege teemat deploy.cmd näide, mis on kommenteerinud välja.  Veenduge, et selliste pakettide pole loetletud requirements.txt pip takistada installimist neid.

Lisage see juurutamise skripti.

    env\scripts\easy_install somepackage

Teil võib olla võimalik, et kasutada lihtne\_installi installi kaudu exe installer (mõned on zip ühilduv, nii et lihtne\_installi toetab neid).  Lisada oma andmebaasi installer ja autonoomsest lihtne\_installida saates tee käivitatava.

Lisage see juurutamise skripti.

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Kaasata virtuaalse keskkonna hoidla (nõuab Windows)

Märkus: Kui kasutate seda suvandit, veenduge, et kasutada virtuaalse keskkond, mis vastab platvormi/arhitektuur/versioon, mis kasutatakse teenuses Azure rakenduse veebirakenduse (Windows/32-bit/2.7 või 3.4).

Kui kaasate virtuaalse keskkonna hoidla, saate takistada tehes luua tühja faili virtuaalse keskkonna haldamise Azure juurutamise skripti:

    .skipPythonDeployment

Soovitame, et kustutada olemasoleva virtuaalse keskkonna rakenduse, konts failide kui virtuaalne keskkond on hallatavad automaatselt.


[Töötab Windows loomine]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ koostaja Python 2.7 jaoks]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
