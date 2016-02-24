<properties 
   pageTitle="クラウド サービス ロールがリサイクルされる一般的な原因 | Microsoft Azure"
   description="クラウド サービス ロールが突然リサイクルされることで、重大なダウンタイムが生じることがあります。 ロールのリサイクルを引き起こす一般的な問題とダウンタイムの防止策を取り上げました。"
   services="cloud-services"
   documentationCenter=""
   authors="dalechen"
   manager="msmets"
   editor=""
   tags="top-support-issue"/>
<tags 
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/14/2015"
   ms.author="daleche" />

# ロールのリサイクルを引き起こす一般的な問題

デプロイに伴う問題の一般的な原因と問題解決に役立つトラブルシューティングのヒントを紹介します。 アプリケーションに問題があることを示す値は、ロール インスタンスが起動に失敗し、交互に切り替えること、 **初期化**, 、**ビジー**, と **停止** 状態です。

## Azure カスタマー サポートへの問い合わせ

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。

または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。 Azure のサポートを使用する方法の詳細については、読み取り、 [Microsoft Azure サポートに関する FAQ](http://azure.microsoft.com/support/faq/)します。


## ランタイムの依存コンポーネントの不足

アプリケーションのロールに必要なアセンブリが .NET Framework や Azure マネージ ライブラリに含まれていない場合、そのアセンブリをアプリケーション パッケージに明示的にインクルードする必要があります。 それ以外の Microsoft フレームワークは、明示的に指定しない限り、Azure では利用できないのでご注意ください。 そのようなフレームワークにロールが依存している場合、対象となるアセンブリをアプリケーション パッケージに追加する必要があります。

アプリケーションをビルドしてパッケージ化する前に、次のことを確認します。

- Visual studio を使用して、確認、 **ローカル コピー** にプロパティが設定されている **True** Azure SDK または .NET Framework の一部ではない、プロジェクト内の各参照アセンブリのです。

- 必ず、 **web.config** ファイル内の未使用のアセンブリを参照しない、 **コンパイル** 要素。
 
-  **ビルド アクション** に設定されているファイルの各 .cshtml **コンテンツ**します。 この設定によって当該ファイルが正しくパッケージに追加され、他の参照ファイルをパッケージに追加することができます。



## アセンブリのターゲット プラットフォームの間違い

Azure は 64 ビット環境です。 そのため、32 ビット ターゲット用にコンパイルされた .NET アセンブリは Azure では動作しません。



## 初期化中または停止中ハンドルされない例外をロールがスローする

メソッドによってスローされる例外、 [RoleEntryPoint] を含むクラス、 [OnStart], 、[OnStop], 、および [Run], 、未処理の例外です。 そのいずれかのメソッドでハンドルされない例外が発生した場合、ロールはリサイクルされます。 ロールが何度もリサイクルされる場合、起動の途中でハンドルされない例外がスローされている可能性があります。


## Run メソッドが終了している

 [Run] 無期限に実行するメソッドが対象としています。 コードをオーバーライドする場合、 [Run] 、メソッド、スリープ無期限にします。 場合、 [Run] メソッドが戻る、ロールがリサイクルされます。




## DiagnosticsConnectionString の設定に誤りがある

アプリケーションで Azure 診断を使用している場合、サービス構成ファイルで `DiagnosticsConnectionString` 構成設定を指定する必要があります。 この設定で、Azure にある該当ストレージ アカウントへの HTTPS 接続を指定します。

アプリケーション パッケージを Azure にデプロイする前に `DiagnosticsConnectionString` が正しく設定されていることを確かめるために、次の点を確認してください。  

- Azure 内の有効なストレージ アカウントが `DiagnosticsConnectionString` の設定に指定されていること。  
  既定では、エミュレートされたストレージ アカウントが指定されているため、アプリケーション パッケージをデプロイする前に、この設定を明示的に変更する必要があります。 この設定を変更しなかった場合、ロール インスタンスが診断モニターを起動しようとしたときに例外がスローされます。 ロール インスタンスが無限にリサイクルされる原因となる場合があります。
  
- 次の接続文字列が指定されている [形式](storage-configure-connection-string.md) (HTTPS とプロトコルを指定する必要があります)。 置換 *MyAccountName* 、ストレージ アカウントの名前と *MyAccountKey* をアクセス キー。    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  For Microsoft Visual Studio の Azure Tools を使用して、アプリケーションを開発する場合は、使用、 [プロパティ ページ](https://msdn.microsoft.com/library/ee405486) この値を設定します。 



## エクスポートした証明書に秘密キーが含まれていない

Web ロールを SSL で実行する場合は、エクスポートした管理証明書に秘密キーが含まれていることを確認してください。 使用する場合、 *Windows 証明書マネージャー* 、証明書をエクスポートするには、必ず選択して、 *はい*, 、秘密キーのオプションをエクスポートします。 証明書は PFX 形式でエクスポートする必要があります。現在サポートされている形式はこれだけです。



## 次のステップ

さらに表示 [トラブルシューティングに関する記事](..\?tag=top-support-issue&service=cloud-services) のクラウド サービスです。




[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Run]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
