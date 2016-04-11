<properties 
   pageTitle="ホスト名の表示と変更 | Microsoft Azure"
   description="名前解決のため、Azure Virtual Machines のホスト名、および Web ロールと Worker ロールを表示、変更する方法"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/07/2015"
   ms.author="joaoma" />

# ホスト名の表示と変更

ロール インスタンスをホスト名で参照できるようにするには、各ロールのサービス構成ファイルでホスト名の値を設定する必要があります。 目的のホスト名を追加すれば、 **vmName** の属性、 **ロール** 要素。 値、 **vmName** 属性は、各ロール インスタンスのホスト名のベースとして使用します。 たとえば場合、 **vmName** は *webrole* とそのロールの 3 つのインスタンスがあるため、インスタンスのホスト名になります *webrole0*, 、*webrole1*, 、および *webrole2*します。 仮想マシンのホスト名は、仮想マシン名に基づいて設定されるため、構成ファイルでバーチャル マシンのホスト名を指定する必要はありません。Microsoft Azure サービスの構成の詳細については、次を参照してください [Azure サービス構成スキーマ (.cscfg ファイル)。](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## ホスト名の表示

さまざまなツールを使用して、クラウド サービスで仮想マシンとロール インスタンスのホスト名を表示することができます。 Azure ポータル、サービス構成ファイル、リモート デスクトップと [Azure サービス管理 REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)します。

### Azure ポータル

Azure ポータルを使用して、仮想マシンのホスト名を仮想マシン ダッシュボード ページに表示できます。 ダッシュ ボードの値が表示されることに留意してください **名** と **ホスト名**します。 これらは最初は同じですが、ホスト名を変更しても、仮想マシンまたはロール インスタンスの名前は変わりません。

ロール インスタンスは Azure ポータルでもわかりますが、クラウド サービスでインスタンスの一覧を表示しても、ホスト名は表示されません。 各インスタンスの名前は表示されますが、その名前はホスト名ではありません。

### サービス構成ファイル

展開済みサービスのサービス構成ファイルをダウンロードすることができます、 **構成** Azure ポータルでサービスのページです。 表示し、 **vmName** 属性を **ロール名** 要素をホスト名を参照してください。 このホスト名は各ロール インスタンスのホスト名に対するベースとして使用されることに留意してください。 たとえば場合、 **vmName** は *webrole* とそのロールの 3 つのインスタンスがあるため、インスタンスのホスト名になります *webrole0*, 、*webrole1*, 、および *webrole2*します。

### リモート デスクトップ

仮想マシンまたはロール インスタンスに対するリモート デスクトップ接続 (Windows)、Windows PowerShell リモート処理接続 (Windows)、または SSH 接続 (Linux と Windows) を有効にした後は、さまざまな方法でアクティブなリモート デスクトップ接続からホスト名を表示できます。

- コマンド プロンプトまたは SSH ターミナルで「hostname」と入力します。

- コマンド プロンプトで「ipconfig /all」と入力します (Windows のみ)。

- システム設定でコンピューター名を表示します (Windows のみ)。

### Azure Service Management REST API

REST クライアントから次の手順を実行します。

1. Azure ポータルに接続するためのクライアント証明書があることを確認します。 クライアント証明書を取得するに表示される手順に従います [方法: ダウンロードとインポート発行の設定とサブスクリプション情報](https://msdn.microsoft.com/library/dn385850.aspx)します。 

1. x-ms-version という名前のヘッダー エントリの値を 2013-11-01 に設定します。

1. 次の形式で要求を送信します https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true。

1. 検索、 **HostName** 要素ごと **RoleInstance** 要素。

>[AZURE.WARNING] チェックして、REST 呼び出し応答からクラウド サービスの内部ドメイン サフィックスを表示することもできる、 **InternalDnsSuffix** 要素、/(Windows)、リモート デスクトップ セッションでコマンド プロンプトから ipconfig を実行して、またはを実行して端末から cat/etc/resolv.conf、SSH (Linux) またはです。

## ホスト名の変更

変更したサービス構成ファイルをアップロードすることにより、またはリモート デスクトップ セッションからコンピューターの名前を変更することにより、仮想マシンまたはロール インスタンスのホスト名を変更できます。

## 次のステップ

[名前解決 (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure サービス構成スキーマ (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network の構成スキーマ](http://go.microsoft.com/fwlink/?LinkId=248093)

[ネットワーク構成ファイルを使用した DNS 設定の指定](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

