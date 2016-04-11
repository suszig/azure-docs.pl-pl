<properties 
    pageTitle="Azure SDK for Java のダウンロード" 
    description="Azure SDK for Java をダウンロードする方法を Maven プロジェクト用に用意されたサンプル コードと共に紹介し、また、Azure Tookit for Eclipse の基本的なインストール手順を説明します。" 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="multiple" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="robmcm"/>

# Azure SDK for Java のダウンロード #

この記事には、Azure Libraries for Java をダウンロードしてインストールための手順が記載されています。

**注:** Azure Libraries for Java はの下で配布、 [Apache License, バージョン 2.0][license]します。

## Azure Libraries for Java - 手動ダウンロード ##

Azure Libraries for Java を手動でインストールするには、次のようにクリックします。 <http://go.microsoft.com/fwlink/?LinkId=690320> すべてのライブラリおよびすべての依存関係を含む ZIP ファイルをダウンロードします。

zip ファイルをコンピューターにダウンロードしたら、コンテンツを抽出し、次のいずれかのオプションを使用して、JAR ファイルをプロジェクトに追加します。

* JAR ファイルを Eclipse の Java プロジェクトにインポートします。

* 構成、 **Build Path** JAR ファイルへのパスを Eclipse で Java プロジェクト用。

Eclipse でのビルド パスの設定の詳細については、次を参照してください。、 [Java Build Path][] Eclipse web サイトの記事です。

**注:** license.txt と ThirdPartyNotices.txt ファイル、ZIP 内のライセンスおよびその他の情報を参照してください。

## Azure Libraries for Java - Maven によるビルド ##

### 手順 1 - ビルドに Maven を使用するようにプロジェクトを設定する ###

Eclipse」の説明に従って、Java 用 Azure ライブラリを使用する Maven プロジェクトを作成する、 [Azure Management Libraries for Java の概要][maven-getting-started] 記事です。 

### 手順 2 - 必要な依存関係を持つ Maven 設定を構成する ###

ビルドに Maven を使用するようにプロジェクトを構成したら、必要な依存関係を、次の例のような構文を使用する pom.xml ファイルに追加できます。 次の例に表示されたすべての依存関係を追加する必要はありません。追加する必要があるのは、プロジェクトに必要な特定の依存関係のみです。

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-compute</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-network</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-sql</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-storage</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-websites</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-media</artifactId>
        <version>0.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>0.6.0</version>
    </dependency>

**注:** 各内にある `<version>` 要素を前の例では、有効なバージョン番号から入手できますこの例ではバージョン番号を置き換えて、 [Maven の Azure ライブラリ リポジトリ][]します。

## Azure Toolkit for Eclipse のインストール ##

このセクションには、Azure Toolkit for Eclipse; をインストールするための基本的な手順が含まれています。詳細については、次を参照してください。 [Azure Toolkit for Eclipse をインストールする][]です。

### 前提条件 ###

1. 示されている Windows オペレーティング システム、 [Azure Toolkit for Eclipse の新][] 記事です。
1. Macintosh または Linux オペレーティング システムに、 [Azure Toolkit for Eclipse の新][] 記事です。
1. Eclipse IDE for Java EE Developers Indigo 以降。 これはからダウンロードできる <http://www.eclipse.org/downloads/>します。

### 基本的なインストール手順 ###

1. Eclipse から、 **ヘルプ** メニューの [ **Install New Software**します。
1. サイトの場所を入力して <http://dl.msopentech.com/eclipse> とキーを押します **Enter**します。
1. インストールして、をクリックする項目の選択 **完了**します。

Azure Toolkit for Eclipse では、最新バージョンの Azure SDK を使用します。 Web Platform Installer (WebPI) を使用してダウンロードできます <http://go.microsoft.com/fwlink/?LinkID=252838>します。 ただし、まだインストールしないうちに、最初の Azure デプロイ プロジェクトを作成すると、Azure Toolkit for Eclipse によって適切なバージョンの Azure SDK が自動的にインストールされます。

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure Toolkit for Eclipse のインストール][] 

[Azure 向け Hello World アプリケーションを Eclipse で作成する][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Libraries Repository on Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java Build Path]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333

