<properties
    pageTitle="VM タスクに対応する Azure CLI コマンド | Microsoft Azure"
    description="Azure リソース マネージャー モードと Azure サービス管理モードで Azure VM を作成および管理するための同等の Azure CLI コマンドを紹介します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="command-line-interface"
    ms.workload="infrastructure-services"
    ms.date="08/28/2015"
    ms.author="danlep"/>



# Mac、Linux、Windows 用 Azure CLI で VM タスクに使用するリソース マネージャーとサービス管理の同等のコマンド

この記事では、Azure サービス管理と Azure リソース マネージャーで Azure VM を作成および管理するための Microsoft Azure コマンド ライン インターフェイス (Azure CLI) の同等のコマンドを紹介します。 この記事は、スクリプトをコマンド モード間で移行するための簡易ガイドとして使用してください。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



* Azure CLI をインストールして、サブスクリプションに接続していない場合、は、次を参照してください。 [Azure CLI をインストール](../xplat-cli-install.md) と [Azure CLI から Azure サブスクリプションへの接続](../xplat-cli-connect.md)します。 リソース マネージャー モードのコマンドを使用する場合は、必ず所定のログイン方法を使用して接続してください。

* をコマンド モードの Azure CLI とスイッチで、リソース マネージャー モードを開始するには、「 [リソース マネージャーで Azure コマンド ライン インターフェイスを使用して](xplat-cli-azure-resource-manager.md)します。 既定では、CLI はサービス管理モードで起動します。 リソース マネージャー モードに変更するには、実行 `azure config モード arm`します。 サービス管理モードに戻るには、実行 `azure config モード asm`します。

* オンライン コマンドのヘルプとオプションでは、次のように入力します。 `azure < コマンド >< サブコマンド >--ヘルプ` または `azure ヘルプ < コマンド >< サブコマンド >`します。

## VM タスク

次の表では、サービス管理とリソース マネージャーで Azure CLI コマンドを使用して実行できる一般的な VM タスクを比較します。 リソース マネージャーの多くのコマンドでは、既存のリソース グループの名前を渡す必要があります。
> [AZURE.NOTE] ここで紹介する例には、リソース マネージャーでのテンプレート ベースの操作は含まれていません。 詳細については、次を参照してください。 [リソース マネージャーで Azure コマンド ライン インターフェイスを使用して](xplat-cli-azure-resource-manager.md)します。

 タスク| サービス管理| リソース マネージャー
