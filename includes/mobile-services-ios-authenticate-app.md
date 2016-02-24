* 開いている **QSTodoListViewController.m** し、次のメソッドを追加します。 変更 _facebook_ に _microsoftaccount_, 、_twitter_, 、_google_, 、または _windowsazureactivedirectory_ id プロバイダーとして Facebook を使用していない場合。

```
        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                [self refresh];
            }];
        }
```

* `viewDidLoad` の `[self refresh]` を次のコードに置き換えます。

```
        [self loginAndGetData];
```

* キーを押して  **実行** 、アプリケーションを開始し、ログインします。 ログインが成功すると、Todo リストを表示して更新できます。

