<properties 
    pageTitle="Azure RemoteApp で USB デバイスをリダイレクトする方法 | Microsoft Azure" 
    description="Azure RemoteApp で USB デバイスのリダイレクトを使用する方法について説明します。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="elizapo" />



# Azure RemoteApp で USB デバイスをリダイレクトする方法

デバイス リダイレクトを使用すると、ユーザーは、コンピューターやタブレットに接続された USB デバイスを Azure RemoteApp のアプリで利用できます。 たとえば、Azure RemoteApp を利用して Skype を共有した場合、ユーザーはデバイスのカメラを使用できるようにする必要があります。

さらに移動する前に、USB リダイレクト情報を確認してください [Azure RemoteApp でリダイレクトを使用して](remoteapp-redirection.md)します。 ただし、nusbdevicestoredirect:s をお勧めします。 * USB web カメラ用が動作しなくなり、一部の USB プリンターまたは USB 多機能デバイスのは使用できません。 仕様やセキュリティ上の理由により、Azure RemoteApp の管理者は、デバイス クラス GUID またはデバイス インスタンス ID を使用してリダイレクトを有効にしないと、ユーザーがこれらのデバイスを使用できません。

USB プリンターやとリダイレクトされない他の USB 多機能デバイスをリダイレクトするような手法を使用することができますが、この記事は、web カメラのリダイレクトについては説明、 **nusbdevicestoredirect:s:*** コマンドです。

## USB デバイスのリダイレクト オプション
Azure RemoteApp では、USB デバイスをリダイレクトするために、リモート デスクトップ サービスで使用できるものと非常によく似たメカニズムが使用されています。 基になるテクノロジでは、高レベルの両方の長所を取得する特定のデバイスと RemoteFX USB デバイスのリダイレクトを適切な方法を選択できます。 リダイレクトを使用して、 **usbdevicestoredirect:s:** コマンドです。 このコマンドに対する要素は 4 つあります。

| 処理の順序 | パラメーター           | 説明                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | 高度なリダイレクトで取得されないすべてのデバイスを選択します。 注: 仕様により、* は USB Web カメラには使用できません。  |
|                  | {Device class GUID} | 指定されたデバイス セットアップ クラスと一致するすべてのデバイスを選択します。                                                           |
|                  | USB\InstanceID      | 特定のインスタンス ID に指定された USB デバイスを選択します。                                                                  |
| 2                | -USB\Instance ID    | 指定されたデバイスのリダイレクト設定を削除します。                                                                 |

## デバイス クラス GUID を使用した USB デバイスのリダイレクト
リダイレクトに使用できるデバイス クラス GUID を検索する方法は 2 とおりあります。 

使用する 1 つ目は、 [[システム定義デバイス セットアップ クラス使用可能な仕入先に](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx)します。 ローカル コンピューターに接続されているデバイスに対して最も適合性が高いクラスを選択します。 デジタル カメラの場合、これは、イメージング デバイス クラスかキャプチャ デバイス クラスになります。 ローカルに接続された USB デバイス (この場合は Web カメラ) で動作する適切なクラス GUID を見つけるには、デバイス クラスでいくつかの実験を行う必要があります。

さらに優れた方法、つまり 2 番目のオプションでは、次の手順に従って、特定のデバイス クラス GUID を見つけます。

1. デバイス マネージャーを開き、リダイレクトされ、右クリックして、デバイスを検索してプロパティを開きます。
![Open the Device Manager](./media/remoteapp-usbredir/ra-devicemanager.png)
2.  **詳細** ] タブで、プロパティを選択 **クラス Guid**します。 表示される値は、デバイスの種類のクラス GUID です。
![カメラのプロパティ](./media/remoteapp-usbredir/ra-classguid.png)
3. このクラス GUID 値を使用して、それに対応するデバイスをリダイレクトします。

次に例を示します。

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

複数のデバイス リダイレクトを 1 つのコマンドレットにまとめることができます。 たとえば、ローカル ストレージと USB Web カメラをリダイレクトする場合、コマンドレットは次のようになります。

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

クラス GUID でデバイス リダイレクトを設定すると、指定されたコレクション内でそのクラス GUID に対応するすべてのデバイスがリダイレクトされます。 たとえば、ローカル ネットワーク上の複数のコンピューターに同じ USB Web カメラが搭載されている場合、1 つのコマンドレットを実行して、すべての Web カメラをリダイレクトすることができます。

## デバイス インスタンス ID を使用した USB デバイスのリダイレクト

より細かく制御したい場合やデバイスごとにリダイレクトを制御する場合は、**USB\InstanceID** リダイレクト パラメーターを使用できます。

この方法の最も難しい部分は、USB デバイス インスタンス ID を見つけるところです。 コンピューターと特定の USB デバイスへのアクセス権が必要になります。 その後、次の手順に従います。

1. 」の説明に従って、リモート デスクトップ セッションでデバイスのリダイレクトを有効にする [使用して方法は自分のデバイスとリソース、リモート デスクトップ セッションのでしょうか。](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. リモート デスクトップ接続を開き、をクリックして **オプションを表示**します。
3. クリックして **として保存** を現在の接続設定を RDP ファイルに保存します。  
    ![設定を RDP ファイルとして保存します。](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. ファイル名と場所 (たとえば、"MyConnection.rdp" と "This PC\Documents") を選択し、ファイルを保存します。
5. テキスト エディターで MyConnection.rdp ファイルを開き、リダイレクトするデバイスのインスタンス ID を検索します。

ここで、見つかったインスタンス ID を次のコマンドレットで使用します。

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### サポートのお願い 
記事を評価したり、下にコメントを投稿したりするだけでなく、記事自体を変更できることを知っていましたか。 説明不足ですか。 間違いがありますか。 わかりにくいことが書いてありますか。 上にスクロールしてクリックして **GitHub の編集** の変更を加えるについては、私たちに来るし、次に、筆に一度表示されます、変更および強化ここです。
