<properties
  pageTitle="Azure IoT Suite と Azure Active Directory | Microsoft Azure"
  description="Azure IoT Suite が Azure Active Directory を使用してアクセス許可を管理する方法を説明します。"
  services=""
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="na"
  ms.devlang="na"
  ms.topic="hero-article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="11/17/2015"
  ms.author="araguila"/>
  
# azureiotsuite.com サイトでのアクセス許可

## サインイン時に行われること

サインインすると最初に [azureiotsuite.com][lnk-azureiotsuite], 、サイトが、現在選択されている Azure Active Directory (AAD) テナントと Azure サブスクリプションに基づいているアクセス許可レベルを決定します。

1.  サイトは最初に、ログイン ユーザー名の隣に表示されるテナントの一覧を設定するため、Azure からユーザーが属している AAD テナントを取得します。 この時点で、サイトは、一度に 1 つのテナントのユーザー トークンしか取得できません。 その結果、ユーザーが右上隅のドロップダウンを使用して異なるテナントに切り替えると、サイトはユーザーをそのテナントに再ログインさせて、そのテナントのトークンを取得します。

2.  次に、サイトは、ユーザーが選択されているテナントと関連付けているサブスクリプションを Azure から検索します。 ユーザーが新しい構成済みソリューションを作成するときに、使用できるサブスクリプションが表示されます。

3.  最後に、サイトはサブスクリプション内および構成済みソリューションというタグが付いているリソース グループ内のすべてのリソースを取得して、ホーム ページのタイトルを設定します。

以下のセクションでは、構成済みソリューションへのアクセスを制御するロールについて説明します。

## AAD のロール

AAD のロールは、構成済みソリューションをプロビジョニングし、構成済みソリューション内のユーザーを管理する権限を制御します。

AAD での詳細については、管理者の役割を見つけることができます [Azure AD で管理者ロールの割り当て][lnk-aad-admin], 、この記事に、主に焦点を当てていますが、 **グローバル管理者** と **ドメイン ユーザーのメンバー** ロールは構成済みのソリューションで使用します。

**グローバル管理者:** AAD テナントごとに多くのグローバル管理者であることができます。 AAD テナントを作成したユーザーは、既定でそのテナントのグローバル管理者になります。 グローバル管理者が構成済みのソリューションを提供することができが割り当てられている、 **管理者** 内の AAD テナントでアプリケーションのロール。 ただし、同じ AAD テナントで別のユーザーは、アプリケーションを作成する場合、グローバル管理者が権既定のロールは **暗黙的な READ ONLY**します。 グローバル管理者を使用するアプリケーション ロールを割り当てることができます、 [Azure クラシック ポータル][lnk-classic-portal]します。

**ドメイン ユーザー/メンバー:** AAD テナントあたりのドメイン ユーザーが/メンバー多くが存在することができます。 ドメイン ユーザーが、構成済みのソリューションをプロビジョニング、 [azureiotsuite.com][lnk-azureiotsuite] サイトです。 既定のロールをプロビジョニングするアプリケーションが付与されます **管理者**します。 Build.cmd スクリプトを使用してアプリケーションを作成することができます、 [iot ソリューションの azure][lnk-github-repo] リポジトリが既定のロールが与えられる **暗黙的な読み取り専用**, ロールを割り当てるためのアクセス許可があるないように、します。 割り当てられている AAD テナントで別のユーザーは、アプリケーションを作成する場合、 **暗黙的な読み取り専用** 既定では、そのアプリケーション ロールです。 アプリケーションのロールを割り当てることはできません。そのため、自分でプロビジョニングしたアプリケーションであっても、ユーザーまたはユーザーのロールを追加することはできません。

**ゲスト ユーザー/ゲスト:** AAD テナント別ゲスト ユーザーが/ゲスト多くがあります。 ゲスト ユーザーの AAD テナントでの権限セットは制限されています。 したがって、ゲスト ユーザーは AAD テナントで構成済みソリューションをプロビジョニングできません。

詳細については、次のリソースを参照してください。

- [Azure AD でのユーザーの作成または編集][lnk-create-edit-users]
- [AAD でのアプリ ロールの割り当て][lnk-assign-app-roles]

## Azure サブスクリプション管理者ロール

Azure 管理者ロールは、Azure サブスクリプションを AD テナントにマップする機能を制御します。

Azure 共同管理者、サービス管理者アカウントの管理者の役割の記事で詳しく調べることができます [追加または Azure 共同管理者、サービス管理者アカウントの管理者を変更する方法][lnk-admin-roles]します。

## アプリケーション ロール

アプリケーション ロールは、構成済みソリューション内のデバイスへのアクセスを制御します。

構成済みソリューションをプロビジョニングすると作成されるアプリケーションには、2 つの定義済みロールと 1 つの暗黙ロールが定義されます。

