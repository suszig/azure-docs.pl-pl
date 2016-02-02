<properties
    pageTitle="Notification Hubs ローカライズ ニュース速報のチュートリアル"
    description="ローカライズ ニュース速報通知を Azure Notification Hubs を使用して送信する方法について説明します。"
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/15/2015" 
    ms.author="wesmc"/>


# 通知ハブを使用したローカライズ ニュース速報の送信

> [AZURE.SELECTOR]
- [Windows Store C#](notification-hubs-windows-store-dotnet-send-localized-breaking-news.md)
- [iOS](notification-hubs-ios-send-localized-breaking-news.md)


## 概要

このトピックでは、Azure 通知ハブの**テンプレート**機能を使用して、言語およびデバイスごとにローカライズしたニュース速報通知をブロードキャストする方法について説明します。 このチュートリアルでは、[通知ハブを使用したニュース速報の送信] で作成した Windows ストア アプリから開始できます。 完了すると、興味のあるニュース速報カテゴリに登録して受信する通知の言語を指定し、選択したカテゴリのその言語のプッシュ通知だけを受信できるようになります。


このシナリオは次の 2 つに分けられます。

- Windows ストア アプリケーションで、クライアント デバイスが言語を指定し、さまざまなニュース速報カテゴリを購読できるようになります。

- Azure Notification Hubs の**タグ**機能と**テンプレート**機能を使用して、バックエンドから通知がブロードキャストされます。



## 前提条件

既に行うには、[通知ハブを使用したニュース速報の送信] チュートリアルこのチュートリアルは、そのコードに基づいているため、使用可能なコードがあるとします。

Visual Studio 2012 以降も必要です。


## テンプレートの概念

[通知ハブを使用したニュース速報の送信] で使用するアプリケーションをビルドした **タグ** さまざまなニュース カテゴリの通知をサブスクライブします。
しかし、多くのアプリケーションは複数の市場をターゲットとしており、ローカライズが必要です。 これは、通知自体の内容をローカライズし、適切なデバイス セットに配信する必要があることを意味します。
このトピックでは、Notification Hubs の**テンプレート**機能を使用して、ローカライズしたニュース速報通知を簡単に配信する方法について説明します。

注: ローカライズした通知を送信する 1 つの方法として、各タグの複数のバージョンを作成する方法があります。 たとえば、ワールド ニュースで英語、フランス語、標準中国語をサポートするには、3 つのタグ ("world_en"、"world_fr"、"world_ch") が必要です。 その後、ワールド ニュースのローカライズ バージョンをこれらの各タグに送信する必要があります。 このトピックでは、テンプレートを使用することでタグの増加を抑制し、複数のメッセージを送信しなくてもよいようにします。

大まかに言えば、テンプレートとは、特定のデバイスが通知をどのように受信するかを特定するための手段です。 テンプレートは、アプリケーション バックエンドにより送信されるメッセージの一部となっているプロパティを参照することで、正確なペイロードを指定します。 このケースでは、サポートされるすべての言語を含む、ロケールにとらわれないメッセージを送信します。

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

次に、デバイスが、適切なプロパティを参照するテンプレートを登録するようにします。 たとえば、簡単なトースト メッセージを受信する Windows ストア アプリケーションは、次の対応するタグを持つテンプレートを登録します。

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>

テンプレートは非常に強力な機能についての詳細で、 [テンプレート](notification-hubs-templates.md) 記事です。


## アプリケーションのユーザー インターフェイス

[通知ハブを使用したニュース速報の送信]」で作成したニュース速報アプリケーションを今すぐ変更を送信するローカライズしたニュース速報テンプレートを使用します。

Windows ストア アプリケーションで、以下の手順を実行します。

MainPage.xaml を変更してロケール コンボボックスを追加します。

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## Windows ストア クライアント アプリケーションを構築する

1. Notifications クラス内にロケール パラメーターを追加、  *StoreCategoriesAndSubscribe* と *SubscribeToCateories* メソッドです。

     public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
     {
         ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
         ApplicationData.Current.LocalSettings.Values["locale"] = locale;
         return await SubscribeToCategories(categories);
     }
    
     public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
     {
         var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
         if (categories == null)
         {
             categories = RetrieveCategories();
         }
    
         // Using a template registration. This makes supporting notifications across other platforms much easier.
         // Using the localized tags based on locale selected.
         string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
    
         return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
     }

 *RegisterNativeAsync* メソッドを呼び出す代わりに *RegisterTemplateAsync* を呼び出す点に注意してください。テンプレートがロケールに依存している特定の通知形式を登録します。 さらに、テンプレートの名前 ("localizedWNSTemplateExample") も指定します。複数のテンプレートを登録する可能性があり (たとえば、トースト通知用のテンプレートとタイル通知用のテンプレートなど)、それらを更新または削除するには名前を付ける必要があるためです。

 同じタグを持つ複数のテンプレートがデバイスによって登録された場合、そのタグをターゲットとするメッセージが受信されると複数の通知がデバイスに配信される点に注意してください (テンプレートごとに 1 つずつ)。 この動作は、同じ論理メッセージによって複数のビジュアル通知を生成する必要がある場合に役立ちます。たとえば、Windows ストア アプリケーションにバッジとトーストの両方を表示する場合などです。

2. 次のメソッドを追加して、格納されたロケールを取得します。

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. 次に示すように、MainPage.xaml.cs でロケール コンボ ボックスの現在の値を取得し、それを Notifications クラスへの呼び出しに提供することで、ボタン クリック ハンドラーを更新します。

     private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
     {
         var locale = (string)Locale.SelectedItem;
    
         var categories = new HashSet<string>();
         if (WorldToggle.IsOn) categories.Add("World");
         if (PoliticsToggle.IsOn) categories.Add("Politics");
         if (BusinessToggle.IsOn) categories.Add("Business");
         if (TechnologyToggle.IsOn) categories.Add("Technology");
         if (ScienceToggle.IsOn) categories.Add("Science");
         if (SportsToggle.IsOn) categories.Add("Sports");
    
         var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
              categories);
    
         var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
             string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
         dialog.Commands.Add(new UICommand("OK"));
         await dialog.ShowAsync();
     }

4. 最後に、App.xaml.cs ファイルで必ず更新して、 `InitNotificationsAsync` ロケールを取得し、サブスクライブしているときに使用する方法。

     private async void InitNotificationsAsync()
     {
         var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
    
         // Displays the registration ID so you know it was successful
         if (result.RegistrationId != null)
         {
             var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
             dialog.Commands.Add(new UICommand("OK"));
             await dialog.ShowAsync();
         }
     }



## バックエンドからローカライズした通知を送信する

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]











[template concepts]: #concepts 
[the app user interface]: #ui 
[building the windows store client app]: #building-client 
[send notifications from your back-end]: #send 
[next steps]: #next-steps 
[mobile service]: /develop/mobile/tutorials/get-started 
[notify users with notification hubs: asp.net]: /manage/services/notification-hubs/notify-users-aspnet 
[notify users with notification hubs: mobile services]: /manage/services/notification-hubs/notify-users 
[use notification hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: /develop/mobile/tutorials/get-started/#create-new-service 
[get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet 
[get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet 
[push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet 
[authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet 
[javascript and html]: /develop/mobile/tutorials/get-started-with-push-js 
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591 
[notification hubs guidance]: http://msdn.microsoft.com/library/jj927170.aspx 
[notification hubs how-to for ios]: http://msdn.microsoft.com/library/jj927168.aspx 
[notification hubs how-to for windows store]: http://msdn.microsoft.com/library/jj927172.aspx 

