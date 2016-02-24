<properties 
    pageTitle="NewTek TriCaster エンコーダーを構成して、単一ビットレートのライブ ストリームを送信する" 
    description="このトピックでは、TriCaster ライブ エンコーダーを構成して、ライブ エンコードが有効になっている AMS チャネルに単一ビットレートのストリームを送信する方法を示します。" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako,cenkdin,anilmur" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/15/2015"  
    ms.author="juliako"/>

#NewTek TriCaster エンコーダーを使用して、単一ビットレートのライブ ストリームを送信する

> [AZURE.SELECTOR]
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Elemental Live](media-services-configure-elemental-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

このトピックは、構成する方法を示します、 [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) ライブ エンコードのチャンネルを AMS に単一ビットレートのストリームを送信するライブ エンコーダーが有効になっています。 詳細については、次を参照してください。 [Azure Media Services によるライブ エンコードの実行を有効になっているチャネルを操作](media-services-manage-live-encoder-enabled-channels.md)します。

このチュートリアルでは、Azure Media Service Explorer (AMSE) ツールを使用して、Azure Media Services (AMS) を管理する方法を示します。 このツールは、Windows PC でのみ実行されます。 Mac または Linux の場合は、Azure クラシック ポータルを使用して作成する [チャネル](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) と [プログラム](media-services-portal-creating-live-encoder-enabled-channel#create-and-manage-a-program)します。

##前提条件

- [Azure Media Services アカウントの作成](media-services-create-account.md)
- 1 つ以上のストリーミング ユニットが割り当てられたストリーミング エンドポイントが実行中であることを確認します。 詳細については、次を参照してください [Media Services アカウントでストリーミング エンドポイントを管理。](media-services-manage-origins.md)
- 最新バージョンのインストール、 [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) ツールです。
- ツールを起動し、AMS アカウントに接続します。

##ヒント

- 可能な限り、有線のインターネット接続を使用します。
- 帯域幅要件の目安は、ストリーミングのビットレートの 2 倍です。 これは必須の要件ではありませんが、ネットワークの混雑の影響を軽減することができます。
- ソフトウェア ベースのエンコーダーを使用する際は、不要なプログラムを終了します。

## チャネルの作成

1.  AMSE ツールに移動し、 **Live** タブをクリックし、チャネルの領域内を右クリックします。 選択 **... チャンネルを作成** ] メニューからです。

![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. チャネルの名前を指定します。説明フィールドは省略可能です。 [チャネルの設定] [ **標準** ライブ エンコードのオプションで、入力プロトコルに設定を **RTMP**します。 それ以外の設定はすべてそのままにしておくことができます。


必ず、 **新しいチャネルを今すぐ開始** が選択されています。

3. クリックして **チャネルの作成**します。
![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

>[AZURE.NOTE] チャネルには開始するまで 20 分程度かかることができます。


チャネルの起動中にすることができます [エンコーダーを構成する](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp)です。

>[AZURE.IMPORTANT] チャネルが準備完了状態になるとすぐに課金が開始することに注意してください。 詳細については、次を参照してください。 [チャネルの状態の](media-services-manage-live-encoder-enabled-channels.md#states)です。

##<a id=configure_tricaster_rtmp></a>NewTek tricaster の各エンコーダーを構成します。

このチュートリアルでは、次の出力設定が使用されます。 このセクションの残りの部分では、構成の手順の詳細について説明します。 

**ビデオ**:
 
- コーデック: H.264 
- プロファイル: 高 (レベル 4.0) 
- ビットレート: 5000 kbps 
- キーフレーム: 2 秒 (60 秒) 
- フレーム レート: 30
 
**オーディオ**:

- コーデック: AAC (LC) 
- ビットレート: 192 kbps 
- サンプル レート: 44.1 kHz


###構成の手順

1. 新規作成 **NewTek TriCaster** によってどのようなビデオ入力ソースが使用されているプロジェクト。 
2. 1 回、そのプロジェクト内で検索、 **ストリーム** ボタンをクリックし、ストリームの構成] メニューにアクセスするようの横にある歯車アイコンをクリックします。

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. クリックして、メニューが開いたら、 **新規** 接続という見出しの下です。 接続の種類のメッセージが表示されたら、[ **Adobe Flash**します。

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)

4. Click **OK**.

5. ドロップダウンの下の矢印をクリックして FMLE プロファイルをインポートできるようになりました **プロファイルのストリーミング** への移動 **参照**します。

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)

6. 構成済みの FMLE プロファイルを保存した場所に移動します。
7. 選択し、キーを押して **OK**します。

    プロファイルがアップロードされたら、次の手順に進みます。

6. Get、チャネルでの URL を入力、Tricaster に割り当てるために **RTMP エンドポイント**します。
    
    AMSE ツールに戻り、チャネルの完了状態を確認します。 状態が変わると **開始** に **を実行している**, 、入力の URL を取得することができます。
      
    チャネル名を右クリックして、経由でのホバー時に移動して、チャネルが実行されているときに **をクリップボードにコピー入力 URL** し、[ **主な入力
   URL**します。  
    
    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)

7. この情報を貼り付け、 **場所** フィールド **Flash Server** tricaster の各プロジェクト内です。 ストリーム名を割り当てることも、 **ストリーム ID** フィールドです。 

    FMLE プロファイルにストリームの情報が追加された場合、インポートすることもここをクリックして **設定のインポート**, 保存されている FMLE プロファイルに移動して、クリックすると **OK**します。 関連する Flash Server フィールドに、FMLE からの情報が入力されます。

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)

9. 終了したら、クリックして **OK** 、画面の下部にあります。 ビデオおよびオーディオの入力、Tricaster には、準備が整ったらをクリックして、AMS にストリーミングが開始、 **ストリーム** ] ボタンをクリックします。

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

>[AZURE.IMPORTANT] クリックする前に **ストリーム**, 、する **必要があります** チャネル準備ができていることを確認します。 
>また、15 分を超える入力投稿フィードがある場合を除き、チャネルを準備可能のままにしないでください。 

##再生をテストする
  
1. AMSE ツールに移動し、テストするチャネルを右クリックします。 メニューから、ポインターを合わせる **再生プレビュー** 選択 **Azure Media Player で**します。  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

ストリームがプレーヤーに表示されている場合は、エンコーダーが AMS に接続するように正しく構成されています。 

エラーが表示される場合は、チャネルをリセットして、エンコーダー設定を調整する必要があります。 参照してください、 [トラブルシューティング](media-services-troubleshooting-live-streaming.md) ガイダンスのトピックです。  

##プログラムを作成する

1. チャネルの再生が確認されたら、プログラムを作成します。 下にある、 **Live** AMSE ツール] タブで、プログラムの領域内を右クリックし、選択 **新しいプログラムの作成**します。  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)

2. プログラムの名前し、必要な場合は、調整、 **アーカイブ ウィンドウの長さ** (既定で 4 時間) です。 ストレージの場所を指定することも、既定値のままにすることもできます。  
3. チェック、 **プログラムを今すぐ開始** ボックス。
4. クリックして **プログラムを作成する**です。  
  
    注: プログラムの作成は、チャネルの作成ほど時間はかかりません。    
 
5. プログラムが実行されると、再生の確認を右クリックして、プログラムに移動する **再生プログラム** を選択して **Azure Media Player で**します。  
6. 確認すると、プログラムもう一度右クリックし、選択 **出力 URL をクリップボードにコピー** (からこの情報を取得または、 **情報と設定をプログラム** メニューのオプション)。 

これで、ストリームをプレーヤーに埋め込んだり、ライブ表示のために対象ユーザーに配信したりできるようになりました。  


## トラブルシューティング

参照してください、 [トラブルシューティング](media-services-troubleshooting-live-streaming.md) ガイダンスのトピックです。 


##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

