<properties
    pageTitle="オプティミスティック同時実行制御でデータベース書き込み競合を処理 (Windows ストア) | Microsoft Azure"
    description="サーバーと Windows ストア アプリケーションの両方でデータベースへの書き込みの競合を処理する方法について説明します。"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/05/2015"
    ms.author="wesmc"/>

# データベースの書き込み競合の処理

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;




##概要

このチュートリアルでは、Windows ストア アプリケーションの同じデータベース レコードに複数のクライアントが書き込みを行ったときに発生する競合を処理する方法について説明します。 シナリオによっては、複数のクライアントが同じ項目に対して同時に変更を書き込む場合があります。 競合を検知しない場合、それを意図していなくても、最後に行われた書き込みによってそれ以前の更新がすべて上書きされます。 Azure モバイル サービスには、このような競合を検出して解決する機能がサポートされています。 このトピックでは、サーバー上とアプリケーション内でデータベース書き込み競合を処理する手順を紹介します。

このチュートリアルでは、クイック スタート アプリケーションに機能を追加して、TodoItem データベースを更新するときに発生する可能性がある競合を処理します。


##前提条件

このチュートリアルには、次のものが必要です。

+ Microsoft Visual Studio 2013 以降
+ このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 このチュートリアルを開始する前に完了しておく必要 [を使ってみるモバイル サービス] です。
+ [Azure アカウント]
+ Azure モバイル サービス NuGet パッケージ 1.1.0 以降。 最新バージョンを入手するには、次の手順に従います。
    1. Visual Studio でプロジェクトを開き、ソリューション エクスプ ローラーでプロジェクトを右クリックし、] をクリックして **Nuget パッケージの管理**します。

        ![][19]

    2. 展開 **オンライン** ] をクリック **Microsoft and .NET**します。 検索テキスト ボックスに入力 **Azure Mobile Services**します。 をクリックして **インストール** 上、 **Azure Mobile Services** NuGet パッケージ。

        ![][20]




##更新を実行できるようにアプリケーションを更新する

このセクションでは、TodoList ユーザー インターフェイスを更新して、ListBox コントロールの各項目のテキストを更新できるようにします。 ListBox にはデータベース テーブルの各項目に対応する CheckBox コントロールと TextBox コントロールが含まれます。 TodoItem のテキスト フィールドを更新できるようになります。 アプリケーションで TextBox の `LostFocus` イベントを処理して、データベース内の項目を更新できるようになります。


1. Visual Studio でダウンロードした TodoList プロジェクトを開き、[モバイル サービスの開始を取得] チュートリアルです。
2. Visual Studio ソリューション エクスプローラーで MainPage.xaml を開き、その中の `ListView` 定義を次に示す `ListView` で置き換えて、変更を保存します。

        <ListView Name="ListItems" Margin="62,10,0,0" Grid.Row="1">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" Checked="CheckBoxComplete_Checked" Margin="10,5" VerticalAlignment="Center"/>
                        <TextBox x:Name="ToDoText" Height="25" Width="300" Margin="10" Text="{Binding Text, Mode=TwoWay}" AcceptsReturn="False" LostFocus="ToDoText_LostFocus"/>
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>


4. Visual Studio Solution Explorer で、共有プロジェクトにある MainPage.cs を開きます。 次に示すように、MainPage に TextBox の `LostFocus` イベントのイベント ハンドラーを追加します。


        private async void ToDoText_LostFocus(object sender, RoutedEventArgs e)
        {
            TextBox tb = (TextBox)sender;
            TodoItem item = tb.DataContext as TodoItem;
            //let's see if the text changed
            if (item.Text != tb.Text)
            {
                item.Text = tb.Text;
                await UpdateToDoItem(item);
            }
        }

4. 共有プロジェクトの MainPage.cs で、次に示すように、イベント ハンドラーで参照される MainPage の `UpdateToDoItem()` メソッドの定義を追加します。

        private async Task UpdateToDoItem(TodoItem item)
        {
            Exception exception = null;
            try
            {
                //update at the remote table
                await todoTable.UpdateAsync(item);
            }
            catch (Exception ex)
            {
                exception = ex;
            }
            if (exception != null)
            {
                await new MessageDialog(exception.Message, "Update Failed").ShowAsync();
            }
        }

これで、TextBox からフォーカスが外れたときに各項目のテキストの変更がデータベースに書き戻されるようになります。

##アプリケーションでの競合検出を有効にする

シナリオによっては、複数のクライアントが同じ項目に対して同時に変更を書き込む場合があります。 競合を検知しない場合、それを意図していなくても、最後に行われた書き込みによってそれ以前の更新がすべて上書きされます。 [オプティミスティック同時実行制御] には、各トランザクションがコミットでき、したがってロックが使用されない任意リソースことが想定しています。 オプティミスティック同時実行制御ではトランザクションをコミットする前に、他のトランザクションがそのデータを変更していないことを確認します。 データが変更されている場合、トランザクションのコミットはロール バックされます。 Azure Mobile Services はオプティミスティック同時実行制御をサポートしており、各テーブルに追加されている `__version` システム プロパティ列を使用して各項目の変更を追跡します。 このセクションでは、アプリケーションで `__version` システム プロパティを利用してこのような書き込み競合を検出できるようにします。 前回のクエリ以降にレコードが変更されている場合は、アプリケーションで更新しようとしたときに `MobileServicePreconditionFailedException` が通知されます。 その際、データベースに変更をコミットするか、前回の変更をそのままデータベースに残すかというどちらかの処理を選択することができます。 Mobile Services のシステムのプロパティの詳細については、[システムのプロパティ] を参照してください。

