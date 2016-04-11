<properties 
   pageTitle="StorSimple Manager サービスをデプロイする | Microsoft Azure"
   description="Azure クラシック ポータルで StorSimple Manager サービスを作成および削除する方法、さらにサービス登録キーを管理する方法について説明します。"
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/01/2015"
   ms.author="v-sharos" />

# StorSimple Manager サービスをデプロイする

## 概要

StorSimple Manager サービスは Microsoft Azure で実行され、複数の StorSimple デバイスに接続します。 サービスを作成したら、それを使用して、ブラウザーで実行される Microsoft Azure クラシック ポータルでデバイスを管理できます。 このポータルを使用すると、StorSimple Manager サービスに接続されているすべてのデバイスを 1 か所から集中的に監視できるので、管理の負荷を最小限に抑えることができます。

StorSimple Manager のランディング ページには、StorSimple ストレージ デバイスの管理に使用できるすべての StorSimple Manager サービスの一覧が示されています。 各 StorSimple Manager について、以下の情報が [StorSimple Manager] ページに表示されます。

- **名前** – 作成時に、StorSimple Manager サービスに割り当てられた名前。 サービス名は、サービスの作成後に変更することはできません。

- **ステータス** – になる可能性がサービスの状態 **Active**, 、**作成**, 、または **オンライン**します。

- **場所** – StorSimple デバイスが展開される地理的な場所です。

- **サブスクリプション** – サービスに関連付けられている課金サブスクリプション。

[StorSimple Manager] ページで実行できる一般的なタスクは以下のとおりです。

- サービスの作成
- サービスの削除
- サービス登録キーを取得する
- サービス登録キーを再生成する

このチュートリアルでは、これらの各タスクの実行方法を説明します。

## サービスの作成

使用して、 **簡易作成** 、StorSimple デバイスを展開する場合は、StorSimple Manager サービスを作成するオプションです。 サービスを作成する上で必要なものは次のとおりです。

- エンタープライズ契約によるサブスクリプション
- アクティブな Microsoft Azure ストレージ アカウント
- アクセス管理で使用する課金情報

サービスの作成時に既定のストレージ アカウントを生成することもできます。

単一のサービスで複数のデバイスを管理できます。 ただし、1 つのデバイスを複数のサービスに割り当てることはできません。 大企業は、そのようなサービス インスタンスを複数持つことで、さまざまなサブスクリプション、組織、デプロイの場所に対応することができます。

サービスを作成するには、次の手順を実行します。

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## サービスの削除

サービスを削除する際は、接続されているデバイスでそのサービスが使用されていないことを先に確認します。 サービスが使用されている場合は、該当するデバイスを非アクティブにします。 非アクティブ化操作によってデバイスとサービス間の接続は切断されますが、クラウド内のデバイスのデータは保持されます。 

[AZURE.IMPORTANT] サービスを削除すると後、に、操作を元に戻すことはできません。 このサービスを利用していたデバイスを別のサービスで使用するには、デバイスを出荷時の状態にリセットする必要があります。 このとき、デバイス上のローカル データや構成は失われます。

サービスを削除するには次の手順を実行します。

### サービスを削除するには

1.  **StorSimple Manager サービス** ] ページで、削除するサービスを選択します。

1. クリックして **削除** ページの下部にあります。

1. クリックして **はい** 通知の確認。 サービスが削除されるまで数分かかる場合があります。

## サービス登録キーを取得する

正常にサービスを作成した後、そのサービスに StorSimple デバイスを登録します。 初めて StorSimple デバイスを登録する場合は、サービス登録キーが必要です。 既存の StorSimple サービスに追加のデバイスを登録するには、登録キーとサービス データ暗号化キー (最初のデバイスの登録中に生成される) の両方が必要です。 サービス データ暗号化キーの詳細については、次を参照してください。 [StorSimple のセキュリティ](storsimple-security.md)します。 登録キーを取得するにアクセスして **登録キー** 上、 **サービス** ページです。

次の手順を実行して、サービス登録キーを取得します。

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

サービス登録キーは、安全な場所に保管してください。 このキーは、サービスに追加のデバイスを登録するときにサービス データ暗号化キーと共に必要です。 サービス登録キーを取得したら、StorSimple 用 Windows PowerShell インターフェイスを介してデバイスを構成する必要があります。

登録キーの使用方法の詳細については、次を参照してください。 [手順 3: 構成し、StorSimple 用 Windows PowerShell を使用してデバイスを登録](storsimple-deployment-walkthrough.md#step-2-configure-and-register-the-device-through-windows-powershell-for-storsimple)します。

## サービス登録キーを再生成する

キー ローテーションを実行する必要があるとき、またはサービス管理者の一覧が変更されたときは、サービス登録キーを再生成する必要があります。 キーを再生成した場合、新しいキーはその後のデバイスの登録に対してのみ使用されます。 既に登録されているデバイスは、再生成の影響を受けません

次の手順を実行して、サービス登録キーを再生成します。

### サービス登録キーを再生成するには

1.  **StorSimple Manager サービス** ] ページで [ **登録キー**します。

1.  **サービス登録キー** ダイアログ ボックスで、をクリックして **を再生成**します。

1. 確認メッセージが表示されます。 クリックして **OK** 、再生成を続行します。

1. 新しいサービス登録キーが表示されます。

1. このキーをコピーし、このサービスに新しいデバイスを登録するときのために保存します。

1. チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-manage-service/HCS_CheckIcon.png) このダイアログ ボックスを閉じます。


## 次のステップ

- 詳細について、 [StorSimple 展開プロセス](storsimple-deployment-walkthrough.md)します。

- 詳細について [StorSimple ストレージ アカウントを管理する](storsimple-manage-storage-accounts.md)です。

- 方法の詳細について [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)です。

 
