
<properties
    pageTitle="Azure RemoteApp イメージの要件 | Microsoft Azure"
    description="Azure RemoteApp で使用するイメージを作成するための要件について説明します。"
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
    ms.date="12/05/2015"
    ms.author="elizapo" />



# Azure RemoteApp イメージの要件
Azure RemoteApp では Windows Server 2012 R2 イメージを使用して、ユーザーと共有するすべてのプログラムをホストします。 カスタム イメージを作成するには、既存のイメージで開始できますまたは [、新しく作成](remoteapp-create-custom-image.md)します。

> [AZURE.TIP] ある Azure RemoteApp サブスクリプションにアクセスできる、独自のテンプレート イメージの作成に使用できる、Azure VM ギャラリーでの Windows Server 2012 R2 イメージをご存知でしたか。 [チェック アウト](remoteapp-image-on-azurevm.md)します。  


Azure RemoteApp で使用するためにアップロードできるイメージの要件は次のとおりです。


- カスタム アプリケーションは、イメージ上にローカルにデータを格納しません。 これらのイメージはステートレスで、アプリケーションのみを含む必要があります。
- イメージには、失われる可能性があるデータは含まれません。
- イメージのサイズは MB の倍数にする必要があります。 正確な倍数ではないイメージをアップロードしようとすると、アップロードは失敗します。
- イメージのサイズは 127 GB 未満でなければなりません。
- VHD ファイルになければなりません (VHDX ファイルは現在サポートされていません)。
- VHD を第 2 世代仮想マシンにしないでください。
- VHD は固定サイズにすることも、動的に拡大する容量可変にすることも可能です。 固定サイズの VHD より Azure へのアップロードの所要時間が短いことから、容量可変の VHD が推奨されます。
- ディスクはマスター ブート レコード (MBR) パーティション分割のスタイルを使用して初期化しなければなりません。 GUID パーティション テーブル (GPT) パーティション分割のスタイルはサポートされていません。
- VHD には Windows Server 2012 R2 の単一インストールが含まれていなければなりません。 複数のボリュームを含むことはできますが、Windows がインストールされるのは 1 ボリュームのみです。
- リモート デスクトップ セッション ホスト (RDSH) ロールとデスクトップ エクスペリエンスの機能がインストール済みでなければなりません。
- リモート デスクトップ接続ブローカーの役割 *いない* をインストールします。
- 暗号化ファイル システム (EFS) は無効にする必要があります。
- イメージはパラメーターを使用して/mode:vm である必要があります **/oobe/generalize/shutdown** (使用しないで、 **/mode:vm** パラメーター)。
- スナップショット チェーンからの VHD のアップロードはサポートされていません。

参照してください [Azure RemoteApp イメージの作成](remoteapp-imageoptions.md) 詳細については、Azure RemoteApp のイメージを作成します。

