<properties 
    pageTitle="Azure Mobile Engagement - バックエンドの統合" 
    description="SharePoint バックエンドに Azure Mobile Engagement を接続して SharePoint からキャンペーンを作成する" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/12/2015" 
    ms.author="piyushjo" />


# Azure Mobile Engagement - API の統合

自動化されたマーケティング システムでは、マーケティング キャンペーンの作成とアクティブ化も自動で行われます。 この目的のために、Azure Mobile Engagement では API を使用して自動マーケティング キャンペーンを作成することもできます。

一般に顧客は、マーケティング キャンペーンの一環として、Mobile Engagement のフロント エンド インターフェイスを使用してアナウンスやポーリングなどを作成します。 しかし、マーケティング キャンペーンが成熟するにつれて、完全に自動化されたパイプラインを作成し、バックエンド システム (CRM システムや、SharePoint のような CMS システムなど) から入力されたデータに基づいて Mobile Engagement でキャンペーンを動的に作成できるようにするために、バックエンド システムに封じ込められたデータを活用することが必要になります。

![][5]

このチュートリアルで扱うシナリオでは、まず SharePoint のビジネス ユーザーが SharePoint リストにマーケティング データを入力します。そして、自動化されたプロセスが利用可能な REST API を使用してこのリストから項目を取得し、Mobile Engagement システムと通信して、SharePoint のデータからマーケティング キャンペーンを作成します。
> [AZURE.IMPORTANT] おおまかに言うと、このサンプルでは認証およびパラメーターの受け渡しという、API の呼び出しに関する 2 つの主要な側面を詳細に扱っているため、ここを出発点として、あらゆる Mobile Engagement REST API を呼び出す方法を身に付けることができます。 

## SharePoint の統合

1. サンプルの SharePoint リストは次のようなものです。 **Title**、**Category**、**NotificationTitle**、**Message**、**URL** はアナウンスの作成に使用されます。 **IsProcessed** という名前の列がありますが、これはコンソール プログラム形式のサンプル自動化プロセスで使用されます。 このコンソール プログラムを Azure WebJob として実行しスケジュールを設定することも、SharePoint ワークフローを直接使用して、この SharePoint リストに項目が挿入された場合にアナウンスが作成およびアクティブ化されるように設定することもできます。 このサンプルで使用するコンソール プログラムは、SharePoint リスト内の項目を調べ、項目ごとに Azure Mobile Engagement でアナウンスを作成してから、最後にアナウンスの作成が成功した場合に **IsProcessed** フラグを true にマークします。

    ![][1]

2. サンプルのコードを使用する *SharePoint クライアント オブジェクト モデルを使用してオンラインでのリモート認証* [ここ](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) 、SharePoint リストを認証します。

3. 認証後に、リスト項目内をループして新規に作成された項目 (**IsProcessed** が false となっています) を検索します。

     static async void CreateCampaignFromSharepoint()
     {
         using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
         {
             // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
             Web site = clientContext.Web;
             List list = site.Lists.GetByTitle("VideoContent");
             CamlQuery query = new CamlQuery();
             query.ViewXml = "<View/>";
             ListItemCollection items = list.GetItems(query);
    
             // Load the SharePoint list
             clientContext.Load(list);
             clientContext.Load(items);
             clientContext.ExecuteQuery();
    
             // Loop through the list to go through all the items which are newly added
             foreach (ListItem item in items)
                 if (item["IsProcessed"].ToString() != "Yes")
                 {
                     string name = item["Title"].ToString();
                     string notificationTitle = item["NotificationTitle"].ToString();
                     string notificationMessage = item["Message"].ToString();
                     string category = item["Category"].ToString();
                     string actionURL = ((FieldUrlValue)item["URL"]).Url;
    
                     // Send an HTTP request to create AzME campaign
                     int campaignId = CreateAzMECampaign
                         (name, notificationTitle, notificationMessage, category, actionURL).Result;
    
                     if (campaignId != -1)
                     {
                         // If creating campaign is successful then set this to true
                         item["IsProcessed"] = "Yes";
    
                         // Now try to activate the campaign also
                         await ActivateAzMECampaign(campaignId);
                     }
                     else
                     {
                         item["IsProcessed"] = "Error";
                     }
                     item.Update();
                 }
             clientContext.ExecuteQuery();
         }
     }


## Mobile Engagement の統合

