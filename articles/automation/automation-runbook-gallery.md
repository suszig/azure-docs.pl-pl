<properties 
    pageTitle="Azure Automation 用の Runbook ギャラリーとモジュール ギャラリー | Microsoft Azure"
    description="Microsoft やコミュニティからの Runbook とモジュールを Azure Automation 環境にインストールして使用できます。この記事では、これらのリソースにアクセスしたり、自分の Runbook をギャラリーに投稿したりする方法について説明します。"
    services="automation"
    documentationCenter=""
    authors="bwren"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/23/2015"
    ms.author="bwren" />



# Azure Automation 用の Runbook ギャラリーとモジュール ギャラリー

Azure Automation で独自の Runbook およびモジュールを作成するのではなく、マイクロソフトやコミュニティによって既に作成されているさまざまなソリューションにアクセスできます。 これらのソリューションを変更しないでそのまま使用することも、特定の要件に合うように編集することもできます。

Runbook を取得できます、 [Runbook ギャラリー](#runbooks-in-runbook-gallery) とモジュールから、 [PowerShell Gallery](#modules-in-powerShell-gallery)します。 開発したソリューションを共有することにより、コミュニティに貢献することもできます。

## Runbook ギャラリーでの Runbook

[Runbook ギャラリー](http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=RootCategory&f[0].Value=WindowsAzure&f[1].Type=SubCategory&f[1].Value=WindowsAzure_automation&f[1].Text=Automation) Microsoft と Azure Automation にインポートできるコミュニティからのさまざまな runbook を提供します。ホストされているギャラリーから runbook をダウンロードすることができます、 [TechNet スクリプト センター](http://gallery.technet.microsoft.com/), 、または Azure ポータルと Azure プレビュー ポータルからギャラリーから runbook を直接インポートできます。

Runbook ギャラリーから直接インポートするのは、Azure ポータルまたは Azure プレビュー ポータルを使用した場合にのみ可能です。 Windows PowerShell を使用してこの機能を実行することはできません。
>[AZURE.NOTE] Runbook ギャラリーから取得した Runbook は、内容を検証し、運用環境でインストールおよび実行するときは細心の注意を払う必要があります。

### Azure ポータルで Runbook ギャラリーから Runbook をインポートするには

1. Microsoft Azure 管理ポータルで、**[新規]**、**[App Services]**、**[Automation]**、**[Runbook]**、**[ギャラリーから]** の順にクリックします。
2. カテゴリを選択して関連する Runbook を表示し、Runbook を選択して詳細を表示します。目的の runbook を選択すると、右矢印ボタンをクリックします。<br>
![Runbook ギャラリー](media/automation-runbook-gallery/runbook-gallery.png)
3. Runbook の内容を確認し、説明の要件に注意します。 終了したら右矢印ボタンをクリックします。
4. Runbook の詳細を入力し、チェック マーク ボタンをクリックします。 Runbook の名前は既に設定されています。
5. Runbook が Automation アカウントの **[Runbook]** タブに表示されます。

### Azure プレビュー ポータルで Runbook ギャラリーから Runbook をインポートするには

1. Azure プレビュー ポータルで、Automation アカウントを開きます。
2. **[Runbook]** タイルをクリックして、Runbook の一覧を開きます。
3. クリックして **ギャラリーの参照** ] ボタンをクリックします。 <br>
![[ギャラリーの参照] ボタン](media/automation-runbook-gallery/browse-gallery-button.png)
4. 選択すると、その詳細を表示して、ギャラリー項目を見つけます。   <br>
![ギャラリーの参照](media/automation-runbook-gallery/browse-gallery.png)
4. をクリックして **ビュー ソース プロジェクト** で項目を表示する、 [TechNet スクリプト センター](http://gallery.technet.microsoft.com/)します。
5. アイテムをインポートするには、クリックして、詳細を表示するために] をクリックして、 **インポート** ] ボタンをクリックします。<br>
![[インポート] ボタン](media/automation-runbook-gallery/gallery-item-detail.png)
6. 必要に応じて Runbook の名前を変更し、**[OK]** をクリックして Runbook をインポートします。
5. Runbook が Automation アカウントの **[Runbook]** タブに表示されます。


### Runbook ギャラリーへの Runbook の追加

