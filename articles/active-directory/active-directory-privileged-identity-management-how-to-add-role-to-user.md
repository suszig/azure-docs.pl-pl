<properties
   pageTitle="Azure Privileged Identity Management: ユーザーへのロールの追加を開始する方法"
   description="Azure Privileged Identity Management 拡張機能で特権 ID にロールを追加する方法について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="IHenkel"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2015"
   ms.author="inhenk"/>

# Azure Privileged Identity Management: ユーザー ロールを追加または削除する方法

## ユーザー ロールの追加または削除
移動するいくつかの方法、 **追加管理されているユーザー ブレード** PIM のインターフェイスのです。 それぞれのクリック シーケンスを次に示します。

## ユーザー ロールの追加または削除へのパス
- ダッシュボード > [管理者ロール] の [ユーザー] > [追加] または [削除]
- ダッシュボード > [ロールの概要] > [すべてのユーザー] 一覧 > [追加] または [削除]
- ダッシュボード > ロール テーブルでユーザー ロールをクリック (たとえば [グローバル管理者]) > [追加] または [削除]

## [管理者ロール] セクションの [ユーザー] または [ロールの概要] の [すべてのユーザー] 一覧から、ユーザーにロールを追加する
移動した後、 **管理対象ユーザーの追加** ブレード.

1. をクリックして **ロールを選択して**します。 ロール テーブルでユーザー ロールをクリックしてここに移動した場合は、ロールは既に選択されています。
2. ロールの一覧からロールを選択します。 たとえば、 **パスワード管理者**, 、 **のユーザーを選択** ブレードが開きます。
3. 検索フィールドに、ロールを追加するユーザーの名前を入力します。  ユーザーがディレクトリに存在する場合は、入力すると似た名前の他のユーザーと共にアカウントが表示されます。
4. 一覧で、ユーザーを選択して [ **実行**します。
5. をクリックして **OK** 、選択肢を保存します。  これを行わないと、選択は保存されません。 選択したユーザーが一覧に表示されます。ロールは一時的です。
6. ロールを永続的にする場合は、一覧のユーザーをクリックします。 ユーザーの情報が新しいブレードに表示されます。 選択 **] アクセス許可を行う** ユーザー情報] メニューにします。
7. をクリックして **Activate** をこのロールのユーザーのアクティブに要求を開始します。  アクティブ化する理由を入力、 **申請理由** テキスト フィールドです。  この時点で、このユーザーのロールが自動的にアクティブになり、グローバル管理者に通知が送信されます。

## ロールからユーザーを削除する
1. 上で説明したパスの 1 つを使用してユーザー ロールの一覧でユーザーに移動します。
2. ユーザーの一覧でユーザーをクリックします。
3. をクリックして **削除**します。  確認メッセージが表示されます。
4. [はい] をクリックしてユーザーからロールを削除します。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
##次のステップ
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

