Andmete factory on mitme rentniku teenus, mis on vaikimisi järgmistele piirangutele, et veenduda, et klientide tellimusi kaitstakse üksteise töökoormus. Paljude piiranguid saate hõlpsalt tõstatatud tellimuse lubatud kuni ühendust toega. 

**Ressurss** | **Vaikimisi limiit** | **Lubatud**
-------- | ------------- | -------------
andmete tehased Azure tellimuse | 50 | [Tugiteenuste osakonna poole pöördumine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
torujuhtmete andmete factory sees | 2500 | [Tugiteenuste osakonna poole pöördumine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
andmekomplektide andmete factory sees | 5000 | [Tugiteenuste osakonna poole pöördumine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
samaaegseid sektorid andmekomplekti euro kohta. | 10 | 10
ühe objekti müügivõimaluste objektidele <sup>1</sup> baiti | 200 KB | 2000 KB
ühe objekti andmekomplekti ja lingitud objektid <sup>1</sup> baiti | 100 KB | 2000 KB
Hdinsightiga nõudmisel kobar tuuma tellimuse <sup>2</sup> | 48 | [Tugiteenuste osakonna poole pöördumine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Pilveteenuse andmete liikumine üksus <sup>3</sup> | 8 | [Tugiteenuste osakonna poole pöördumine](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Uute katsete arv müügivõimaluste tegevuse jookseb | 1000 | MaxInt (32-bitine)

<sup>1</sup> müügivõimaluste, andmekomplekti ja lingitud objekte tähistavad loogiline rühmitamine oma töökoormus. Nende objektide limiidid käsitle suurt hulka andmeid, saate teisaldada ja Azure andmete Factory teenuse töötlemine. Andmete factory on mõeldud mastaapimiseks käsitlema terabait andmeid.

<sup>2</sup> nõudmisel Hdinsightiga tuuma jagatakse välja tellimuse, mis sisaldab andmeid factory. Selle tulemusena ülaltoodud on andmete Factory jõustatud core limiit nõudmisel Hdinsightiga tuuma ja erineb Azure tellimusega seostatud core limiit.

<sup>3</sup> cloud andmekogumiks liikumine (DMU) on kasutusel pilv pilve koopia toiming. See on näitaja, mis tähistab power (kombinatsiooni CPU, mälu ja võrgu ressursi eraldus) andmete Factory ühe üksuse. Tehtavate rohkem diiselrongide mõnel juhul võite saavutada suurema Kopeeri läbilaskevõimega. Vaadake [pilveteenuste andmete liikumine üksused](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) jaotises üksikasjad.

**Ressurss** | **Vaikimisi alampiir** | **Alampiir**
-------- | ------------------- | -------------
Intervall plaanimine | 15 minutit | 15 minutit
Proovi uuesti üritab vaheline intervall | 1 teise | 1 teise
Proovige ajalõpu väärtust | 1 teise | 1 teise


### <a name="web-service-call-limits"></a>Web teenuse kõne piirangud

Azure'i ressursihaldur on API kõned piirangud. Saate helistada API määra [Azure'i ressursihaldur API piirangud](../azure-subscription-service-limits.md#resource-group-limits). 


