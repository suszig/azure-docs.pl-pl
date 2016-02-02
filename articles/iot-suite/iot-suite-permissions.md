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

サインインすると最初に [azureiotsuite.com ][lnk-azureiotsuite], 、サイトが、現在選択されている Azure Active Directory (AAD) テナントと Azure サブスクリプションに基づいているアクセス許可レベルを決定します。

1.  サイトは最初に、ログイン ユーザー名の隣に表示されるテナントの一覧を設定するため、Azure からユーザーが属している AAD テナントを取得します。 この時点で、サイトは、一度に 1 つのテナントのユーザー トークンしか取得できません。 その結果、ユーザーが右上隅のドロップダウンを使用して異なるテナントに切り替えると、サイトはユーザーをそのテナントに再ログインさせて、そのテナントのトークンを取得します。

2.  次に、サイトは、ユーザーが選択されているテナントと関連付けているサブスクリプションを Azure から検索します。 ユーザーが新しい構成済みソリューションを作成するときに、使用できるサブスクリプションが表示されます。

3.  最後に、サイトはサブスクリプション内および構成済みソリューションというタグが付いているリソース グループ内のすべてのリソースを取得して、ホーム ページのタイトルを設定します。

以下のセクションでは、構成済みソリューションへのアクセスを制御するロールについて説明します。

## AAD のロール

AAD のロールは、構成済みソリューションをプロビジョニングし、構成済みソリューション内のユーザーを管理する権限を制御します。

AAD での詳細については、管理者の役割を見つけることができます [[lnk aad 管理者] の Azure AD で管理者ロールの割り当て][lnk-aad-admin], 、この記事に、主に焦点を当てていますが、 **グローバル管理者** と **ドメイン ユーザーのメンバー** ロールは構成済みのソリューションで使用します。

**グローバル管理者:** AAD テナントごとに多くのグローバル管理者であることができます。 AAD テナントを作成したユーザーは、既定でそのテナントのグローバル管理者になります。 グローバル管理者は、構成済みソリューションをプロビジョニングでき、AAD テナントの内部にあるアプリケーションに対する **ADMINISTRATOR** ロールを割り当てられます。 ただし、同じ AAD テナントの別のユーザーがアプリケーションを作成した場合は、グローバル管理者に付与される既定のロールは **IMPLICIT READ ONLY** です。 グローバル管理者を使用するアプリケーション ロールを割り当てることができます、 [Azure クラシック ポータル ][lnk-classic-portal]します。

**ドメイン ユーザーのメンバー:** AAD テナントあたりのドメイン ユーザーが/メンバー多くが存在することができます。 ドメイン ユーザーが、構成済みのソリューションをプロビジョニング、 [azureiotsuite.com ][lnk-azureiotsuite] サイトです。 既定のロールをプロビジョニングするアプリケーションが付与されます **管理者**します。 Build.cmd スクリプトを使用してアプリケーションを作成することができます、 [azure iot ソリューション ][lnk-github-repo] リポジトリが既定のロールが与えられる **暗黙的な読み取り専用**, ロールを割り当てるためのアクセス許可があるないように、します。 AAD テナントの別のユーザーが作成したアプリケーションに対して既定で割り当てられるロールは **IMPLICIT READONLY** です。 アプリケーションのロールを割り当てることはできません。そのため、自分でプロビジョニングしたアプリケーションであっても、ユーザーまたはユーザーのロールを追加することはできません。

**ゲスト ユーザー/ゲスト:** AAD テナント別ゲスト ユーザーが/ゲスト多くがあります。 ゲスト ユーザーの AAD テナントでの権限セットは制限されています。 したがって、ゲスト ユーザーは AAD テナントで構成済みソリューションをプロビジョニングできません。

詳細については、次のリソースを参照してください。

- [作成または Azure AD ][lnk-create-edit-users]
- [AAD ][lnk-assign-app-roles]

## Azure サブスクリプション管理者ロール

Azure 管理者ロールは、Azure サブスクリプションを AD テナントにマップする機能を制御します。

Azure 共同管理者、サービス管理者アカウントの管理者の役割の記事で詳しく調べることができます [追加または Azure 共同管理者、サービス管理者アカウントの管理者 ][lnk-admin-roles]します。

## アプリケーション ロール

アプリケーション ロールは、構成済みソリューション内のデバイスへのアクセスを制御します。

