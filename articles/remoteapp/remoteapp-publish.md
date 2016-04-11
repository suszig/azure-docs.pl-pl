<properties
    pageTitle="Azure RemoteApp のアプリの発行 | Microsoft Azure"
    description="Azure RemoteApp のアプリとリソースを発行する方法について説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2015"
    ms.author="elizapo" />


# RemoteApp のアプリを発行する方法

RemoteApp コレクションの作成後、ユーザーが使用できるアプリまたはリソースを発行する必要があります。 サブスクリプションで提供されるテンプレート イメージには、既定で発行されているいくつかのアプリしか含まれていません。その他のアプリを共有するにはアプリを公開する必要があります。

> [AZURE.NOTE] アプリを更新する必要がありますか。 必要があります [イメージを更新](remoteapp-update.md) 最初です。

 **発行** ポータル] タブ、[ **発行**します。 テンプレートのイメージからアプリを追加することができます **開始** メニューまたはテンプレート イメージ上のアプリケーションのインストール場所にパスを指定します。 追加する場合、 **開始** ] メニューの [一覧から発行するアプリケーションを選択します。 アプリのパスを指定する場合は、アプリの名前とアプリのパスを入力します。 パスには、"c:\" ではなく、たとえば次のような変数を使用します - ""%systemdrive%"

> [AZURE.NOTE] アプリを追加する場合、 **開始** 権が必要] メニューの [ *アプリに追加、 **開始** テンプレート イメージ] メニューにします。*それ以外の場合、RemoteApp のみが表示内容 *は* 上、 **開始** ] メニューの [したはわかりにくくなります。 

>アプリが、確認、 **開始** ] メニューの [ショートカット ファイルの配置 **.lnk** - %systemdrive%\ProgramData\Microsoft\Windows\Start メニュー \ プログラム フォルダー内です。

> アプリを追加しなかった場合、 **開始** アプリケーションへのパスを追加する、テンプレートの作成時にメニューを選択します。 (またはテンプレート イメージを再作成することもできますが、これは少し手間のかかる作業です)。


 
