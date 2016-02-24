    <properties
    pageTitle="Add a Git artifact repository to your DevTest Lab | Microsoft Azure"
    description="Add a GitHub or Visual Studio Team Services Git repository for your custom artifacts to your lab"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/01/2015"
    ms.author="tarcher"/>

# DevTest ラボへの Git アーティファクト リポジトリの追加

## 概要

既定では、公式の Azure DevTest ラボ アーティファクト リポジトリからのアーティファクトが DevTest ラボに含まれます。 ラボに Git アーティファクト リポジトリを追加することで、チームが作成したアーティファクトを含めることができます。 リポジトリをホストできる [GitHub](https://github.com) または [Visual Studio Team Services (VSTS)](https://visualstudio.com)します。

- GitHub リポジトリを作成する方法については、次を参照してください。 [GitHub Bootcamp](https://help.github.com/categories/bootcamp/)します。
- Git リポジトリをチームのサービス プロジェクトを作成する方法については、次を参照してください。 [Visual Studio チームのサービスへの接続](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online)します。

次のスクリーン ショットでは、成果物を含むリポジトリが GitHub で検索する方法の例を示します。  
![GitHub アーティファクト リポジトリのサンプル](./media/devtest-lab-add-artifact-repo/devtestlab-github-artifact-repo-home.png)

## GitHub のアーティファクト リポジトリのラボへの追加

ラボに GitHub のアーティファクト リポジトリを追加するには、最初に HTTPS クローン URL と個人用アクセス トークンをアーティファクト リポジトリから取得し、次にラボでその情報を入力します。

### GitHub リポジトリのクローン URL と個人用アクセス トークンの取得

1. チームの成果物を含む GitHub リポジトリのホーム ページでは、保存、 **クローン url が HTTPS** を後で使用します。

1. 右上隅でプロファイル画像をタップし、選択 **設定**します。

1.  **個人設定** メニューで、左、タップ **個人アクセス トークン**します。

1. タップ **新しいトークンを生成する**です。

1.  **新しい個人用アクセス トークン** ページで、入力、 **トークンの説明**, で既定の項目をそのまま使用、 **スコープを選択**, を選択し **トークンの生成**します。

1. 後で必要になるため、生成されたトークンを保存します。

1. これで、GitHub を閉じることができます。   

###GitHub リポジトリに、ラボを接続します。

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. タップ **参照**, 、順にタップ **DevTest ラボ** リストからです。

1. ラボの一覧で目的のラボをタップします。   

1. ラボのブレードで、[タップ **設定**します。

1. ラボの **設定** ブレードで、tap **アーティファクトのリポジトリ**します。

1.  **アーティファクトのリポジトリ** ブレード。

    1. 入力、 **名前** リポジトリにします。
    1. 保存されている入力 **Git クローン Url**します。
    2. 入力、 **フォルダー パス** アーティファクトが格納されている成果物のリポジトリにします。
    3. 保存されている入力 **個人アクセス トークン** アーティファクトのリポジトリにします。
    4. タップ **保存**します。

リポジトリ内のアイテムが一覧表示されます、 **成果物の追加** ブレードです。

## ラボへの Visual Studio Git のアーティファクト リポジトリの追加

ラボに Visual Studio Git のアーティファクト リポジトリを追加するには、最初に HTTPS クローン URL と個人用アクセス トークンをアーティファクト リポジトリから取得し、次にラボでその情報を入力します。

### アーティファクト プロジェクトの Visual Studio Web ページ

1. チーム コレクションのホーム ページ (例、 `https://contoso-web-team.visualstudio.com`) を開き、アーティファクト プロジェクトをタップします。

2. プロジェクトのホーム ページのタップ **コード**します。

1. プロジェクトのクローン URL を表示する **コード** ] ページで、タップ **複製**します。

1. このチュートリアルで後ほど必要になるため、URL を保存します。

1. 個人用アクセス トークンを作成するには、タップ **プロフィール** 、ユーザー アカウント] ドロップダウン メニューからです。

1. プロファイル情報ページ tap に、 **セキュリティ**します。

1.  **セキュリティ** ] タブで、タップ **追加**します。

1.  **個人のアクセス トークンを作成する** ページ。

    1. 入力、 **説明** トークンにします。
    2. 選択 **180 日間** から、 **の有効期限が切れる** ] ボックスの一覧です。
    3. 選択 **アクセス可能なすべてのアカウント** から、 **アカウント** ] ボックスの一覧です。
    4. 選択、 **すべてのスコープ** オプション。
    5. 選択 **トークンを作成する**です。

1. 完了したら、新しいトークンが [に移動、 **個人用のアクセス トークン** ] ボックスの一覧です。 タップ **トークン コピー** はすぐに使用し、トークンの値を保存します。

### DevTest ラボ

1. ラボのブレードで、tap **設定**します。

    ![設定の選択](./media/devtest-lab-add-artifact-repo/devtestlab-add-artifacts-repo-open-dtl-settings.png)

1.  **設定** ブレードで、tap **アーティファクトのリポジトリ**します。

1.  **アーティファクトのリポジトリ** ブレード

    1. ディスプレイを入力して **名** リポジトリのです。
    1. 保存されている入力 **Git クローン Url**します。
    2. 入力、 **フォルダー パス** アーティファクトが格納されている成果物のリポジトリにします。
    3. 保存されている入力 **個人アクセス トークン** アーティファクトのリポジトリにします。
    4. タップ **保存**します。