1. 共有プロジェクトで TodoItem.cs を開き、`TodoItem` クラスの定義を次のコードで更新し、書き込み競合の検出サポートを有効にする `__version` システム プロパティを含めます。

        public class TodoItem
        {
            public string Id { get; set; }
            [JsonProperty(PropertyName = "text")]
            public string Text { get; set; }
            [JsonProperty(PropertyName = "complete")]
            public bool Complete { get; set; }
            [JsonProperty(PropertyName = "__version")]
            public string Version { set; get; }
        }

    > [AZURE.NOTE] 型指定されていないテーブルを使用する場合は、テーブルの SystemProperties に Version フラグを追加することでオプティミスティック同時実行制御を有効にします。
    >
    >`````
    //Enable optimistic concurrency by retrieving __version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
`````


2. By adding the `Version` property to the `TodoItem` class, the application will be notified with a `MobileServicePreconditionFailedException` exception during an update if the record has changed since the last query. This exception includes the latest version of the item from the server. In MainPage.cs for the shared project, add the following code to handle the exception in the `UpdateToDoItem()` method.

        private async Task UpdateToDoItem(TodoItem item)
        {
            Exception exception = null;
            try
            {
                //update at the remote table
                await todoTable.UpdateAsync(item);
            }
            catch (MobileServicePreconditionFailedException<TodoItem> writeException)
            {
                exception = writeException;
            }
            catch (Exception ex)
            {
                exception = ex;
            }
            if (exception != null)
            {
                if (exception is MobileServicePreconditionFailedException)
                {
                    //Conflict detected, the item has changed since the last query
                    //Resolve the conflict between the local and server item
                    await ResolveConflict(item, ((MobileServicePreconditionFailedException<TodoItem>) exception).Item);
                }
                else
                {
                    await new MessageDialog(exception.Message, "Update Failed").ShowAsync();
                }
            }
        }


3. In MainPage.cs, add the definition for the `ResolveConflict()` method referenced in `UpdateToDoItem()`. Notice that in order to resolve the conflict, you set the local item's version to the updated version from the server before committing the user's decision. Otherwise, you will continually encounter the conflict.


        private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
        {
            //Ask user to choose the resolution between versions
            MessageDialog msgDialog = new MessageDialog(String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
                                                        serverItem.Text, localItem.Text),
                                                        "CONFLICT DETECTED - Select a resolution:");
            UICommand localBtn = new UICommand("Commit Local Text");
            UICommand ServerBtn = new UICommand("Leave Server Text");
            msgDialog.Commands.Add(localBtn);
            msgDialog.Commands.Add(ServerBtn);
            localBtn.Invoked = async (IUICommand command) =>
            {
                // To resolve the conflict, update the version of the
                // item being committed. Otherwise, you will keep
                // catching a MobileServicePreConditionFailedException.
                localItem.Version = serverItem.Version;
                // Updating recursively here just in case another
                // change happened while the user was making a decision
                await UpdateToDoItem(localItem);
            };
            ServerBtn.Invoked = async (IUICommand command) =>
            {
                RefreshTodoItems();
            };
            await msgDialog.ShowAsync();
        }



##Test database write conflicts in the application

In this section you will build a Windows Store app package to install the app on a second machine or virtual machine. Then you will run the app on both machines generating a write conflict to test the code. Both instances of the app will attempt to update the same item's `text` property requiring the user to resolve the conflict.


1. Create a Windows Store app package to install on second machine or virtual machine. To do this, click **Project**->**Store**->**Create App Packages** in Visual Studio.

    ![][0]

2. On the Create Your Packages screen, click **No** as this package will not be uploaded to the Windows Store. Then click **Next**.

    ![][1]

3. On the Select and Configure Packages screen, accept the defaults and click **Create**.

    ![][10]

4. On the Package Creation Completed screen, click the **Output location** link to open the package location.

    ![][11]

5. Copy the package folder, "todolist_1.0.0.0_AnyCPU_Debug_Test", to the second machine. On that machine, open the package folder and right click on the **Add-AppDevPackage.ps1** PowerShell script and click **Run with PowerShell** as shown below. Follow the prompts to install the app.

    ![][12]

5. Run instance 1 of the app in Visual Studio by clicking **Debug**->**Start Debugging**. On the Start screen of the second machine, click the down arrow to see "Apps by name". Then click the **todolist** app to run instance 2 of the app.

    App Instance 1
    ![][2]

    App Instance 2
    ![][2]


