<properties 
    pageTitle="Jenkins 継続的インテグレーション ソリューションでの Azure Storage の使用 | Microsoft Azure" 
    description="このチュートリアルでは、Jenkins 継続的インテグレーション ソリューションで作成されるビルド アーティファクトのリポジトリとして Azure BLOB サービスを使用する方法について説明します。" 
    services="storage" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="robmcm"/>


# Jenkins 継続的インテグレーション ソリューションでの Azure Storage の使用

## 概要

ここでは、Jenkins 継続的インテグレーション (CI) ソリューションで作成されるビルド アーティファクトのリポジトリとして、または、ビルド プロセスで使用されるダウンロード可能なファイルのソースとして Azure BLOB サービスを使用する方法について説明します。 この方法が有用になるシナリオの 1 つが、アジャイル開発環境で (Java などの言語を使って) コーディングをしており、継続的インテグレーションに基づいてビルドを実行するとき、ビルド アーティファクト用のリポジトリが必要な場合です。このリポジトリがあれば、ビルド アーティファクトを他の組織のメンバーや顧客と共有したり、そのアーカイブを保存したりできます。 もう 1 つのシナリオとしては、ビルド ジョブ自体にその他のファイルが必要になる場合、たとえば、ビルドの入力で依存関係のダウンロードが必要になる場合などが考えられます。

このチュートリアルでは、Microsoft が公開している Jenkins CI 用の Azure Storage プラグインを使用します。

## Jenkins の概要

Jenkins では、開発者がコードの変更を簡単に統合し、ビルドを自動的に頻繁に生成することができ、開発者の生産性が向上するため、ソフトウェア プロジェクトの継続的な統合を実現できます。 ビルドはバージョン管理され、ビルド アーティファクトをさまざまなリポジトリにアップロードできます。 このトピックでは、Azure BLOB ストレージをビルド アーティファクトのリポジトリとして使用する方法について説明します。 また、Azure BLOB ストレージから依存関係をダウンロードする方法も紹介します。

Jenkins の詳細については掲載されて [対応 Jenkins:operator[]][]します。

## BLOB サービスを使用するメリット

BLOB サービスを使用してアジャイル開発のビルド アーティファクトをホストするメリットには、次の点が挙げられます。

- ビルド アーティファクトやダウンロード可能な依存関係に高可用性を実現。
- Jenkins CI ソリューションでビルド アーティファクトをアップロードする際のパフォーマンスを改善。
- 顧客およびパートナーがビルド アーティファクトをダウンロードする際のパフォーマンスを改善。
- 匿名アクセス、有効期限ベースの Shared Access Signature によるアクセス、プライベート アクセスなどから 1 つを選んでユーザー アクセス ポリシーを制御。

## 前提条件

Jenkins CI ソリューションで BLOB サービスを使用するには、次のものが必要です。

- Jenkins 継続的インテグレーション ソリューション。

    Jenkins CI ソリューションがない場合には、次の方法によって Jenkins CI ソリューションを実行できます。

    1. Java が有効なコンピューターから jenkins.war をダウンロード <http://jenkins-ci.org>します。
    2. コマンド プロンプトを開いて jenkins.war が格納されているフォルダーに移動し、次のコマンドを実行します。

        `java の jar jenkins.war`

    3. ブラウザーで開く `http://localhost:8080/`します。 Jenkins ダッシュボードが開きます。ここで、Azure Storage プラグインをインストールおよび構成できます。

        通常の Jenkins CI ソリューションであればサービスとして実行されるように設定しますが、このチュートリアルではコマンド ラインで Jenkins.war を実行するだけで十分です。

- Azure アカウント。Azure アカウントにサインアップすることができます <http://www.azure.com>します。

- Azure ストレージ アカウント。 ストレージ アカウントがない場合の手順を使用してを作成できます [をストレージ アカウントのを作成する方法][]します。

- 以降では、Jenkins CI のビルド アーティファクトで BLOB サービスをリポジトリとして使用するうえで必要な手順を、基本的な例を使って説明しています。Jenkins CI ソリューションにある程度習熟していることが望ましいものの、必須ではありません。

## Jenkins CI で BLOB サービスを使用する方法

Jenkins で BLOB サービスを使用するには、Azure Storage プラグインをインストールし、そのプラグインを構成してストレージ アカウントを使用するようにしたうえで、ビルド後にビルド アーティファクトをストレージ アカウントにアップロードするアクションを作成する必要があります。 以降のセクションでは、ここに挙げた手順について説明します。

## Azure Storage プラグインのインストール方法

1. Jenkins ダッシュボードで、**[Manage Jenkins]** をクリックします。
2. **[Manage Jenkins]** ページで **[Manage Plugins]** をクリックします。
3. **[Available]** タブをクリックします。
4. **[Artifact Uploaders]** セクションで、**Microsoft Azure Storage プラグイン**を確認します。
5. [**Install without restart**] と [**Download now and install after restart**] のいずれかをクリックします。
6. Jenkins を再起動します。