他のユーザーにも役に立つと思われる Runbook を Runbook ギャラリーに追加してください。 Runbook を追加することができます [スクリプト センターへのアップロード](http://gallery.technet.microsoft.com/site/upload) 、次の詳細を考慮します。

- 指定する必要があります *Windows Azure* の **カテゴリ** と *オートメーション* の **Subcategory** ウィザードで表示する runbook にします。

- アップロードは、単一の .ps1 ファイルまたは .graphrunbook ファイルである必要があります。 Runbook でモジュール、子 Runbook、または資産が必要な場合は、送信の説明および Runbook のコメント セクションで列記する必要があります。 複数の Runbook を必要とするソリューションがある場合は、各 Runbook を個別にアップロードし、それぞれの説明に関連する Runbook の名前を列記します。 同じカテゴリに表示されるように同じタグを使用していることを確認します。 ソリューションが動作するためには他の Runbook が必要であることをユーザーが知るには、説明を読む必要があります。

- **[コード セクションの挿入]** アイコンを使用して、PowerShell または PowerShell ワークフローのコード スニペットを説明に挿入します。

- アップロードの概要が Runbook ギャラリーの結果に表示されるので、ユーザーが Runbook の機能を特定するのに役立つ詳細な情報を提供する必要があります。

- 次のタグから 1 ～ 3 個をアップロードに割り当てる必要があります。 ウィザードでは、Runbook はそのタグに一致するカテゴリの下に表示されます。 この一覧にないタグは、ウィザードによって無視されます。 一致するタグを指定しないと、Runbook はその他のカテゴリに表示されます。

 - Backup
 - 容量管理
 - Change Control
 - コンプライアンス
 - Dev / Test Environments
 - 障害復旧
 - Monitoring
 - Patching
 - プロビジョニング
 - Remediation
 - VM Lifecycle Management

- Automation はギャラリーを 1 時間に 1 回更新するので、投稿物がすぐに表示されないことがあります。 1 時間後に、ギャラリーに runbook が表示されない場合は、要件を確認、 [Runbook ギャラリーに Runbook を追加する](#AddRunbook) セクションです。

## PowerShell ギャラリーのモジュール

PowerShell モジュールには、runbook で使用できるコマンドレットが含まれており、Azure Automation にインストールできる既存のモジュールがで利用できる、 [PowerShell Gallery](http://www.powershellgallery.com)します。 このギャラリーは Azure プレビュー ポータルから起動でき、Azure Automation に直接インストールするか、ダウンロードして手動でインストールすることができます。 Azure ポータルからは、直接モジュールをインストールすることはできませんが、他のモジュールと同じように、ダウンロードしてインストールすることはできます。

### Azure プレビュー ポータルで PowerShell ギャラリーからモジュールをインポートするには

1. Azure プレビュー ポータルで、Automation アカウントを開きます。
2. **[アセット]** タイルをクリックして、資産の一覧を開きます。
3. **[モジュール]** タイルをクリックして、モジュールの一覧を開きます。
3. をクリックして、 **PowerShell ギャラリー** を別のブラウザー ウィンドウで、PowerShell ギャラリーを起動するボタンをクリックします。 <br>
![PowerShell ギャラリー](media/automation-runbook-gallery/powershell-gallery-button.png)
4. クリックして、 **モジュール** ] メニューの [利用可能なモジュールの一覧にアクセスします。<br>
![[PowerShell ギャラリー] ボタン](media/automation-runbook-gallery/powershell-gallery.png)
4. 目的のモジュールを探し、選択して詳細を表示します。
5. Azure Automation に直接モジュールをインストールするにはクリックして、 **Azure Automation への配置]** ] ボタンをクリックします。<br>
![[PowerShell ギャラリー] ボタン](media/automation-runbook-gallery/powershell-gallery-detail.png)
6. Azure プレビュー ポータルの **[カスタム デプロイメント]** ウィンドウに戻ります。 **[新規または既存の Automation アカウント]** および **[Automation アカウント名]** で、モジュールをインストールするかどうかを指定します。 既存のアカウントを使用する場合、**[Automation アカウントの場所]** は無視されます。
7. **[リソース グループ]** を選択し、既存のリソース グループを指定するか、モジュール用に新しいリソース グループを作成します。
6. **[法的条項]** を選択し、**[購入]** をクリックします。 このボタンの名前はこうなっていますが、実際にはモジュールをインストールしても課金されません。
7. **[作成]** をクリックしてモジュールをインポートします。 各アクティビティを抽出する必要があるため、数分かかる場合があります。
8. モジュールがデプロイされていることを示す通知、および完了したことを示す通知を受け取ります。


## Runbook またはモジュールの要求

要求を送信する [User Voice](http://feedback.azure.com/forums/246290-azure-automation)します。Runbook の作成を支援する、または PowerShell について質問があるを場合に質問を投稿、 [フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)します。

## 関連記事:

- [作成または Azure Automation で runbook をインポートします。](automation-creating-importing-runbook.md)
- [PowerShell ワークフローについて](automation-powershell-workflow.md)




