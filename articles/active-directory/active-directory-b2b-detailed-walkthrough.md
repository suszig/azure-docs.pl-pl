<properties
   pageTitle="Azure Active Directory B2B コラボレーション プレビューの使用方法に関する詳細なチュートリアル | Microsoft Azure"
   description="Azure Active Directory B2B は、会社のアプリケーションにビジネス パートナーが選択的にアクセスできるようにすることで、会社間のリレーションシップをサポートします"
   services="active-directory"
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="10/27/2015"
   ms.author="viviali"/>

# Azure Active Directory (Azure AD) B2B コラボレーション プレビューの詳細なチュートリアル
このチュートリアルでは、Azure AD B2B コラボレーションの使用方法について説明します。 Contoso の IT 管理者は、パートナー企業 3 社の従業員とアプリケーションを共有することを検討しています。 パートナー企業側が Azure AD を導入する必要はありません。

- Simple Partner Org の Alice
- Medium Partner Org の Bob は、複数のアプリに対するアクセス権が必要です
- Complex Partner Org の Carol は、複数のアプリに対するアクセス権だけでなく、Contoso 社内のグループのメンバーシップも必要です

パートナー ユーザーに招待を送信した後に、Azure AD でそれらのユーザーを構成し、Azure ポータルを使用してアプリに対するアクセス権とグループのメンバーシップを付与することができます。 まず Alice を追加してみましょう。

## Alice を Contoso のディレクトリに追加する
1. ようにのみ Alice の設定のヘッダーを含む .csv ファイルを作成 **電子メール**, 、**DisplayName**, 、および **InviteContactUsUrl**します。 **DisplayName** は、招待状に表示される名前とも Contoso の Azure AD ディレクトリ内に表示される名前です。 **InviteContactUsUrl** Alice contoso 社に連絡する方法です。 次の例では、Contoso 社の LinkedIn プロフィールを指定しています。 .csv ファイルの 1 行目の見出しは、この例と同じスペル、同じ順序で作成する点が重要です。 後述の「CSV 形式を参照してください。  
![Alice の CSV ファイルの例](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Azure ポータルで、Contoso 社のディレクトリにユーザーを追加します ([Active Directory] > [Contoso] > [ユーザー] > [ユーザーの追加])。 [ユーザーの種類] ドロップダウンで [パートナー会社のユーザー] を選択します。 .csv ファイルをアップロードします。 アップロードする前に、.csv ファイルを閉じたことを確認します。  
![Alice の CSV ファイルのアップロード](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice が Contoso の Azure AD ディレクトリ内の外部のユーザーとして表されます。  
![Alice が Azure AD に表示されています](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice の観点からは、次のメール彼女を受け取ります。  
![Alice の招待電子メール](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice がリンクをクリックすると、招待を受け、会社の資格情報を使用してサインインするように求められます。 Alice が Azure AD ディレクトリにない場合は、Alice はサインアップするように求められます。  
![Alice の招待後にサインアップします](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice は、アプリへのアクセスを与えまで空アプリケーションのアクセス パネルにリダイレクトされます。  
![Alice のアクセス パネル](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

これが B2B コラボレーションの最も単純な形式です。 Alice は Contoso 社の Azure AD ディレクトリのユーザーになったので、Azure ポータルでアプリケーションやグループに対するアクセス権を Alice に付与できます。 次に Bob を追加してみましょう。Bob は、アプリケーション Moodle と Salesforce に対するアクセス権が必要です。

## Bob を Contoso 社のディレクトリに追加し、アプリへのアクセスを許可する
1. Azure AD モジュールがインストールされている Windows PowerShell を使用して、Moodle と Salesforce のアプリケーション ID を検索します。 コマンドレットを使用して Id を取得することができます: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` Contoso とその AppPrincialIds で利用可能なすべてのアプリケーションの一覧が表示されます。  
![Bob の Id を取得します。](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Bob の電子メール、DisplayName を設定する .csv ファイルを作成 **InviteAppID**, 、**InviteAppResources**, 、InviteContactUsUrl とします。 **InviteAppResources** Moodle AppPrincipalIds 伴って、スペースで区切って、PowerShell から Salesforce が検出されます。 下図の下線は、上の PowerShell のスクリーンショットに示されている緑色と青色の枠の ID に対応しています。 **InviteAppId** は、同じ AppPrincipalId の Moodle、電子メールをブランド化し、サインイン ページが表示されます。  
![Bob の CSV ファイルの例](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Alice の場合と同様に、Azure ポータルで .csv ファイルをアップロードします。 Bob は Contoso 社の Azure AD ディレクトリの外部ユーザーになりました。

4. Bob には、次のメールを受け取ります。  
![Invitation email for Bob](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob はリンクをクリックし、招待を受けるように求められます。 彼をサインインした後は、彼はアクセス パネルに送信し、Moodle と Salesforce を既に使用できます。  
![Access Panel for Bob](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

次に Carol を追加しましょう。Carol は、アプリケーションに対するアクセス権だけでなく、Contoso 社のディレクトリ内にあるグループのメンバーシップも必要です。

## Carol を Contoso 社のディレクトリに追加し、アプリケーションに対するアクセス権を付与し、グループのメンバーシップを与える

1. Azure AD モジュールがインストールされている Windows PowerShell を使用して、Contoso 社内のアプリケーション ID とグループ ID を検索します。
 - Bob の場合と同様に、`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` コマンドレットを使用して AppPrincipalId を取得します。
 - `Get-MsolGroup | fl DisplayName, ObjectId` コマンドレットを使用して、グループの ObjectId を取得します。その結果、Contoso 社社内のすべてのグループとその ObjectId の一覧が表示されます。 グループ Id は、Azure ポータルでグループのプロパティ] タブで、オブジェクト id も取得できます。  
![Carol の Id とグループを取得します。](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. .Csv ファイル、Carol の設定を作成、電子メール、DisplayName、InviteAppID InviteAppResources、 **InviteGroupResources**, 、InviteContactUsUrl とします。 **InviteGroupResources** MyGroup1、およびスペースで区切って、外部グループの Objectid によって設定されます。  
![Carol の CSV ファイルの例](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Azure ポータルで .csv ファイルをアップロードします。

4. Carol は、Contoso のディレクトリ内のユーザーでありも MyGroup1 と外部参照では、グループのメンバーとして、Azure ポータルで表示します。  
![Azure AD のグループに Carol が表示されます](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol は、招待を受けるリンクが記載された電子メールを受信します。 サインイン後は、アプリ アクセス パネルにリダイレクトされ、Moodle と Salesforce へのアクセス権が付与されます。  

Azure AD B2B コラボレーションのパートナー企業のユーザーを追加する手順は以上です。 このチュートリアルは、追加の Alice、Bob、および 3 つの独立した .csv ファイルに Carol を示しましたが、実際に同時に追加できるわかりやすくするための 1 つの .csv ファイルにします。  
![Alice、Bob、Carol の CSV ファイルの例](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## 関連記事:
Azure AD B2B コラボレーションに関する他の記事を参照してください。

- [Azure AD B2B コラボレーションとは](active-directory-b2b-what-is-azure-ad-b2b.md)
- [動作のしくみ](active-directory-b2b-how-it-works.md)
- [CSV ファイル形式リファレンス](active-directory-b2b-references-csv-file-format.md)
- [外部ユーザー トークンの形式](active-directory-b2b-references-external-user-token-format.md)
- [外部ユーザー オブジェクト属性の変更](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [現在のプレビューの制限事項](active-directory-b2b-current-preview-limitations.md)

