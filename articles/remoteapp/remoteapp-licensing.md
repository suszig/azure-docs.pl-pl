<properties
    pageTitle="Azure RemoteApp ライセンス | Microsoft Azure"
    description="Azure RemoteApp のライセンスの仕組みについて説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="elizapo" />


# RemoteApp のライセンスはどのような仕組みになっていますか。


Azure RemoteApp サービスを設定し、テンプレートを作成したので、ユーザーにアプリケーションを発行する準備ができました。 最後にもうひとつ解決することがあります: ライセンスです。 RemoteApp のライセンスはどのような仕組みになっていますか。また、RemoteApp を通して共有するアプリケーションについてはどうですか。

RemoteApp は、Windows ライセンスもリモート デスクトップ CAL も必要としません。 サブスクリプションにより、RemoteApp 側自体のライセンス処理が行われます  (チェック アウトの詳細、 [料金プラン](../../../pricing/details/remoteapp/).)

サブスクリプションに含まれているイメージの 1 つを使用する場合、そのイメージにインストールされたアプリはすべて共有できます。その際、個別のライセンスは必要ありません。 たとえば、Windows Server 2012 R2 テンプレート イメージを使用してコレクションを作成すると、System Center Endpoint Protection をユーザーと共有できます。 このルールの例外として、個別のサブスクリプションを必要とする Office 365 ProPlus と、実稼働コレクションでは共有できない Office 2013 があります。

RemoteApp に付属するテンプレート イメージを Office 365 を使用する場合が必要な *既存* Office 365 ProPlus プランです。 カスタム テンプレートを使用して発行する Office 365 アプリも同様です。 自分のサブスクリプションでのアプリを有効化する必要があります。 これは評価版でも有料サブスクリプションでも同様です。 試用期間中に、Office 365 テンプレート イメージを使用する場合 *サブスクリプションがないと*, に Office 365 ページにアクセスして [サインアップ](https://go.microsoft.com/fwlink/p/?LinkID=403802) 試用版サブスクリプションにします。 参照してください [RemoteApp と Office 連携するしくみですか?](remoteapp-o365.md) の詳細。

試用期間中に、Office 365 試用版のサブスクリプションが必要ない場合は、RemoteApp に付属の Office 2013 Professional Plus テンプレート イメージを使用します。 このテンプレート イメージは 30 日間のみ使用でき、有料のコレクションに変換することはできません。

その他のアプリケーションについては、そのアプリケーションを共有するためのライセンスを所有していることを確認します。

ご理解いただけたでしょうか。 法的に共有が可能なすべてのアプリを発行することができます。 またプログラムを共有する資格を確認する必要があります。

クラウド コレクションでは CAL またはボリューム ライセンス契約を使用できないことに注意してください。  *できます* ボリューム ライセンス契約を使用して、(Office を除く)、ハイブリッド コレクション内のアプリケーションをアクティブ化します。 ボリューム ライセンス メディアから、テンプレート イメージにインストールする必要があります。 アプリケーションのベンダの情報に従ってリモート デスクトップ環境にライセンスをインストールしてください。

