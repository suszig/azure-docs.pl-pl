<properties
    pageTitle="Oracle WebLogic Server および Database VM | Microsoft Azure"
    description="リソース マネージャーのデプロイ モデルを使用して、Windows Server 2012 で実行される Oracle WebLogic Server 12c および Oracle Database 12c Azure イメージを作成します。"
    services="virtual-machines"
    authors="bbenz"
    documentationCenter=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="06/22/2015"
    ms.author="bbenz" />

#Azure での Oracle WebLogic Server 12c および Oracle Database 12c の仮想マシンの作成

この記事では、Windows Server 2012 で実行しているマイクロソフト提供の Oracle WebLogic Server 12c および Oracle Database 12c のイメージに基づいて、Azure で仮想マシンを作成する方法を示しています。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


##Azure で Oracle WebLogic Server 12c および Oracle Database 12c の仮想マシンを作成するには

1. [Azure ポータル](https://ms.portal.azure.com/)にサインインします。

2.  クリックして、 **Marketplace**, 、] をクリックして **コンピューティング**, 、し、入力 **Oracle** 検索ボックスにします。

3.  選択、 **Oracle Database 12c および Windows Server 2012 上の WebLogic Server 12c Standard Edition** または **Oracle Database 12c および WebLogic Server 12c Enterprise Edition の Windows Server 2012** イメージ。 (最小推奨サイズなど)、このイメージに関する情報を確認し、クリックして **次**します。

4.  指定する **ホスト名** 、仮想マシンのです。

5.  指定する **ユーザー名** 、仮想マシンのです。 なお、このユーザー名は仮想マシンにリモートログインするためのもので、Oracle データベースのユーザー名ではありませんのでご注意ください。

6.  仮想マシンのパスワードを指定し確認するか、または Secure Shell (SSH) 公開キーを入力します。

7.  選択、 **価格レベル**します。  既定では推奨される価格レベルが表示されます。 すべての構成オプションを表示するには、クリックして **をすべて表示** 右上にします。

8. 必要に応じて、オプションの構成を設定します。 次の考慮事項に従います。

    a. ままにして **ストレージ アカウント** としてでは、仮想マシンの名前で新しいストレージ アカウントを作成します。

    b. ままにして **可用性セット** として **未構成**します。

    c. この時点ではエンドポイントを追加しないでください。

9.  リソース グループを選択または作成します。 詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](resource-group-portal.md)です。

10. 選択、 **サブスクリプション**します。

11. 選択、 **場所**します。


##この仮想マシンでホストされているデータベースを作成するには

指示に従って、 [Azure で Oracle Database 12c 仮想マシンを作成](virtual-machines-creating-oracle-database-virtual-machine.md), で始まる、 **を Azure で Oracle Database 12c 仮想マシンを使用して、データベースを作成する** セクションです。

##この仮想マシンでホストされている Oracle WebLogic Server 12c を構成するには
指示に従って、 [Azure で Oracle WebLogic Server 12c 仮想マシンを作成](virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine.md), で始まる、 **Azure で Oracle WebLogic Server 12c 仮想マシンを構成する** セクションです。 WebLogic Server クラスターをセットアップする場合にも表示 [Azure で Oracle WebLogic Server 12c クラスターを作成](virtual-machines-creating-oracle-webLogic-server-12c-cluster.md)します。

##その他のリソース
[Oracle 仮想マシンのイメージに関するその他の考慮事項](miscellaneous-considerations-for-oracle-virtual-machine-images-new-article.md)

[Oracle 仮想マシン イメージの一覧](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

[Java アプリケーションから Oracle Database に接続する](http://docs.oracle.com/cd/E11882_01/appdev.112/e12137/getconn.htm#TDPJD136)

[Microsoft Azure で Linux を使用する Oracle WebLogic Server 12c](http://www.oracle.com/technetwork/middleware/weblogic/learnmore/oracle-weblogic-on-azure-wp-2020930.pdf)

[Oracle Database 2 日間 DBA 12c リリース 1](http://docs.oracle.com/cd/E16655_01/server.121/e17643/toc.htm)


