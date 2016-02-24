
前の例は、アプリが起動するたびに ID プロバイダーとモバイル サービスの両方と通信します。 代わりに認証トークンをキャッシュし、先に認証トークンの使用を試みる方法もあります。

* iOS クライアントで認証トークンを暗号化し、格納する場合は、iOS Keychain の使用をお勧めします。 使用して [SSKeychain](https://github.com/soffes/sskeychain) -iOS Keychain の単純なラッパーです。 SSKeychain ページの指示に従って、それをプロジェクトに追加します。 いることを確認、 **Enable Modules** 、プロジェクトの設定が有効になっている **ビルド設定** (セクション **Apple LLVM-LANGUAGES - Modules**.)

* 開いている **QSTodoListViewController.m** し、次のコードを追加します。

```
        - (void) saveAuthInfo {
                [SSKeychain setPassword:self.todoService.client.currentUser.mobileServiceAuthenticationToken forService:@"AzureMobileServiceTutorial" account:self.todoService.client.currentUser.userId]
        }


        - (void)loadAuthInfo {
                NSString *userid = [[SSKeychain accountsForService:@"AzureMobileServiceTutorial"][0] valueForKey:@"acct"];
            if (userid) {
                NSLog(@"userid: %@", userid);
                self.todoService.client.currentUser = [[MSUser alloc] initWithUserId:userid];
                 self.todoService.client.currentUser.mobileServiceAuthenticationToken = [SSKeychain passwordForService:@"AzureMobileServiceTutorial" account:userid];

            }
        }
```

*  `loginAndGetData`, 、変更  `loginWithProvider:controller:animated:completion:`の完了ブロックします。 `[self refresh]` の直前に次の行を追加し、ユーザー ID とトークンのプロパティを格納します。

```
                [self saveAuthInfo];
```

* アプリの起動時にユーザー ID とトークンを読み込むようにします。  `viewDidLoad` で **QSTodoListViewController.m**, 、直後に追加`self.todoService` が初期化されています。

```
                [self loadAuthInfo];
```