-------------- | ----------- | -------------------------
 最も基本的な VM の作成| `azure の仮想マシンの作成 < dns 名 >< イメージ > [ユーザー名] [password] の [オプション]`| `azure の仮想マシンの簡易作成 [オプション] < リソース グループ >< 名 >< 場所 >< os 種類 >< イメージ urn >< admin username >< 管理パスワード >`<br/><br/>(取得、 `イメージ urn` から、 `azure vm イメージ リスト` コマンドです)。
 Linux VM の作成| `azure の仮想マシンの作成 < dns 名 >< イメージ > [ユーザー名] [password] の [オプション]`| `azure の仮想マシンの作成 [オプション] < リソース グループ >< 名 >< 場所 > y"Linux"`
 Windows VM の作成| `azure の仮想マシンの作成 < dns 名 >< イメージ > [ユーザー名] [password] の [オプション]`| `azure の仮想マシンの作成 [オプション] < リソース グループ >< 名 >< 場所 >-y"Windows"`
 VM の一覧表示| `azure vm list [オプション]`| `azure vm list [オプション] < resource_group >`
 VM に関する情報の取得| `azure 仮想マシンの表示] < vm_name > の [オプション]`| `azure vm show [オプション] < resource_group >< 名 >`
 VM の起動| `azure vm の開始 < 名前 > の [オプション]`| `azure vm の開始 < resource_group >< 名 > の [オプション]`
 VM の停止| `azure vm のシャット ダウン < 名前 > の [オプション]`| `azure vm の停止 < resource_group >< 名 > の [オプション]`
 VM の割り当て解除| 使用できません。| `azure vm の割り当てを解除 < リソース グループ >< 名 > の [オプション]`
 VM の再起動| `azure vm の再起動 < vname > の [オプション]`| `azure vm を再起動 < resource_group >< 名 > の [オプション]`
 VM の削除| `azure vm の削除 [オプション] < 名前 >`| `azure 仮想マシン名 > の < resource_group >< の [オプション] を削除します。`
 VM のキャプチャ| `azure vm のキャプチャ < 名前 > の [オプション]`| `azure vm のキャプチャ < resource_group >< 名 > の [オプション]`
 ユーザー イメージからの VM の作成| `azure の仮想マシンの作成 < dns 名 >< イメージ > [ユーザー名] [password] の [オプション]`| `azure の仮想マシンの作成 [オプション] – q < イメージ名 >< リソース グループ >< 名 >< 場所 >< os の種類 >`
 専用ディスクからの VM の作成| `azure の仮想マシンの作成 [オプション]-d < カスタム データ ファイルに >< dns 名 > [ユーザー名] [password]`| `azue の仮想マシンの作成 [オプション] – d < os のディスクの vhd >< リソース グループ >< 名 >< 場所 >< os の種類 >`
 VM へのデータ ディスクの追加| `azure vm ディスクのアタッチ [オプション] < vm 名 >< ディスク イメージ名 >` OR- <br/>  `vm ディスクを接続-新しい < vm 名 >< サイズで-gb > [blob の url] の [オプション]`| `azure vm ディスクを接続-新しい < リソース グループ >< vm 名 >< サイズで-gb > [vhd 名] の [オプション]`
 VM からのデータ ディスクの削除| `azure vm ディスクをデタッチ < vm 名 >< lun > の [オプション]`| `azure vm ディスクのデタッチ [オプション] < リソース グループ >< vm 名 >< lun >`
 VM への一般的な拡張機能の追加| `azure vm 拡張機能の設定 < vm 名 >< 拡張機能名 >< パブリッシャー名 >< バージョン > の [オプション]`| `azure vm 拡張機能設定の [オプション] < リソース グループ >< vm 名 >< 名 >< パブリッシャー名 >< バージョン >`
 VM への VM アクセス拡張機能の追加| 使用できません。| `azure vm リセット アクセス < リソース グループ >< 名 > の [オプション]`
 VM への Docker 拡張機能の追加| `azure vm の docker < dns 名 >< イメージ >< ユーザー名 > [パスワード] の [オプション] の作成します。`| `azure vm の docker < リソース グループ >< 名 >< 場所 >< os の種類 > の [オプション] の作成します。`
 VM への Chef 拡張機能の追加| `azure vm 拡張機能の取得-chef の < vm 名 > の [オプション]`| 使用できません。
 VM 拡張機能の無効化| `azure vm 拡張機能設定の [オプション] – b < vm 名 >< 拡張機能名 >< パブリッシャー名 >< バージョン >`| 使用できません。
 VM 拡張機能の削除| `azure vm 拡張機能設定の [オプション] – u < vm 名 >< 拡張機能名 >< パブリッシャー名 >< バージョン >`| `azure vm 拡張機能設定の [オプション] – u < リソース グループ >< vm 名 >< 名 >< パブリッシャー名 >< バージョン >`
 VM 拡張機能の一覧表示| `azure vm extension list [オプション]`| `azure vm 拡張機能は、[オプション] < リソース グループ >< vm 名 > を取得します。`
 VM イメージの一覧表示| `azure vm image list [オプション]`| `azure vm image list [オプション] < 場所 >< パブリッシャー > [プラン] [sku]` OR- <br/> `azure vm イメージの一覧、パブリッシャーの [オプション] < 場所 >` OR- <br/> `azure vm イメージ リストは [オプション] < 場所 >` OR- <br/> `azure vm イメージの一覧の sku の [オプション] < 場所 >`
 VM イメージの表示| `azure vm イメージの表示 [オプション]`| 使用できません。
 VM リソースの使用量の取得| 使用できません。| `azure vm の一覧-使用状況 [オプション] < 場所 >`
 使用可能なすべての VM サイズの取得| 使用できません。| `azure vm のサイズを [オプション]`


## 次のステップ

* Azure CLI を使用して、リソース マネージャーのリソースを使用する方法の詳細を参照してください。 [リソース マネージャーで Azure コマンド ライン インターフェイスを使用して](xplat-cli-azure-resource-manager.md) と [ベース アクセス制御 Azure コマンド ライン インターフェイスによる](../role-based-access-control-xplat-cli-install.md)します。
* CLI コマンドの他の例を参照してください [Azure サービス管理で Azure コマンド ライン インターフェイスを使用して](../virtual-machines-command-line-tools.md) と。
[Azure リソース マネージャーで Azure CLI を使用して](azure-cli-arm-commands.md)します。