## Azure Storage プラグインを構成してストレージ アカウントを使用する方法

1. Jenkins ダッシュボードで、**[Manage Jenkins]** をクリックします。
2. **[Manage Jenkins]** ページで **[Configure System]** をクリックします。
3. **[Microsoft Azure Storage Account Configuration]** セクションで、次の操作を行います。
    1. 取得した、ストレージ アカウント名を入力して、 [Azure ポータル](portal.azure.com)します。
    2. ストレージ アカウント キーを入力します同様に、 [Azure ポータル](portal.azure.com)します。
    3. パブリック Azure クラウドを使用している場合、**[Blob Service Endpoint URL]** には既定値を使用します。 異なる Azure クラウドを使用している場合で指定されているエンドポイントを使用して、 [Azure ポータル](portal.azure.com) 、ストレージ アカウント用です。
    4. **[Validate storage credentials]** をクリックしてストレージ アカウントを検証します。
    5. [省略可能] Jenkins CI で利用できるストレージ アカウントを追加する場合には、**[Add more Storage Accounts]** をクリックします。
    6. **[Save]** をクリックして設定を保存します。

## ビルド後にビルド アーティファクトをストレージ アカウントにアップロードするアクションの作成方法

説明のため、ストレージ アカウントにファイルをアップロードするためのビルド後のアクションを追加する前に、複数のファイルを作成するジョブを作成する必要があります。

1. Jenkins ダッシュボードで、**[New Item]** をクリックします。
2. ジョブの名前を **MyJob** に設定し、**[Build a free-style software project]**、**[OK]** の順にクリックします。
3. ジョブ構成の **[Build]** セクションで **[Add build step]** をクリックした後、**[Execute Windows batch command]** を選択します。
4. **[Command]** で次のコマンドを使用します。

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt

5. ジョブ構成の **[Post-build Actions]** セクションで **[Add post-build action]** をクリックした後、**[Upload artifacts to Microsoft Azure Blob storage]** を選択します。
6. **[Storage Account Name]** では、使用するストレージ アカウントを選択します。
7. **[Container name]** では、コンテナー名を指定します (コンテナーは、ビルド アーティファクトをアップロードする時点で存在していなければ、自動で作成されます)。 環境変数を使用することもできます。この例では、コンテナー名に「**${JOB_NAME}**」と入力します。

    **ヒント**

    **[Execute Windows batch command]** にスクリプトを入力した **[Command]** セクションの下には、Jenkins が認識できる環境変数へのリンクがあります。 環境変数の名前および説明を確認するには、リンクをクリックします。 **BUILD_URL** など、特殊文字が含まれる環境変数は、コンテナー名および共通仮想パスに使用できません。