-   **[管理者]:** フル コントロールを追加、管理、およびデバイスを削除するには

-   **読み取り専用:** がデバイスを表示する機能

-   **暗黙的な READ ONLY:** は読み取り専用で同じこれは、AAD テナントのすべてのユーザーに付与します。 これは、開発中の利便性のためのものです。 この役割を削除するには変更することにより、 [RolePermissions.cs][lnk-resource-cs] ソース ファイルです。

### アプリケーション ロールの変更

ユーザーのロールを変更できるのは、AAD グローバル管理者だけです。

1. 移動して、 [Azure クラシック ポータル][lnk-classic-portal]

2. Select **Active Directory**

3. AAD テナントの名前をクリックします。

4. クリックして **アプリケーション**します。

5. 一覧で、アプリケーションが表示されない場合は、スイッチ、 **表示** 下にドロップ **私の会社が所有するアプリケーション** チェック マークをクリックします。

6. 構成済みソリューション名と一致するアプリケーションの名前をクリックします。

7. クリックして **ユーザー**します。

8. ロールを切り替えるユーザーを選択します。

9. 割り当てボタンをクリックして割り当てるロールをクリックし、チェック マークをクリックします。

## FAQ

### サービス管理者が自分のサブスクリプションと特定の AAD テナントの間のディレクトリ マッピングを変更する必要がある場合は、 どうすればよいですか

1. 移動して、 [Azure クラシック ポータル][lnk-classic-portal], 、クリックして **設定** 左側にあるサービスの一覧でします。

2. ディレクトリ マッピングを変更するサブスクリプションを選択します。

3. クリックして **ディレクトリを編集**します。

4. 選択、 **ディレクトリ** ドロップダウン リストで使用したいです。 右向きの矢印をクリックします。

5. ディレクトリのマッピングと影響を受ける共同管理者を確認します。 別のディレクトリから移動する場合、元のディレクトリのすべての共同管理者が削除されることに注意してください。

### AAD テナントのドメイン ユーザー/メンバーが構成済みソリューションを作成した場合、 アプリケーションに対するロールを割り当ててもらうにはどうすればよいですか

グローバル管理者に依頼して AAD テナントでのグローバル管理者を割り当ててもらい、自分でユーザーにロールを割り当てるアクセス許可を取得するか、またはグローバル管理者にロールの割り当てを依頼します。 構成済みソリューションをデプロイした AAD テナントを変更したい場合は、次の質問を参照してください。

### リモート監視構成済みソリューションおよびアプリケーションが割り当てられている AAD テナントを切り替えるにはどうすればよいですか

クラウドの展開を行うことができます <https://github.com/Azure/azure-iot-remote-monitoring> を新しく作成した AAD テナントを再配置します。 新しい AAD テナントを作成したユーザーは既定でグローバル管理者になるので、ユーザーを追加し、ユーザーにロールを割り当てることができます。

1. 新しい AAD ディレクトリを作成、 [Azure 管理ポータル][lnk-classic-portal]します。

2. 移動して <https://github.com/Azure/azure-iot-remote-monitoring>します。 クラウドの展開の詳細については、次を参照してください。 [クラウド デプロイ][lnk-wiki-clouddeployment]します。

3. `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` を実行します (例: `build.cmd cloud debug myRMSolution`)

4. 求められたら、設定、 **tenantid** 、以前のテナントではなく、新しく作成したテナントにします。


### 組織アカウントでログインするときにサービス管理者または共同管理者を変更するにはどうすればよいですか

サポートの記事を参照してください [を変更するサービスの管理者/共同管理者組み合わせますアカウントを使用してログインしたときに][lnk-service-admins]します。

### 次のエラーが表示されるのはなぜですか。 「お使いのアカウントにはソリューションを作成する適切なアクセス許可がありません。 アカウント管理者に確認するか、別のアカウントを使用してください。」

次の図を見てください。

![][img-flowchart]

**Azure サブスクリプションがあるときにこのエラーを発生する理由ですか。***「構成済みソリューションを作成するには Azure サブスクリプションが必要です。数分で無料試用版のアカウントを作成することができます。」*

Azure サブスクリプションが確かにある場合は、サブスクリプションのテナント マッピングを調べ、ドロップダウンで正しいテナントが選択されていることを確認してください。 目的のテナントが正しいことを確認できた場合は、上の図に従って、サブスクリプションとこの AAD テナントのマッピングを確認してください。

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-github-repo]: https://github.com/Azure/azure-iot-solution
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://github.com/Azure/azure-iot-remote-monitoring/wiki/Manually-setting-up-roles-and-assigning-permissions-in-Azure-Active-Directory-(AAD)#assigning-users-to-the-roles
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-wiki-clouddeployment]: https://github.com/Azure/azure-iot-remote-monitoring/wiki/Cloud-deployment