1.  リスト項目と呼び出しからのお知らせの作成に必要な情報を抽出処理を必要とする項目を発見 `CreateAzMECampaign` それを作成して、その `ActivateAzMECampaign` アクティブにします。 これらは、本質的には Mobile Engagement バックエンドを呼び出す REST API 呼び出しです。

2.  Mobile Engagement REST Api が必要な **基本認証スキームの HTTP 承認ヘッダー** ので構成されています、 `ApplicationId` と `ApiKey` Azure ポータルから入手します。 **[SDK キー]** セクション*ではなく*、**[API キー]** セクションのキーを使用していることを確認します。

![][2]

    static string CreateAuthZHeader()
    {
        string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
        return BasicAuthzHeaderString;
    }
    
    static public string EncodeTo64(string toEncode)
    {
        byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
        string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
        return returnValue;
    }  

3. お知らせを作成するためキャンペーンを入力 - を参照してください、 [ドキュメント](https://msdn.microsoft.com/library/dn913754.aspx)します。 キャンペーンを指定しているかどうかを確認する必要があります。 `種類` として *アナウンス* と [ペイロード](https://msdn.microsoft.com/library/dn913749.aspx) FormUrlEncodedContent として渡すこととします。

     static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
         string notificationMessage, string notificationCategory, string actionURL)
     {
         string createURIFragment = "/reach/1/create";
    
         using (var client = new HttpClient())
         {
             // Add Authorization Header
             client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
    
             // Create the payload to send the content
             // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
             string data =
                 @"{""name"":""" + campaignName + @"""," + 
                 @"""type"":""only_notif""," + 
                 @"""deliveryTime"":""any"","" + 
                 @""notificationTitle"":""" + notificationTitle + 
                 @""",""notificationMessage"":""" + notificationMessage + 
                 @""",""actionUrl"":""" + actionURL + @"""}";
    
             var content = new FormUrlEncodedContent(new[] 
             {
                 new KeyValuePair<string, string>("kind", "announcement"),
                 new KeyValuePair<string, string>("data", data),
             });
    
             // Send the POST request
             var response = await client.PostAsync(url + createURIFragment, content);
             var responseString = response.Content.ReadAsStringAsync().Result;
             int campaignId = -1;
             if (response.StatusCode.ToString() == "OK")
             {
                 // This is the campaign id
                 campaignId = Convert.ToInt32(responseString);
                 Console.WriteLine("Campaign successfully created with id {0}", campaignId);
             }
             else
             {
                 Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
             }
             return campaignId;
         }
     }

4. お知らせを作成すると、Mobile Engagement ポータルに次のような結果が表示されます (State が Draft であり、Acitivated が N/A であることに注意してください)。

    ![][3]

5. `CreateAzMECampaign` アナウンス キャンペーンを作成し、呼び出し元にその Id を返します。 `ActivateAzMECampaign` キャンペーンのアクティブ化パラメーターとしてこの Id が必要です。

     static async Task<bool> ActivateAzMECampaign(int campaignId)
     {
         string activateUriFragment = "/reach/1/activate";
         using (var client = new HttpClient())
         {
             // Add Authorization Header
             client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
    
             var content = new FormUrlEncodedContent(new[] 
             {
                 new KeyValuePair<string, string>("kind", "announcement"),
                 new KeyValuePair<string, string>("id", campaignId.ToString()),
             });
    
             // Send the POST request
             var response = await client.PostAsync(url + activateUriFragment, content);
             var responseString = response.Content.ReadAsStringAsync().Result;
             if (response.StatusCode.ToString() == "OK")
             {
                 Console.WriteLine("Campaign successfully activated");
                 return true;
             }
             else
             {
                 Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                 return false;
             }
         }
     }

6. お知らせを作成すると、Mobile Engagement ポータルに次のような結果が表示されます。

    ![][4]

7. キャンペーンがアクティブ化されるとすぐに、キャンペーンの基準を満たすデバイスで通知が表示されるようになります。

8. アナウンス キャンペーンが作成されると、IsProcessed が false であったリスト項目が true に設定されることにも注目してください。

このサンプルでは、ほとんどの必須プロパティを指定する単純なアナウンス キャンペーンを作成しました。 これを情報を使用して、ポータルからできる限りでもカスタマイズ [ここ](https://msdn.microsoft.com/library/dn913749.aspx)します。



[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png 
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png 
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png 
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png 
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png 

