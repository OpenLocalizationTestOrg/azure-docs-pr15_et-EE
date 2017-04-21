<properties
   pageTitle="Lehe pealkiri, mis kuvab brauseri menüü ja Otsingu tulemused"
   description="Artikli kirjeldus, mis kuvatakse lehekülgi algus ja kõige otsingutulemuste"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Allahindlusest mall (artikkel pealkiri 1 või H1): selle artikli ülaosas pealkiri

Kopeerige soovitud allahindlusest selle malli põhjal, kopeerige see artikkel oma kohaliku repo või töötlemata nuppu GitHub UI ja kopeerige soovitud allahindlusest.

  ![Aseteksti; tühjaks. Kirjeldage pilt.][8]

Sissejuhatav lõik: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia porta nisl imperdiet kasutamise lõpetada. Mauris dolor mauris, tempus sed lacinia nec, mitte felis euismod. Nunc semper porta ultrices. Maecenas neque nulla, condimentum vitae ipsum istuda amet dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Pealkiri 2 (H2)

Aenean istuda amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, faucibus rhoncus sisse, faucibus sed urna sisse.  volutpat mi id purus ultrices iaculis nec mitte neque. [Lingi teksti lingi azure.microsoft.com väljaspool](http://weblogs.asp.net/scottgu). Nullam dictum dolor aliquam pharetra juures. Vivamus ac hendrerit mauris [näide lingi tekst link artikkel teenuse kausta](../articles/expressroute/expressroute-bandwidth-upgrade.md).

10 korda rohkem liiklust toomine [Google]  [ gog] kui [Yahoo]  [ yah] [MSN- i] [msn].

> [AZURE.NOTE] Taandatud märkuse tekst.  Sõna "märge" lisatakse publikatsiooni ajal. TÜ EL-i pretium lacus. Nullam purus Eesti, iaculis sed Eesti vel, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi EL condimentum turpis nisi on purus.

1. Aenean istuda amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, faucius sisse **Rhoncus** faucibus sed urna sisse. Suspendisse volutpat mi id purus ultrices iaculis nec mitte neque.

    ![Aseteksti; tühjaks. Koguja auto Racing punane.][5]

3. Nullam dictum dolor aliquam pharetra juures. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum ja varius a, vehicula nisl juures.

    ![Aseteksti; jätke tühjaks][6]


Suspendisse volutpat mi id purus ultrices iaculis nec mitte neque. Nullam dictum dolor aliquam pharetra juures. Vivamus ac hendrerit mauris. Otrus informatus: [Link 1 mõne muu azure.microsoft.com dokumentatsioon](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Pealkiri (H2)

TÜ EL-i pretium lacus. Nullam purus Eesti, iaculis sed Eesti vel, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi EL condimentum turpis nisi on purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum eelneva ipsum miinimumpalgaga faucibus orci luctus sisse ja ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] DUIs sed diam mitte <i>nisl molestie</i> pharetra eget on Eesti. [Mõne muu azure.microsoft.com dokumentatsiooni teemas link 2](web-sites-custom-domain-name.md)


Quisque commodo eros vel lectus euismod auctor eget istuda amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Pealkiri 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse häirete korral võtke ühen.

  + Fusce
  + Malesuada
  + SEM

2. Nullam massa EL tellus tempus hendrerit sisse.

    ![Aseteksti; tühjaks. Näide 9 kanali video.][7]

3. Quisque felis ENIMile, fermentum TÜ aliquam nec, pellentesque pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Vestibul eelneva ipsum miinimumpalgaga faucibus orci luctus sisse ja ultrices posuere cubilia Curae; Nullam ultricies ipsum vitae volutpat hendrerit, purus diam pretium eros, vitae tincidunt nulla lorem sed turpis: [Link 3 mõne muu azure.microsoft.com dokumentatsiooni](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
