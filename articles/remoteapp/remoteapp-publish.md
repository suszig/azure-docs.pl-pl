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
> [AZURE.NOTE] アプリを更新する場合には、 必要があります [イメージを更新](remoteapp-update.md) 最初です。

ポータルの **[発行しています]** タブをクリックして、**[発行]** をクリックします。 **[スタート]** メニューのテンプレート イメージからアプリを追加することも、テンプレート イメージにインストールされているアプリの場所の指定することもできます。 **[スタート]** メニューから追加する場合は、一覧から発行するアプリケーションを選択します。 アプリのパスを指定する場合は、アプリの名前とアプリのパスを入力します。 パスには、"c:\" ではなく、たとえば次のような変数を使用します - ""%systemdrive%"
> [AZURE.NOTE] アプリケーションを追加する場合、 **開始** ] メニューの [おく必要があります * アプリに追加、 **開始** 、テンプレート image.* それ以外の場合、RemoteApp のメニューがどのようなを表示のみ *は* 上、 **開始** ] メニューの [したはわかりにくくなります。 

>アプリを **[スタート]** メニューに表示するには、%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs フォルダーの中にショートカット ファイル (**.lnk**) を作成します。
>
> テンプレートを作成するときに **[スタート]** メニューにアプリを追加しなかった場合は、アプリのパスを追加する方法を選択します (またはテンプレート イメージを再作成することもできますが、これは少し手間のかかる作業です)。







