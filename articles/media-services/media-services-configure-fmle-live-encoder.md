<properties 
    pageTitle="FMLE エンコーダーを使用して、単一ビットレートのライブ ストリームを構成する" 
    description="このトピックでは、Flash Media Live Encoder (FMLE) エンコーダーを構成して、ライブ エンコードが有効になっている AMS チャネルに単一ビットレートのストリームを送信する方法を示します。" 
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

#FMLE エンコーダーを使用して、単一ビットレートのライブ ストリームを送信する

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Elemental Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

このトピックで説明を構成する方法、 [Flash メディアのライブ エンコーダー](http://www.adobe.com/products/flash-media-encoder.html) チャンネルを AMS に単一ビットレートのストリームを送信する (FMLE) エンコーダーは、ライブ エンコーディングに対して有効なです。 詳細については、次を参照してください。 [Azure Media Services によるライブ エンコードの実行を有効になっているチャネルを操作](media-services-manage-live-encoder-enabled-channels.md)します。

このチュートリアルでは、Azure Media Service Explorer (AMSE) ツールを使用して、Azure Media Services (AMS) を管理する方法を示します。 このツールは、Windows PC でのみ実行されます。 Mac または Linux の場合は、Azure クラシック ポータルを使用して作成する [チャネル](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) と [プログラム](media-services-portal-creating-live-encoder-enabled-channel#create-and-manage-a-program)します。

このチュートリアルでは、AAC の使用方法について説明することに注意してください。 ただし、FMLE は、既定では AAC をサポートしません。 AAC のエンコードなど MainConcept から用のプラグインを購入する必要がある [AAC プラグイン。](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

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

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. チャネルの名前を指定します。説明フィールドは省略可能です。 [チャネルの設定] [ **標準** ライブ エンコードのオプションで、入力プロトコルに設定を **RTMP**します。 それ以外の設定はすべてそのままにしておくことができます。


必ず、 **新しいチャネルを今すぐ開始** が選択されています。

3. クリックして **チャネルの作成**します。
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] チャネルには開始するまで 20 分程度かかることができます。


チャネルの起動中にすることができます [エンコーダーを構成する](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp)です。

>[AZURE.IMPORTANT] チャネルが準備完了状態になるとすぐに課金が開始することに注意してください。 詳細については、次を参照してください。 [チャネルの状態の](media-services-manage-live-encoder-enabled-channels.md#states)です。

##<a id=configure_fmle_rtmp></a>FMLE エンコーダーを構成します。

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

1. 使用中のコンピューターで、Flash Media Live Encoder (FMLE) のインターフェイスに移動します。

    インターフェイスは、設定のメイン ページの 1 つです。 FMLE を使用するストリーミングを開始するには、次の推奨設定に注意してください。
    
    - 形式: H.264 フレーム レート: 30.00 
    - 入力サイズ: 1280 x 720 
    - ビットレート: 5000 Kbps (ネットワークの制限に基づいて調整可能)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    インターレースされたソースを使用する場合は、"インターレース解除" オプションをオフにしてください。

2. 形式の横にあるレンチのアイコンをクリックして、追加設定を次のようにする必要があります。

    - プロファイル: メイン
    - レベル: 4.0
    - キーフレームの頻度: 2 秒 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. 次の重要なオーディオの設定を行います。
    
    - 形式: AAC 
    - サンプル レート: 44100 Hz
    - ビットレート: 192 kbps
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Get、チャネルの FMLE に割り当てるために URL を入力 **RTMP エンドポイント**します。
    
    AMSE ツールに戻り、チャネルの完了状態を確認します。 状態が変わると **開始** に **を実行している**, 、入力の URL を取得することができます。
      
    チャネル名を右クリックして、経由でのホバー時に移動して、チャネルが実行されているときに **をクリップボードにコピー入力 URL** し、[ **主な入力
   URL**します。  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. この情報を貼り付け、 **FMS URL** ストリーム名を割り当て、出力セクションのフィールドです。 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    冗長性をさらに高めるには、セカンダリ入力 URL に対して次の手順を繰り返します。
8. 選択 **接続**します。

>[AZURE.IMPORTANT] クリックする前に **接続**, 、する **必要があります** チャネル準備ができていることを確認します。 
>また、15 分を超える入力投稿フィードがある場合を除き、チャネルを準備可能のままにしないでください。

##再生をテストする
  
1. AMSE ツールに移動し、テストするチャネルを右クリックします。 メニューから、ポインターを合わせる **再生プレビュー** 選択 **Azure Media Player で**します。  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

ストリームがプレーヤーに表示されている場合は、エンコーダーが AMS に接続するように正しく構成されています。 

エラーが表示される場合は、チャネルをリセットして、エンコーダー設定を調整する必要があります。 参照してください、 [トラブルシューティング](media-services-troubleshooting-live-streaming.md) ガイダンスのトピックです。  

##プログラムを作成する

1. チャネルの再生が確認されたら、プログラムを作成します。 下にある、 **Live** AMSE ツール] タブで、プログラムの領域内を右クリックし、選択 **新しいプログラムの作成**します。  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

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

