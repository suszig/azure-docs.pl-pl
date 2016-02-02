<properties 
   pageTitle="クラウド サービス ロールがリサイクルされる一般的な原因 | Microsoft Azure"
   description="クラウド サービス ロールが突然リサイクルされることで、重大なダウンタイムが生じることがあります。ロールのリサイクルを引き起こす一般的な問題とダウンタイムの防止策を取り上げました。"
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

デプロイに伴う問題の一般的な原因と問題解決に役立つトラブルシューティングのヒントを紹介します。 アプリケーションに存在する問題の兆候として、ロール インスタンスが起動に失敗したり、**初期化**、**ビジー**、**停止**という状態を繰り返したりすることが挙げられます。

## Azure カスタマー サポートへの問い合わせ

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。

または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。 Azure のサポートを使用する方法の詳細については、読み取り、 [Microsoft Azure サポートに関する FAQ](http://azure.microsoft.com/support/faq/)します。


## ランタイムの依存コンポーネントの不足

アプリケーションのロールに必要なアセンブリが .NET Framework や Azure マネージ ライブラリに含まれていない場合、そのアセンブリをアプリケーション パッケージに明示的にインクルードする必要があります。 それ以外の Microsoft フレームワークは、明示的に指定しない限り、Azure では利用できないのでご注意ください。 そのようなフレームワークにロールが依存している場合、対象となるアセンブリをアプリケーション パッケージに追加する必要があります。

アプリケーションをビルドしてパッケージ化する前に、次のことを確認します。

- Visual studio を使用している場合、Azure SDK や .NET Framework に含まれていない、プロジェクトのすべての参照アセンブリの **[ローカル コピー]** プロパティが **[True]** に設定されていること。

- **web.config** ファイルの **compilation** 要素が、未使用のアセンブリを参照していないこと。

- すべての .cshtml ファイルの **[ビルド アクション]** が **[コンテンツ]** に設定されていること。 この設定によって当該ファイルが正しくパッケージに追加され、他の参照ファイルをパッケージに追加することができます。



## アセンブリのターゲット プラットフォームの間違い

Azure は 64 ビット環境です。 そのため、32 ビット ターゲット用にコンパイルされた .NET アセンブリは Azure では動作しません。



## 初期化中または停止中ハンドルされない例外をロールがスローする

[OnStart] が含まれている [RoleEntryPoint] クラスのメソッドによってスローされる例外 [OnStop] と [実行] が未処理の例外です。 そのいずれかのメソッドでハンドルされない例外が発生した場合、ロールはリサイクルされます。 ロールが何度もリサイクルされる場合、起動の途中でハンドルされない例外がスローされている可能性があります。


## Run メソッドが終了している

[Run] メソッドは無期限に実行する対象としています。 コードでは、[Run] メソッドをオーバーライドする場合、無制限にスリープする必要があります。 [実行] のメソッドが返される場合は、ロールがリサイクルされます。




## DiagnosticsConnectionString の設定に誤りがある

アプリケーションが Azure 診断を使用する場合、サービス構成ファイルを指定する必要があります、 `DiagnosticsConnectionString` 構成設定。 この設定で、Azure にある該当ストレージ アカウントへの HTTPS 接続を指定します。

いることを確認する、 `DiagnosticsConnectionString` を Azure にアプリケーション パッケージを展開する前に、設定が正しいこと、次のことを確認します。

- `DiagnosticsConnectionString` ポイントを Azure で有効なストレージ アカウントに設定します。  
  既定では、エミュレートされたストレージ アカウントが指定されているため、アプリケーション パッケージをデプロイする前に、この設定を明示的に変更する必要があります。 この設定を変更しなかった場合、ロール インスタンスが診断モニターを起動しようとしたときに例外がスローされます。 ロール インスタンスが無限にリサイクルされる原因となる場合があります。

- 次の接続文字列が指定されている [形式](storage-configure-connection-string.md) (HTTPS とプロトコルを指定する必要があります)。 *MyAccountName* には該当するストレージ アカウントの名前を、*MyAccountKey* には該当するアクセス キーを指定してください。

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  For Microsoft Visual Studio の Azure Tools を使用して、アプリケーションを開発する場合は、使用、 [プロパティ ページ](https://msdn.microsoft.com/library/ee405486) この値を設定します。



## エクスポートした証明書に秘密キーが含まれていない

Web ロールを SSL で実行する場合は、エクスポートした管理証明書に秘密キーが含まれていることを確認してください。 *Windows 証明書マネージャー*を使用して証明書をエクスポートする場合は必ず、*[はい、秘密キーをエクスポートします]* オプションを選択してください。 証明書は PFX 形式でエクスポートする必要があります。現在サポートされている形式はこれだけです。



## 次のステップ

さらに表示 [トラブルシューティングに関する記事](..\?tag=top-support-issue&service=cloud-services) のクラウド サービスです。





[roleentrypoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx 
[onstart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx 
[onstop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx 
[run]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx 

