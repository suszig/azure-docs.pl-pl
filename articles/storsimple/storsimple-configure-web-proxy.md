<properties 
   pageTitle="StorSimple デバイスの Web プロキシを設定する | Microsoft Azure"
   description="StorSimple 用 Windows PowerShell を使用して StorSimple デバイス用に Web プロキシを構成する方法について説明します。"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/02/2015"
   ms.author="alkohli" />

#  StorSimple デバイスの Web プロキシを構成する

## 概要

このチュートリアルでは、StorSimple 用 Windows PowerShell を使用して StorSimple デバイスの Web プロキシ設定を構成および表示する方法について説明します。 Web プロキシ設定は、クラウドと通信するときに StorSimple デバイスで使用されます。 Web プロキシ サーバーは、別のセキュリティ レイヤーの追加、コンテンツのフィルター処理、キャッシュによる帯域幅要件の緩和、分析の補助に使用されます。

Web プロキシは StorSimple デバイスのオプションの構成です。 Web プロキシの構成には StorSimple 用 Windows PowerShell のみ使用できます。 構成には、次の 2 つの手順を実行します。

1. まず、セットアップ ウィザードまたは StorSimple 用 Windows PowerShell のコマンドレットを使用して、Web プロキシ設定を構成します。

2. 次に、StorSimple 用 Windows PowerShell のコマンドレットを使用して、構成した Web プロキシ設定を有効にします。

Web プロキシの構成が完了したら、Microsoft Azure StorSimple Manager サービスと StorSimple 用 Windows PowerShell の両方で、構成済みの Web プロキシ設定を表示できます。 

このチュートリアルを読むと、次のことができるようになります。

- セットアップ ウィザードとコマンドレットを使用して Web プロキシを構成する
- コマンドレットを使用して Web プロキシを有効にする
- Azure クラシック ポータルで Web プロキシ設定を表示する
- Web プロキシの構成中のエラーをトラブルシューティングする


## StorSimple 用 Windows PowerShell を使用して Web プロキシを構成する

Web プロキシ設定を構成するには、次のいずれかを使用します。

- 構成手順を案内するセットアップ ウィザード。

- StorSimple 用 Windows PowerShell のコマンドレット。

これらの各方法については、以下のセクションで説明します。

## セットアップ ウィザードを使用して Web プロキシを構成する

セットアップ ウィザードを使用し、案内に従って Web プロキシの構成手順を実行できます。 次の手順を実行して、デバイスで Web プロキシを構成します。

#### セットアップ ウィザードを使用して Web プロキシを構成するには

1. シリアル コンソール メニューでは、オプション 1 を選択して **フル アクセスでログイン** を提供し、 **デバイス管理者のパスワード**します。 次のコマンドを入力してセットアップ ウィザード セッションを開始します。

    `Invoke-HcsSetupWizard`

2. デバイスの登録に初めてセットアップ ウィザードを使用する場合は、Web プロキシ構成の前に表示される、必要なすべてのネットワーク設定を構成する必要があります。 デバイスが既に登録されている場合は、web プロキシ構成が表示されるまで、すべての構成済みのネットワーク設定を受理できます。セットアップ ウィザードで web プロキシ設定を構成するメッセージが表示されたら、入力 **はい**します。

3.  **Web プロキシ URL**, 、web プロキシ サーバーの IP アドレスまたは完全修飾ドメイン名 (FQDN) を指定し、TCP ポート番号をクラウドと通信するときに使用するデバイスを希望します。 次の形式を使用します。

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    既定では、TCP ポート番号 8080 が指定されています。

