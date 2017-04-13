## <a name="wordpress-and-azure-app-service"></a>WordPress ja Azure rakenduse teenus

* [Mis on WordPress?](https://wordpress.org/)
* [Kuidas seadistada äriklassi WordPress web app](../articles/app-service-web/web-sites-php-enterprise-wordpress.md)
* [Kuidas osta ClearDB ühiskasutusse antud MySQL-i majutusteenuse rakenduse WordPress](http://blog.syntaxc4.net/post/2012/12/03/provisioning-a-mysql-database-from-the-windows-azure-store.aspx)
* [Kuidas soovite osta ClearDB sihtotstarbeline MySQL-i klaster rakenduse WordPress](https://azure.microsoft.com/blog/announcing-new-mysql-premium-tiers-from-cleardb/)
* [WordPress web appi tagatud MySQL-i kopeerimine kobar juurutamine](/documentation/templates/wordpress-mysql-replication/)
* [Koostage oma põhi-juhtslaidi MySQL-i kobar abil Percona kobar](/documentation/templates/mysql-ha-pxc/) ja [Lugege lisateavet kohta, kuidas hallata klaster](https://github.com/fanjeffrey/axiom.articles/tree/master/pxc)
* [WordPress toetavad MySQL-i kopeerimine kobar juhtslaidi-slave konfiguratsiooni juurutamine](/documentation/templates/mysql-replication/)
* [SQL Azure'i DB haldab ProjectNami toetavad WordPress minirakenduse juurutamine](/marketplace/partners/projectnami/projectnami/)
  
## <a name="troubleshooting-wordpress-application"></a>WordPress rakenduse tõrkeotsing

* [WordPress rakenduse tõrkeotsing](https://sunithamk.wordpress.com/2014/09/04/wordpress-troubleshooting-techniques-on-azure-websites/)
* [Koguge kokku kasutus telemeetria Azure'i rakenduse ülevaated teenuse abil](https://azure.microsoft.com/blog/usage-analytics-for-wordpress-with-azure-app-insights/)
* [Zend Zray profiler vastuolus oma veebirakenduse diagnoosida probleemid ja jõudlus](https://sunithamk.wordpress.com/2015/08/04/profiling-php-application-on-azure-web-apps/)
* [Diagnoosimine ja leevendada probleemid reaalajas Kudu tugi portaali abil](https://sunithamk.wordpress.com/2015/11/04/diagnose-and-mitigate-issues-with-azure-web-apps-support-portal/)
* [Erinevate automaatne-heal kasutamine reeglite automatiseerimiseks lahendamise reaalajas juhtumite](http://microsoftazurewebsitescheatsheet.info/#auto-heal)
* [Kuidas varundada oma veebirakenduse](../articles/app-service-web/web-sites-backup.md) ja [oma veebirakenduse taastamine](../articles/app-service-web/web-sites-restore.md)

## <a name="performance"></a>Jõudluse

* [Kuidas kiirendada WordPress web app](https://sunithamk.wordpress.com/2014/08/01/10-ways-to-speed-up-your-wordpress-site-on-azure-websites/)
* [Kuidas lubatud redis vahemälu](../articles/redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) [redis vahemälu lisandmooduli](https://wordpress.org/plugins/wp-redis/) abil
* [Kuidas lubada memcached objektivahemälu WordPress](../articles/app-service-web/web-sites-connect-to-redis-using-memcache-protocol.md) [memcached lisandmooduli](https://wordpress.org/plugins/memcached/) abil
* [Wincache koos W3 Kokku vahemälu lisandmooduli lubamine](https://wordpress.org/plugins/w3-total-cache/)
* [WordPress app kiirendamiseks supercache lisandmooduli kasutamise kohta](http://ruslany.net/2008/12/speed-up-wordpress-on-iis-70/)
* [Kuidas serveriga vahemälu kasutades IIS-i väljundi vahemällu talletamine](http://blogs.msdn.com/b/brian_swan/archive/2011/06/08/performance-tuning-php-apps-on-windows-iis-with-output-caching.aspx)
* [Kuidas lubatud brauseri vahemälu Staatiline sisu jaoks](http://www.iis.net/configreference/system.webserver/staticcontent)
