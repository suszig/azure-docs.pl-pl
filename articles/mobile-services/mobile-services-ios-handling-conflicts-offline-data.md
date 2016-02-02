<properties
    pageTitle="Mobile Services におけるオフライン データとの競合の処理 (iOS) | モバイル デベロッパー センター"
    description="Azure Mobile Services を使用して、iOS アプリケーションのオフライン データの同期時に生じる競合を処理する方法を説明します。"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2015"
    ms.author="krisragh;donnam"/>



# Mobile Services でのオフライン データの同期との競合の処理

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-offline-conflicts](../../includes/mobile-services-selector-offline-conflicts.md)]

このトピックでは、Azure Mobile Services のオフライン機能を使用しているときに、データを同期し、競合を処理する方法について説明します。 このチュートリアルは、[オフライン データの「」チュートリアルに基づいています。
>[AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28" target="_blank">Azure 無料試用版</a>します。


## IOS プロジェクトのダウンロード

このチュートリアルでは、ダウンロード [Github から更新済みの Xcode プロジェクト](https://github.com/Azure/mobile-services-samples/tree/master/TodoOffline/iOS)します。 [オフライン データの「チュートリアルの末尾から、Xcode プロジェクトを開始点として使用し、、項目の編集を許可するように更新しますしました。 サポートするクラスとメソッドも追加し、次のセクションに競合ハンドラーを追加できるようにしました。

このチュートリアルの最後に実行する場合、このアプリ 2 つの電話機がローカルで、両方の電話で同じ項目を変更し、サーバーへの変更をプッシュ バック、保持するバージョンを選択するには、各電話のユーザーを許可します。
  * クライアント バージョンを保持する (サーバー上のバージョンを上書きする)
  * サーバー バージョンを保持する (クライアントのローカル テーブルを更新する)
  * いずれのバージョンも保持しない (プッシュを取り消し、操作を保留したままにする)

次に、この機能を有効にする競合ハンドラーを追加しましょう。

## <a name="add-conflict-handling"></a>競合ハンドラーの Todo リスト ビュー コント ローラーへの追加します。

1. **QSTodoListViewController.m** で、**viewDidLoad** を編集します。 **defaultService** への呼び出しを、**defaultServiceWithDelegate** への呼び出しに置き換えます。

        self.todoService = [QSTodoService defaultServiceWithDelegate:self];

2. **QSTodoListViewController.h** で、**&lt;MSSyncContextDelegate&gt;** をインターフェイス宣言に追加し、**MSSyncContextDelegate** プロトコルを実装します。

        @interface QSTodoListViewController : UITableViewController<MSSyncContextDelegate, NSFetchedResultsControllerDelegate>

3. **QSTodoListViewController.m** の最上部に次の import ステートメントを追加します。

        #import "QSUIAlertViewWithBlock.h"

4. 最後に、このヘルパー クラスを使用するために、次の 2 つの操作を **QSTodoListViewController.m** に追加し、3 とおりのいずれかの方法で競合を調整するようにユーザーに要求します。

     - (void)tableOperation:(MSTableOperation *)operation onComplete:(MSSyncItemBlock)completion
     {
         [self doOperation:operation complete:completion];
     }
    
     -(void)doOperation:(MSTableOperation *)operation complete:(MSSyncItemBlock)completion
     {
         [operation executeWithCompletion:^(NSDictionary *item, NSError *error) {
    
             NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
    
             if (error.code == MSErrorPreconditionFailed) {
                 QSUIAlertViewWithBlock *alert = [[QSUIAlertViewWithBlock alloc] initWithCallback:^(NSInteger buttonIndex) {
                     if (buttonIndex == 1) { // Client
                         NSMutableDictionary *adjustedItem = [operation.item mutableCopy];
    
                         [adjustedItem setValue:[serverItem objectForKey:MSSystemColumnVersion] forKey:MSSystemColumnVersion];
                         operation.item = adjustedItem;
    
                         [self doOperation:operation complete:completion];
                         return;
    
                     } else if (buttonIndex == 2) { // Server
                         NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
                         completion(serverItem, nil);
                     } else { // Cancel
                         [operation cancelPush];
                         completion(nil, error);
                     }
                 }];
    
                 NSString *message = [NSString stringWithFormat:@"Client value: %@\nServer value: %@", operation.item[@"text"], serverItem[@"text"]];
    
                 [alert showAlertWithTitle:@"Server Conflict"
                                   message:message
                         cancelButtonTitle:@"Cancel"
                         otherButtonTitles:[NSArray arrayWithObjects:@"Use Client", @"Use Server", nil]];
             } else {
                 completion(item, error);
             }
         }];
     }


## <a name="test-app"></a>アプリケーションをテストします。

競合するアプリケーションをテストします。 稼働中のアプリケーションの 2 つの異なるインスタンス内の同じ項目を同時に編集するか、またはアプリケーションと REST クライアントを使用します。

上部からドラッグして、アプリケーション インスタンス内で更新ジェスチャを実行します。 これで、競合を調整するためのプロンプトが表示されます。

![][conflict-ui]




[update the app project to allow editing]: #update-app 
[update todo list view controller]: #update-list-view 
[add todo item view controller]: #add-view-controller 
[add todo item view controller and segue to storyboard]: #add-segue 
[add item details to todo item view controller]: #add-item-details 
[add support for saving edits]: #saving-edits 
[conflict handling problem]: #conflict-handling-problem 
[update qstodoservice to support conflict handling]: #service-add-conflict-handling 
[add ui alert view helper to support conflict handling]: #add-alert-view 
[add conflict handler to todo list view controller]: #add-conflict-handling 
[test the app]: #test-app 
[add-todo-item-view-controller-3]: ./media/mobile-services-ios-handling-conflicts-offline-data/add-todo-item-view-controller-3.png 
[add-todo-item-view-controller-4]: ./media/mobile-services-ios-handling-conflicts-offline-data/add-todo-item-view-controller-4.png 
[add-todo-item-view-controller-5]: ./media/mobile-services-ios-handling-conflicts-offline-data/add-todo-item-view-controller-5.png 
[add-todo-item-view-controller-6]: ./media/mobile-services-ios-handling-conflicts-offline-data/add-todo-item-view-controller-6.png 
[todo-list-view-controller-add-segue]: ./media/mobile-services-ios-handling-conflicts-offline-data/todo-list-view-controller-add-segue.png 
[update-todo-list-view-controller-2]: ./media/mobile-services-ios-handling-conflicts-offline-data/update-todo-list-view-controller-2.png 
[conflict-handling-problem-1]: ./media/mobile-services-ios-handling-conflicts-offline-data/conflict-handling-problem-1.png 
[conflict-ui]: ./media/mobile-services-ios-handling-conflicts-offline-data/conflict-ui.png 
[segmented controls]: https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/UIKitUICatalog/UISegmentedControl.html 
[core data model editor help]: https://developer.apple.com/library/mac/recipes/xcode_help-core_data_modeling_tool/Articles/about_cd_modeling_tool.html 
[creating an outlet connection]: https://developer.apple.com/library/mac/recipes/xcode_help-interface_builder/articles-connections_bindings/CreatingOutlet.html 
[build a user interface]: https://developer.apple.com/library/mac/documentation/ToolsLanguages/Conceptual/Xcode_Overview/Edit_User_Interfaces/edit_user_interface.html 
[adding a segue between scenes in a storyboard]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/chapters/StoryboardSegue.html#//apple_ref/doc/uid/TP40014225-CH25-SW1 
[adding a scene to a storyboard]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/chapters/StoryboardScene.html 
[core data]: https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreData/cdProgrammingGuide.html 
[download the preview sdk here]: http://aka.ms/Gc6fex 
[how to use the mobile services client library for ios]: mobile-services-ios-how-to-use-client-library.md 
[getting started offline ios sample]: https://github.com/Azure/mobile-services-samples/tree/master/TodoOffline/iOS/blog20140611 
[get started with offline data]: mobile-services-ios-get-started-offline-data.md 
[get started with mobile services]: mobile-services-ios-get-started.md 

