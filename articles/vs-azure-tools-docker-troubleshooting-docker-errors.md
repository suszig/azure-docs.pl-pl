<properties
   pageTitle="Visual Studio を使用した Windows での Docker クライアント エラーのトラブルシューティング | Microsoft Azure"
   description="Visual Studio を使用して、Windows 上で Web アプリを作成し、Docker にデプロイする際に発生する問題のトラブルシューティングを行います。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/20/2015"
   ms.author="tarcher" />

# Docker エラーのトラブルシューティング

アプリケーションの Docker コンテナーのすべての設定を構成したら、設定とパスが正しいことを確認する必要があります。 Visual Studio では、[発行] ダイアログに、この確認に役立つ [検証] ボタンがあります。

このトピックは、Docker で Visual Studio アプリケーションをホストする場合に発生する最も一般的な問題を診断し、修正または回避するために役立ちます。 問題が検出されると、このトピックに追加されます。

## [Web の発行] ダイアログ ボックスで Docker ホストへの接続を検証しようとすると失敗メッセージが表示される

以下に、この問題のいくつかの解決策を示します。

-  **接続** のタブをクリック、 **発行** ダイアログ ボックスで、ことを確認、 **サーバーの Url** が正しいと末尾 `:<port_number>` に、 **サーバーの URL** Docker デーモンがリッスンするポートです。

-  **接続** のタブ、 **発行** ダイアログ ボックスで、展開、 **Docker の詳細オプション** セクションし、正しいことを確認する **Auth** オプションが指定されています。
  - サーバー上の Docker デーモンが TLS セキュリティを使用するように構成されている場合、Windows Docker コマンド ライン インターフェイス (docker.exe) は既定で `<%userprofile%>\.docker` フォルダーの下にあるクライアント キー (key.pem) と証明書 (cert.pem) を検索します。 これらの項目が存在しない場合、OpenSSL を使用して生成する必要があります。 TLS の Docker を構成する方法については、次を参照してください。 [Docker デーモン ソケットの HTTPS で保護](https://docs.docker.com/articles/https/)します。

    Docker で Windows クライアントから Linux サーバーへの認証が適切に行われていることを確認する 1 つの方法は、[プレビュー] ボックスの内容を新しいコマンド ウィンドウにコピーし、次に示すように `<command>` を "info" に変更することです。

    ```
    // This example assumes the Docker daemon is configured to use the default port
    // of 2376 to listen for connections.docker.

    --tls -H tcp://contoso.cloudapp.net:2376 info
    ```

    クライアントの証明書とキー ファイルを .docker フォルダーにコピーする代わりに、変更することができます、 **Auth** 、次のパラメーターを追加することによってオプション。

    ```
    --tls --tlscert=C:\mycert\cert.pem --tlskey=C:\mycert\key.pem
    ```
- Docker ホスト マシン上の Docker デーモンがバージョン 1.6 以降であることを確認します。

## Docker フォルダー内にクライアント証明書がなく独自の証明書を使用している場合のタイムアウト エラー

Visual Studio で Docker ホストを作成するときに、独自の証明書を使用する場合 (つまりをオフにする、 **Docker の自動生成証明書** ] チェック ボックス、 **Microsoft Azure で仮想マシンを作成** ] ダイアログ ボックス)、(あると key.pem) クライアント証明書とキー ファイルを Docker フォルダーにコピーする必要があります (`<%userprofile%>\.docker`)。 そうしないと、プロジェクトの発行時に、1 時間でタイムアウト エラーが発生し、発行操作が失敗します。

## Docker コンテナーへの発行には PowerShell 3.0 が必要である

オペレーティング システムが Windows 7 または Windows Server 2008 の場合は、Docker コンテナーに発行する前に、PowerShell 3.0 をインストールする必要があります。 PowerShell 3.0 が含まれます [Windows Management Framework 3.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595)します。 インストール後にシステムを再起動する必要があります。

別の回避策として、PowerShell 3.0 が既に組み込まれている Windows 8.1 または Windows 10 にアップグレードすることができます。

## PowerShell ウィンドウは自動的に閉じない

VM の作成後、PowerShell ウィンドウが自動的に閉じないことがあります。 このウィンドウを閉じると、Visual Studio も閉じられます。 ウィンドウは、Visual Studio や Docker ツールの機能に影響しないため、作業を完了するまで、開いたままにしておいてください。

## FAQ

Q: Visual Studio ツールを使用して、Azure で新しい Docker 対応の Linux マシンを作成するにはどうすればよいですか?

A:「 [Docker で Web アプリをホストしている](vs-azure-tools-docker-hosting-web-apps-in-docker.md) これを行う方法の詳細。

Q: どのような Visual Studio プロジェクト テンプレートは、Linux Docker コンテナーへの発行にサポートされますか。

A: visual Studio には、c# コンソール アプリケーション (パッケージ) およびなどの c# ASP.NET 5 Preview web テンプレート現在サポートされています。

- Empty

- Web API

- Web Application

コマンドラインから MSBUILD を使用して、Docker に ASP.NET 5 web またはコンソール プロジェクトを公開 q: しますか。

A: は、次の MSBuild コマンドを使用します。

    `msbuild <projectname.xproj> /p:deployOnBuild=true;publishProfile=<profilename>`

コマンドラインから PowerShell を使用して、Docker に ASP.NET 5 web またはコンソール プロジェクトを公開 q: しますか。

A: は、次の PowerShell コマンドを使用します。

```
.\contoso-Docker-publish.ps1 -packOutput $env:USERPROFILE\AppData\Local\Temp\PublishTemp -pubxmlFile .\contoso-Docker.pubxml
```

Q: 自分の Linux サーバー Docker がインストールされた、この指定方法、 **Web の発行** ダイアログですか?

A: を参照してください **カスタム Docker ホストが提供され** 」トピックの「 [Docker で Web アプリをホストしている](vs-azure-tools-docker-hosting-web-apps-in-docker.md)します。

Q: Docker がインストールされた独自の Linux サーバーを使用しています。 TLS を使用して認証を構成するためにキーと証明書を生成するにはどうすればよいですか?

A: 1 つの方法では、CA、サーバー、およびクライアントに必要な証明書とキーを生成するサーバーで OpenSSL を使用します。 そうすれば、サード パーティのソフトウェアを使用して SSH/SFTP 接続を確立してから、証明書をローカルの Windows 開発用コンピューターにコピーできます。 既定では、Docker (CLI) は `<userprofile>\.docker` フォルダーにある証明書の使用を試みます。

別のオプションは、Windows 用の OpenSSL をダウンロードして、必要な証明書とキーを生成してから、CA、サーバーの証明書、およびキーを Linux コンピューターにアップロードすることです。 Docker へのセキュリティで保護された接続を確立する方法の詳細については、次を参照してください。 [Docker デーモン ソケットの HTTPS で保護](https://docs.docker.com/articles/https/)します。

