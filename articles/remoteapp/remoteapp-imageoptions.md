<properties
    pageTitle="Azure RemoteApp イメージの作成 | Microsoft Azure"
    description="Azure RemoteApp のイメージ作成に使用できるオプションについて説明します。"
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




# Azure RemoteApp イメージの作成

Azure RemoteApp は、イメージを使用してユーザーと共有しているアプリを保持します。 クラウドまたはハイブリッドであるかどうかをお好みのアプリケーションの Azure RemoteApp コレクションを作成するには、インストールされているこれらのアプリケーションのイメージを作成して開始します。 次に、そのイメージを使用するコレクションを作成し、コレクションにユーザーを割り当てて、それらのユーザーにアプリを発行します。

イメージの作成または使用には、いくつかのオプションあります。 基本的な [要件](remoteapp-imagereqs.md) として、イメージが Windows Server 2012 R2 を実行し、インストールされているリモート デスクトップ セッション ホスト (RDSH) ロールを持っていること。 興味深いのは、その取得方法です。

イメージの場合、次のオプションがあります。

- インポートして使用すると、 [Azure の仮想マシンに基づくイメージ](remoteapp-image-on-azurevm.md)します。 これは、カスタム設定を必要とする基幹業務アプリに最適です。 イメージをカスタマイズして、アプリ用に動作させることができます。
- 実行できます [を作成し、カスタム イメージをアップロード](remoteapp-create-custom-image.md)します。 オンプレミスのリモート デスクトップ サービス デプロイに使用するイメージが既にある場合は、これが最適です。
- いずれかを使用することができます、 [テンプレート イメージ](remoteapp-images.md) RemoteApp サブスクリプションに含まれています。 これらのイメージは、RemoteApp チームによって作成および管理されているもので、イメージに含まれる標準的なアプリケーション (Office スイートなど) をユーザーが使用できます。 なお、運用設定では Office 365 Pro Plus イメージのみ使用可能です。

イメージの取得や、作成する方法に関係なくを理解することを確認しますが、 [アプリ要件](remoteapp-appreqs.md) アプリが RemoteApp で快適に動作するためにします。 次に、次は、作成する、 [クラウド](remoteapp-create-cloud-deployment.md) または [ハイブリッド](remoteapp-create-hybrid-deployment.md) コレクションです。