構成済みソリューションをプロビジョニングすると作成されるアプリケーションには、2 つの定義済みロールと 1 つの暗黙ロールが定義されます。

-   **ADMINISTRATOR:** デバイスの追加、管理、削除を完全に制御できます

-   **READ ONLY:** デバイスを表示できます

-   **IMPLICIT READ ONLY:** READ ONLY と同じですが、AAD テナントのすべてのユーザーに付与されます。 これは、開発中の利便性のためのものです。 この役割を削除するには変更することにより、 [RolePermissions.cs ][lnk-resource-cs] ソース ファイルです。

### アプリケーション ロールの変更

ユーザーのロールを変更できるのは、AAD グローバル管理者だけです。

1. に [Azure クラシック ポータル ][lnk-classic-portal]

2. **[Active Directory]** を選択します。

3. AAD テナントの名前をクリックします。

4. **[アプリケーション]** をクリックします。

5. 一覧にアプリケーションが表示されない場合は、**[表示]** ドロップダウンを **[自分の会社が所有するアプリケーション]** に切り替えて、チェック マークをクリックします。

6. 構成済みソリューション名と一致するアプリケーションの名前をクリックします。

7. **[ユーザー]** をクリックします。

8. ロールを切り替えるユーザーを選択します。

9. 割り当てボタンをクリックして割り当てるロールをクリックし、チェック マークをクリックします。

## FAQ

### サービス管理者が自分のサブスクリプションと特定の AAD テナントの間のディレクトリ マッピングを変更する必要がある場合は、どうすればよいですか

1. 移動して、 [Azure クラシック ポータル ][lnk-classic-portal], 、クリックして **設定** 左側にあるサービスの一覧でします。

2. ディレクトリ マッピングを変更するサブスクリプションを選択します。

3. **[ディレクトリの編集]** をクリックします。

4. ドロップダウン リストで使用する**ディレクトリ**を選択します。 右向きの矢印をクリックします。

5. ディレクトリのマッピングと影響を受ける共同管理者を確認します。 別のディレクトリから移動する場合、元のディレクトリのすべての共同管理者が削除されることに注意してください。

### AAD テナントのドメイン ユーザー/メンバーが構成済みソリューションを作成した場合、アプリケーションに対するロールを割り当ててもらうにはどうすればよいですか

グローバル管理者に依頼して AAD テナントでのグローバル管理者を割り当ててもらい、自分でユーザーにロールを割り当てるアクセス許可を取得するか、またはグローバル管理者にロールの割り当てを依頼します。 構成済みソリューションをデプロイした AAD テナントを変更したい場合は、次の質問を参照してください。

### リモート監視構成済みソリューションおよびアプリケーションが割り当てられている AAD テナントを切り替えるにはどうすればよいですか

クラウドの展開を行うことができます <https://github.com/Azure/azure-iot-remote-monitoring> を新しく作成した AAD テナントを再配置します。新しい AAD テナントを作成したユーザーは既定でグローバル管理者になるので、ユーザーを追加し、ユーザーにロールを割り当てることができます。

1. 新しい AAD ディレクトリを作成、 [Azure 管理ポータルの ][lnk-classic-portal]します。

2. Go to <https://github.com/Azure/azure-iot-remote-monitoring>. クラウドの展開の詳細については、次を参照してください。 [クラウド デプロイ ][lnk-wiki-clouddeployment]します。

3. 実行 `build.cmd クラウド [デバッグ | リリース] {以前に展開したリモート監視ソリューションの名前}` (たとえば、 `build.cmd クラウド デバッグ myRMSolution`)

4. プロンプトが表示されたら、古いテナントの代わりに新しく作成したテナントの **tenantid** を設定します。


### 組織アカウントでログインするときにサービス管理者または共同管理者を変更するにはどうすればよいですか

サポートの記事を参照してください [サービス管理者を変更し、組み合わせますを使用するログインとの共同管理者アカウントの ][lnk-service-admins]します。

### 次のエラーが表示されるのはなぜですか。「お使いのアカウントにはソリューションを作成する適切なアクセス許可がありません。アカウント管理者に確認するか、別のアカウントを使用してください。」

次の図を見てください。

![][img-flowchart]

**が発生する理由このエラーは Azure サブスクリプションがあるときですか?** * 事前構成済みのソリューションを作成する Azure サブスクリプションが必要です。 Minutes.* の 2 つだけで無料の試用アカウントを作成することができます。

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