6. In instance 1 of the app, update the text of the last item to **Test Write 1**, then click another text box so that the `LostFocus` event handler updates the database. The screenshot below shows an example.

    App Instance 1
    ![][3]

    App Instance 2
    ![][2]

7. At this point the corresponding item in instance 2 of the app has an old version of the item. In that instance of the app, enter **Test Write 2** for the `text` property. Then click another text box so the `LostFocus` event handler attempts to update the database with the old `_version` property.

    App Instance 1
    ![][4]

    App Instance 2
    ![][5]

8. Since the `__version` value used with the update attempt didn't match the server `__version` value, the Mobile Services SDK throws a `MobileServicePreconditionFailedException` allowing the app to resolve this conflict. To resolve the conflict, you can click **Commit Local Text** to commit the values from instance 2. Alternatively, click **Leave Server Text** to discard the values in instance 2, leaving the values from instance 1 of the app committed.

    App Instance 1
    ![][4]

    App Instance 2
    ![][6]



##Automatically handling conflict resolution in server scripts

You can detect and resolve write conflicts in server scripts. This is a good idea when you can use scripted logic instead of user interaction to resolve the conflict. In this section, you will add a server side script to the TodoItem table for the application. The logic this script will use to resolve conflicts is as follows:

+  If the TodoItem's ` complete` field is set to true, then it is considered completed and `text` can no longer be changed.
+  If the TodoItem's ` complete` field is still false, then attempts to update `text` will be comitted.

The following steps walk you through adding the server update script and testing it.

1. Log into the [Azure classic portal], click **Mobile Services**, and then click your app.

    ![][7]

2. Click the **Data** tab, then click the **TodoItem** table.

    ![][8]

3. Click **Script**, then select the **Update** operation.

    ![][9]

4. Replace the existing script with the following function, and then click **Save**.

        function update(item, user, request) {
            request.execute({
                conflict: function (serverRecord) {
                    // Only committing changes if the item is not completed.
                    if (serverRecord.complete === false) {
                        //write the updated item to the table
                        request.execute();
                    }
                    else
                    {
                        request.respond(statusCodes.FORBIDDEN, 'The item is already completed.');
                    }
                }
            });
        }
5. Run the **todolist** app on both machines. Change the TodoItem `text` for the last item in instance 2. Then click another text box so the `LostFocus` event handler updates the database.

    App Instance 1
    ![][4]

    App Instance 2
    ![][5]

6. In instance 1 of the app, enter a different value for the last text property. Then click another text box so the `LostFocus` event handler attempts to update the database with an incorrect `__version` property.

    App Instance 1
    ![][13]

    App Instance 2
    ![][14]

7. Notice that no exception was encountered in the app since the server script resolved the conflict allowing the update since the item is not marked complete. To see that the update was truly successful, click **Refresh** in instance 2 to re-query the database.

    App Instance 1
    ![][15]

    App Instance 2
    ![][15]

8. In instance 1, click the check box to complete the last Todo item.

    App Instance 1
    ![][16]

    App Instance 2
    ![][15]

9. In instance 2, try to update the last TodoItem's text and trigger the `LostFocus` event. In response to the conflict, the script resolved it by refusing the update because the item was already completed.

    App Instance 1
    ![][17]

    App Instance 2
    ![][18]

##Next steps

This tutorial demonstrated how to enable a Windows Store app to handle write conflicts when working with data in Mobile Services. Next, consider completing one of the following Windows Store tutorials:

* [Add authentication to your app]
  <br/>Learn how to authenticate users of your app.

* [Add push notifications to your app]
  <br/>Learn how to send a very basic push notification to your app with Mobile Services.



<!-- Images. -->
[0]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package1.png
[1]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package2.png
[2]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1.png
[3]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write1.png
[4]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write2.png
[5]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write2.png
[6]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write2-conflict.png
[7]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-services-selection.png
[8]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-portal-data-tables.png
[9]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-insert-script-users.png
[10]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package3.png
[11]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-create-app-package4.png
[12]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-install-app-package.png
[13]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app1-write3.png
[14]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-app2-write3.png
[15]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-write3.png
[16]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-checkbox.png
[17]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-2-items.png
[18]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/Mobile-oc-store-already-complete.png
[19]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-manage-nuget-packages-VS.png
[20]: ./media/mobile-services-windows-store-dotnet-handle-database-conflicts/mobile-manage-nuget-packages-dialog.png

<!-- URLs. -->
[Optimistic Concurrency Control]: http://go.microsoft.com/fwlink/?LinkId=330935
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Azure Account]: http://www.windowsazure.com/pricing/free-trial/
[Validate and modify data with scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[Refine queries with paging]: /develop/mobile/tutorials/add-paging-to-data-dotnet
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Add authentication to your app]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Add push notifications to your app]: /develop/mobile/tutorials/get-started-with-push-dotnet

[Azure classic portal]: https://manage.windowsazure.com/
[Windows Phone 8 SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268374
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkID=268375
[Developer Code Samples site]:  http://go.microsoft.com/fwlink/p/?LinkId=271146
[System Properties]: http://go.microsoft.com/fwlink/?LinkId=331143


