<properties 
   pageTitle="Visual Studio を使用した Azure プロジェクトの作成"
   description="Visual Studio を使用した Azure プロジェクトの作成"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="tarcher" />

# Visual Studio を使用した Azure プロジェクトの作成

Azure Tools for Visual Studio には、Azure でクラウド サービスを作成するためのテンプレートが用意されています。 このツールは、クラウド サービスを構成、デバッグし、Azure にデプロイするときにも役立ちます。

Azure クラウド サービスのソリューションには、以下の種類のプロジェクトが含まれています。

- **Azure プロジェクト** 
    
    Azure プロジェクトは、ソリューション内のロール プロジェクトに関連付けられています。 また、サービス定義ファイルとサービス構成ファイルが含まれます。 サービス定義ファイルは、必要なロール、エンドポイント、仮想マシンのサイズを含む、アプリケーションのランタイム設定を定義します。 サービス構成ファイルでは、実行されるロールのインスタンス数とロールに定義されている設定の値を構成します。 これらの設定の詳細については、次を参照してください。 [方法: Visual Studio で Azure クラウド サービスの役割を構成する](https://msdn.microsoft.com/library/azure/hh369931.aspx)です。

- **Web ロール プロジェクト**
 
    worker ロールは、バック グラウンド処理を実行します。 worker ロールは、ストレージ サービスや他のインターネット ベースのサービスと通信できます。 worker ロールは、任意の数の HTTP、HTTPS、TCP エンドポイントを含むことができます。

    - **ASP.NET Web ロール**, 、web フロント エンドを持つ ASP.NET アプリケーションを構築するため
    - **ASP.NET MVC5 Web ロール**
    - **ASP.NET MVC4 Web ロール**
    - **ASP.NET MVC3 Web ロール**
    - **WCF サービス Web ロール**, 、WCF サービスを構築するため
    - **Silverlight ビジネス アプリケーション Web ロール** (Visual Studio 2012 が必要)

- **キャッシュ Worker ロール** 

    アプリケーションに専用のキャッシュを提供するロール。

- **Service Bus キューを使用する Worker ロール** 

    Service Bus キューは、worker プロセスと通信するためのメッセージのキュー機能を提供します。 詳細については、次を参照してください。 [サービス バス キューの使用方法](http://go.microsoft.com/fwlink/?LinkId=260560)します。

## Visual Studio で Azure クラウド サービス プロジェクトを作成するには

1. Microsoft Visual Studio を管理者として起動します。

1. メニュー バー **ファイル**, 、**新規**, 、**プロジェクト**します。

1.  **プロジェクトの種類** ] ウィンドウで、選択 **クラウド** から Visual c# または Visual Basic プロジェクト テンプレート ノードです。

1.  **テンプレート** ] ウィンドウで、選択  **Azure クラウド サービス**します。

1. プロジェクトの開発に使用する .NET Framework のバージョンを指定します。

1. プロジェクトの名前と場所、およびソリューションの名前を入力します。 選択、 **OK** ] ボタンをクリックします。

1.  **新しい Azure プロジェクト** ] ダイアログ ボックスで、追加するロールを選択し、ソリューションに追加する右矢印ボタンをクリックします。 ロールは、必要なだけ追加することができます。

1. プロジェクトに追加されているロールの名前を変更するには、ロールでポイント、 **新しい Azure プロジェクト** ] ダイアログ ボックス、および選択、 **の名前を変更** ロールの右側にあるアイコン。 追加後も、ソリューション内でロールの名前を変更できます。



