
次に、登録が試行される前にユーザーが認証されるように、プッシュ通知が登録される方法を変更する必要があります。

1.  **QSAppDelegate.m**, の実装をまとめて削除 **didFinishLaunchingWithOptions** 全体です。

2. 開いている **QSTodoListViewController.m** の末尾に次のコードを追加し、 **viewDidLoad** メソッド。

```
// Register for remote notifications
[[UIApplication sharedApplication] registerForRemoteNotificationTypes:
UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
```

