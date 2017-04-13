**Eesmärk-C**: 

1. Mac-arvutisse, avage _QSTodoListViewController.m_ Xcode ja lisage järgmisel viisil. Muuta _google_ _microsoftaccount_, _Twitteri_, _Facebooki_või _windowsazureactivedirectory_ , kui te ei kasuta Google teie identiteedi pakkuja. Kui kasutate Facebook, [peate nimekiri Facebooki domeenid rakenduse](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Asendage `[self refresh]` sisse `viewDidLoad` sisse _QSTodoListViewController.m_ koos järgmist:

            [self loginAndGetData];

3. Vajutage, _käivitage_ rakendus alustada, ja seejärel logige sisse. Kui olete sisse logitud, peaks saama Todo loendi vaatamiseks ja värskendusi tegema.

**Kiire**:

1. Mac-arvutisse, avage _ToDoTableViewController.swift_ Xcode ja lisage järgmisel viisil. Muuta _google_ _microsoftaccount_, _Twitteri_, _Facebooki_või _windowsazureactivedirectory_ , kui te ei kasuta Google teie identiteedi pakkuja. Kui kasutate Facebook, [peate nimekiri Facebooki domeenid rakenduse](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Eemalda read `self.refreshControl?.beginRefreshing()` ja `self.onRefresh(self.refreshControl)` lõpus `viewDidLoad()` _ToDoTableViewController.swift_sisse. Kõne lisamine `loginAndGetData()` oma kohas:

            loginAndGetData()

3. Vajutage _käivitada_ , käivitage rakendus ja seejärel logige sisse. Kui olete sisse logitud, peaks saama Todo loendi vaatamiseks ja värskendusi tegema.
