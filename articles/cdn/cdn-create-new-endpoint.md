<properties 
     pageTitle="Azure の Content Delivery Network (CDN) を有効にする方法" 
     description="このトピックでは、Azure の Content Delivery Network (CDN) を有効にする方法を説明します。" 
     services="cdn" 
     documentationCenter="" 
     authors="camsoper" 
     manager="dwrede" 
     editor=""/>
<tags 
     ms.service="cdn" 
     ms.workload="media" 
     ms.tgt_pltfrm="na" 
     ms.devlang="na" 
     ms.topic="article" 
     ms.date="12/02/2015" 
     ms.author="casoper"/>



#Azure の Content Delivery Network (CDN) を有効にする方法  

Azure 管理ポータルを使用して送信元の CDN を有効にできます。 Web Apps、BLOB ストレージ、Cloud Services など多数の種類の統合された Azure 送信元がサポートされています。 Azure Media Services ストリーミング エンドポイントに対して CDN を有効にすることもできます。  配信元がこれらの Azure サービスのいずれでもない場合、または Azure 外の場所でホストされている場合は、カスタムの配信元を作成することもできます。  配信元の CDN エンドポイントを有効にすると、パブリックにアクセスできるオブジェクトはすべて CDN エッジ キャッシュの対象になります。

## 新しい CDN プロファイルを作成する

CDN プロファイルは、CDN エンドポイントのコレクションです。  各プロファイルには、1 つ以上の CDN エンドポイントが含まれます。  複数のプロファイルを使って、インターネット ドメイン、Web アプリケーション、またはその他の一部の基準別に CDN エンドポイントを整理する必要が生じる場合があります。

> [AZURE.NOTE] 1 つの Azure サブスクリプションは、次の 4 つの CDN プロファイルに限定されます。 各 CDN プロファイルは、4 つの CDN エンドポイントに制限されます。
>
> CDN の価格は、CDN プロファイル レベルで適用されます。 Standard と Premium の CDN 機能を組み合わせて使用する場合は、複数の CDN プロファイルが必要になります。


**新しい CDN プロファイルを作成するには**

1.  [Azure 管理ポータル](https://portal.azure.com), 、左上でクリックして **新規**します。   **新規** ブレードで、 **メディア + CDN**, 、し **CDN**します。

    新しい CDN プロファイル ブレードが表示されます。
    
    ![新しい CDN プロファイル][new-cdn-profile]

2. CDN プロファイルの名前を入力します。 

3. 選択、 **価格レベル** または既定値を使用します。

4. 選択または作成、 **リソース グループ**します。  リソース グループの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview/#resource-groups)します。 

5. 選択、 **サブスクリプション** この CDN プロファイルの。

6. 選択、 **場所**します。  これは、CDN プロファイル情報が格納される Azure の場所です。  CDN エンドポイントの場所には影響しません。  ストレージ アカウントと同じ場所である必要はありません。

7. クリックして、 **作成** 新しいプロファイルを作成する] ボタンをクリックします。

## 新しい CDN エンドポイントを作成する

**ストレージ アカウントに対する新しい CDN エンドポイントを作成するには**

1.  [Azure 管理ポータル](https://portal.azure.com), 、CDN プロファイルに移動します。  これは、前の手順でダッシュボードにピン留めしている可能性があります。  かどうかは表示されていない、することができますをクリックして **参照**, 、し、 **CDN プロファイル**, 、ボードを使用するエンドポイントを追加する予定のプロファイル] をクリックします。

    CDN プロファイル ブレードが表示されます。
    
    ![CDN プロファイル][cdn-profile-settings]
    
2. クリックして、 **エンドポイントの追加** ] ボタンをクリックします。

    ![[エンドポイントの追加] ボタン][cdn-new-endpoint-button]

     **エンドポイントを追加** ブレードが表示されます。
    
    ![[エンドポイントの追加] ブレード][cdn-add-endpoint]

3. 入力、 **名前** この CDN エンドポイントです。  この名前は、ドメイン `<EndpointName>.azureedge.net` でキャッシュされたリソースにアクセスする際に使用します。

4.  **配信元の種類** ドロップダウン リストで、送信元の種類を選択します。
    
    ![CDN 配信元の種類](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5.  **配信元のホスト名** ] ボックスの一覧を選択するか、元のドメインを入力します。  ドロップダウンに、手順 4 で指定した種類の利用可能な配信元がすべて一覧表示されます。  選択した場合は *カスタム配信元* として、 **配信元の種類**, 、カスタムの配信元のドメイン内に入力されます。

6.  **オリジン パス** テキスト ボックスに、キャッシュするか、手順 5. で指定されたドメインでリソースのキャッシュを許可するように空白のままにリソースへのパスを入力します。

7.  **Origin ヘッダーがホスト**, 、各要求と一緒に送信する CDN が必要なホスト ヘッダーを入力するか、既定のままにします。

8.  **プロトコル** と **送信元ポート**, 、プロトコルと原点にリソースにアクセスするために使用するポートを指定します。  クライアントは CDN 上のリソースにアクセスするときに、引き続きこれらの同じプロトコルとポートを使用します。  少なくとも 1 つのプロトコル (HTTP または HTTPS) を選択する必要があります。

9. クリックして、 **追加** 新しいエンドポイントを作成する] ボタンをクリックします。

10. エンドポイントが作成されると、プロファイルのエンドポイントの一覧に表示されます。 一覧には、キャッシュされたコンテンツへのアクセスに使用する URL と元のドメインが表示されます。

    ![CDN エンドポイント][cdn-endpoint-success]

    > [AZURE.NOTE] エンドポイントはすぐにはできません使用します。  登録が CDN ネットワークに反映されるまでに最大で 90 分かかる場合があります。 CDN ドメイン名を直ちに使用しようとするユーザーは、CDN を経由してコンテンツを取得できるようになるまでは状態コード 404 を受け取る場合があります。

##関連項目
- [カスタム ドメインに Content Delivery Network (CDN) コンテンツをマップする方法](cdn-map-content-to-custom-domain.md)
- [Azure CDN エンドポイントの消去](cdn-purge-endpoint.md)

[new-cdn-profile]: ./media/cdn-create-new-endpoint/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png 
 