4. 認証の種類として選択 **NTLM**, 、**基本的な**, 、または **None**します。 [Basic] は、プロキシ サーバー構成で最も安全性が低い認証です。 [NTLM] (NT LAN Manager) は、ユーザーを認証するために 3 段階のメッセージング システム (さらに整合性が要求される場合は 4 段階の場合もあります) を使用する最も安全性が高く複雑な認証プロトコルです。 既定の認証は NTLM です。 詳細については、次を参照してください。 [基本的な](http://hc.apache.org/httpclient-3.x/authentication.html) と [NTLM 認証](http://hc.apache.org/httpclient-3.x/authentication.html)します。 

    > [AZURE.IMPORTANT] **、StorSimple Manager サービス デバイス監視チャートと動作しない基本的なまたはデバイスのプロキシ サーバー構成で NTLM 認証を有効にします。監視グラフを機能させるには、認証が [None] に設定されていることを確認する必要があります。**

5. 認証を使用している場合は、指定、 **Web プロキシ ユーザー名** と **Web プロキシ パスワード**します。 また、パスワードの確認も必要です。

    ![Configure Web Proxy On StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

初めてデバイスを登録する場合は、登録に進みます。 デバイスが既に登録されている場合、ウィザードは終了します。 構成した設定は保存されます。

また、Web プロキシも有効になります。 スキップすることができます、 [web プロキシを有効にする](#enable-web-proxy) 手順およびに直接移動 [Azure クラシック ポータルで web プロキシ設定を表示](#view-web-proxy-settings-in-the-azure-classic-portal)します。


## StorSimple 用 Windows PowerShell のコマンドレットを使用して Web プロキシを構成する

Web プロキシ設定を構成するための代替手段として、StorSimple 用 Windows PowerShell のコマンドレットを使用する方法があります。 次の手順を実行して、Web プロキシを構成します。

#### コマンドレットを使用して Web プロキシを構成するには

1. シリアル コンソール メニューでは、オプション 1 を選択して **フル アクセスでログイン**します。 入力を求められたら、 **デバイス管理者のパスワード**します。 既定のパスワードは  `Password1`です。

2. コマンド プロンプトに、次のコマンドを入力します。

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    入力を求めるメッセージが次のように表示されたら、パスワードを入力して確認します。

    ![Configure Web Proxy On StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

これで、Web プロキシが構成されたので、有効にする必要があります。

## Web プロキシを有効にする

Web プロキシは、既定では無効になっています。 StorSimple デバイスで Web プロキシ設定を構成したら、StorSimple 用 Windows PowerShell を使用して Web プロキシ設定を有効にする必要があります。

> [AZURE.NOTE] **セットアップ ウィザードを使用して web プロキシを構成する場合、この手順は必要なできません。セットアップ ウィザードのセッションの後、Web プロキシは既定で自動的に有効になります。**

StorSimple 用 Windows PowerShell で次の手順を実行し、デバイスで Web プロキシを有効にします。

#### Web プロキシを有効にするには

1. シリアル コンソール メニューでは、オプション 1 を選択して **フル アクセスでログイン**します。 入力を求められたら、 **デバイス管理者のパスワード**します。 既定のパスワードは  `Password1`です。

2. コマンド プロンプトに、次のコマンドを入力します。

    `Enable-HcsWebProxy`

    これで、StorSimple デバイスの Web プロキシ構成が有効になりました。

    ![Configure Web Proxy On StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## Azure クラシック ポータルで Web プロキシ設定を表示する

Web プロキシ設定は、Windows PowerShell インターフェイスを使用して構成します。クラシック ポータル内から変更することはできません。 ただし、構成済みの設定をクラシック ポータルで表示することはできます。 次の手順を実行して、Web プロキシを表示します。

#### Web プロキシ設定を表示するには
1. 移動 **StorSimple Manager サービス > デバイス**します。 選択し、[デバイス] をクリックしに **構成**します。
1. 下へスクロール、 **構成** ページ **Web プロキシ設定** セクションです。 次に示すように、StorSimple デバイスで構成した Web プロキシ設定を表示できます。

    ![管理ポータルで Web プロキシを表示する](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## Web プロキシ構成中のエラー

Web プロキシの設定が正しく構成されていない場合は、StorSimple 用 Windows PowerShell でユーザーにエラー メッセージが表示されます。 次の表では、このようなエラー メッセージ、考えられる原因、および推奨される操作について説明します。

|シリアル番号|HRESULT エラー コード|考えられる根本原因|推奨される操作|
|:---|:---|:---|:---|
|1.|0x80070001|コマンドがパッシブ コントローラーから実行されていて、アクティブ コントローラーと通信することができません。|アクティブ コントローラーでコマンドを実行します。 パッシブ コントローラーからコマンドを実行するには、接続をパッシブ コントローラーからアクティブ コントローラーに修正する必要があります。 この接続を確立できない場合、Microsoft サポートに問い合わせる必要があります。|
|2.|0x800710dd - 操作識別子が無効です|StorSimple 仮想デバイスではプロキシ設定がサポートされていません。|StorSimple 仮想デバイスではプロキシ設定がサポートされていません。 これらの設定は、StorSimple の物理デバイスでのみ構成できます。|
|3.|0x80070057 - 無効なパラメーター|プロキシ設定に指定されたパラメーターの 1 つが有効ではありません。|URI が正しい形式で指定されていません。 形式は `http://<IP address or FQDN of the web proxy server>:<TCP port number>` を使用してください。|
|4.|0x800706ba - RPC サーバーを利用できません|根本原因は、次のいずれか。</br></br>クラスターは、アップではありません。</br></br>データパス サービスが実行されていません。</br></br>パッシブ コント ローラーからコマンドを実行して、アクティブ コント ローラーと通信することはできません。|マイクロソフト サポート、クラスターが稼働し、データパス サービスが実行されていることを確認すると連携してください。</br></br>アクティブなコント ローラーからコマンドを実行します。 パッシブ コントローラーからコマンドを実行する場合、パッシブ コントローラーがアクティブ コントローラーと通信できることが必要となります。 この接続を確立できない場合、Microsoft サポートに問い合わせる必要があります。|
|5.|0x800706be - RPC 呼び出しに失敗しました|クラスターはダウンしています。|Microsoft サポートに問い合わせて、クラスターが稼働しているかどうかを確認してください。|
|6.|0x8007138f - クラスター リソースが見つかりません|プラットフォーム サービス クラスター リソースが見つかりません。 これはインストールが適切でなかった場合に発生することがあります。|デバイスで出荷時の設定にリセットすることが必要になる場合があります。プラットフォーム リソースの作成が必要になる場合があります。 Microsoft サポートに対処法をお問い合わせください。|
|7.|0x8007138c - クラスター リソースがオンラインではありません|プラットフォームまたはデータパス クラスター リソースがオンラインではありません。|Microsoft サポートに連絡し、データパスとプラットフォーム サービス リソースがオンラインになるようにしてください。|

> [AZURE.NOTE] 
> 
> -  上記のエラー メッセージの一覧ですべてを網羅しているわけではありません。 
> - Web プロキシの設定に関連するエラーは、StorSimple Manager サービスの Azure クラシック ポータルには表示されません。 構成が完了した後で web プロキシに問題がある場合は、デバイスの状態が変わります **オフライン** クラシック ポータルで |。

## 次のステップ

- デバイスのデプロイまたは web プロキシ設定の構成中に問題が発生した場合を参照してください [StorSimple デバイスのデプロイのトラブルシューティングを行う](storsimple-troubleshoot-deployment.md)します。

- StorSimple Manager サービスを使用する方法を学習するには [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)です。