8. この例では **[Make new container public by default]** をクリックします。 (プライベート コンテナーを使用する場合には、Shared Access Signature を作成してアクセスを許可する必要があります。 ただし、この点についてはこのトピックでは取り扱いません。 で共有アクセス署名に関する詳細については、 [共有アクセス署名の作成](http://go.microsoft.com/fwlink/?LinkId=279889).)
9. [省略可能] ビルド アーティファクトをアップロードする前にコンテナーの内容をクリアする場合、**[Clean container before uploading]** をクリックします (コンテナーの内容をクリアしない場合は、チェック ボックスをオフにします)。
10. **[List of Artifacts to upload]** では、「**text/*.txt**」と入力します。
11. **アップロードされたアーティファクトの共通仮想パス**, このチュートリアルの目的を入力、 **${build \_id}/${build \_number}**します。
12. **[Save]** をクリックして設定を保存します。
13. Jenkins ダッシュボードで、**[Build Now]** をクリックして **MyJob** を実行します。 コンソール出力でステータスを確認します。 ビルド後のアクションによってビルド アーティファクトのアップロードが開始されると、コンソール出力に Azure Storage に関するステータス メッセージが表示されます。
14. ジョブが正常に完了すると、パブリック BLOB を開いてビルド アーティファクトを確認できます。
    1. ログイン、 [Azure ポータル](portal.azure.com)します。
    2. **[Storage]** をクリックします。
    3. Jenkins に使用したストレージ アカウント名をクリックします。
    4. **[コンテナー]** をクリックします。
    5. **myjob** という名前のコンテナーをクリックします。これは、Jenkins ジョブを作成したときに割り当てたジョブ名を小文字にしたものです。 Azure ストレージでは、コンテナー名と BLOB 名は小文字です (大文字と小文字は区別されます)。 **myjob** という名前のコンテナーの BLOB の一覧に、**hello.txt** と **date.txt** の 2 つがあります。 そのどちらかの URL をコピーして、ブラウザーで開きます。 このテキスト ファイルがビルド アーティファクトとしてアップロードされていることがわかります。

アーティファクトを Azure BLOB ストレージにアップロードするビルド後のアクションは、ジョブごとに 1 つのみ作成できます。 **[List of Artifacts to upload]** でセミコロンを区切り記号として使用することで、アーティファクトを Azure BLOB ストレージにアップロードするビルド後のアクション 1 つに、(ワイルドカードを含む) 複数のファイルとファイル パスを指定できます。 たとえば、Jenkins ビルドする場合 JAR ファイルとファイルが生成 TXT ワークスペースの **ビルド** ] フォルダーは、両方を Azure blob ストレージにアップロードするには、次を使用します、 **の Artifacts to upload] の一覧** 値: **build/\*.jar;build/\*.txt**します。 また、2 重コロンの構文を使用すると、BLOB 名で使用するパスを指定できます。 たとえば、jar ファイルのアップロードを使用して **バイナリ** blob パスと、TXT ファイルを使用してアップロード **通知** blob パスでは、次を使用して、 **の Artifacts to upload] の一覧** 値: **build/\*.jar::binaries;build/\*.txt::notices**します。

## Azure BLOB ストレージからのダウンロードを実行するビルド手順の作成方法

次の手順では、Azure BLOB ストレージから項目をダウンロードするビルド手順を構成する方法を示します。 この手順は、Azure BLOB ストレージに保持している JAR ファイルなどの項目をビルドに含める場合に便利です。

1. ジョブ構成の **[Build]** セクションで **[Add build step]** をクリックし、**[Download from Azure Blob storage]** を選択します。
2. **[Storage Account Name]** では、使用するストレージ アカウントを選択します。
3. **[Container name]** では、ダウンロードする BLOB が格納されているコンテナーの名前を指定します。 環境変数を使用できます。
4. **[Blob name]** には BLOB 名を指定します。 環境変数を使用できます。 また、アスタリスクを使用して、BLOB 名の先頭文字 (複数可) の後にワイルドカードを指定できます。 たとえば、 **プロジェクト *** で始まる名前のすべての blob を指定 **プロジェクト**します。
5. [省略可能] **[Download path]** では、Jenkins を実行しているコンピューター上のパスを指定します。Azure BLOB ストレージのファイルはこのパスにダウンロードされます。 環境変数も使用できます (**[Download path]** に値を入力しない場合、Azure BLOB ストレージのファイルは、ジョブのワークスペースにダウンロードされます)。

Azure BLOB ストレージからダウンロードする項目が他にもある場合は、追加のビルド手順を作成できます。

目的の BLOB が正常にダウンロードされていることを確かめるには、ビルドを実行した後に、ビルド履歴のコンソール出力またはダウンロード先を確認します。

## BLOB サービスが使用するコンポーネント

以下では、BLOB サービス コンポーネントの概要を説明します。

- **ストレージ アカウント**: Azure Storage にアクセスするときは必ずストレージ アカウントを使用します。 これは、アクセスする BLOB の名前空間の 中でも最高レベルに位置するものです。 アカウントに格納できるコンテナーの数は、 コンテナーの合計サイズが 100 TB 未満である限り無制限です。
- **コンテナー**: コンテナーは、一連の BLOB をグループ化します。 すべての BLOB はコンテナーに格納されている必要があります。 1 つのアカウントに格納できるコンテナーの数は無制限です。 また、1 つのコンテナーに保存できる BLOB の数も無制限です。
- **BLOB**: 任意の種類およびサイズのファイルです。 Azure Storage に格納できる BLOB には、ブロック BLOB とページ BLOB の 2 種類があります。 ほとんどのファイルは ブロック BLOB です。 1 つのブロック BLOB には、最大で 200 GB までのデータを格納できます。 このチュートリアルでは、 ブロック BLOB を使用します。 もう 1 つの種類の BLOB であるページ BLOB には、最大 1 TB までの データを格納できます。ファイルのバイト数の範囲が頻繁に変更される 場合には、こちらの方が効率的です。 Blob の詳細については、次を参照してください。 [ブロック Blob およびページ Blob について](http://msdn.microsoft.com/library/azure/ee691964.aspx)します。
- **URL 形式**: BLOB は、次の URL 形式を使用してアドレス指定できます。

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`

    (ここに挙げた形式は、パブリック Azure クラウドに適用されるものです。 異なる Azure クラウドを使用している場合は、内のエンドポイントを使用して、 [Azure ポータル](portal.azure.com) URL エンドポイントを指定します)。

    形式では、 `storageaccount` 、ストレージ アカウントの名前を表す `container_name` 、コンテナーの名前を表すと `blob_name` 、blob の名前をそれぞれ表します。 コンテナー名にはパスを複数使用することができます。その場合には、スラッシュ (**/**) で区切ります。 このチュートリアルではコンテナー名の例が **MyJob**, 、および **${build \_id}/${build \_number}** 共通仮想パスは、blob の url は次のように使用されました。

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## 次のステップ


[how to create a storage account]: http://go.microsoft.com/fwlink/?LinkId=279823 
[meet jenkins]: https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins 

