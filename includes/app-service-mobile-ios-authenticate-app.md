1. Mac の Xcode で **QSTodoListViewController.m** を開き、次のメソッドを追加します。 Facebook を ID プロバイダーとして使用しない場合は、_facebook_ を _microsoftaccount_、_twitter_、_google_、_windowsazureactivedirectory_ のいずれかに変更します。

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

2. 置換 `[自動更新]` で `viewDidLoad` 伴って。

        [self loginAndGetData];

3. キーを押して  **実行** 、アプリケーションを開始し、ログインします。 ログインが成功すると、Todo リストを表示して更新できます。





