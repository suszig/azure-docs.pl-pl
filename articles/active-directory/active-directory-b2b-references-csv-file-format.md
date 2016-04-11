<properties
   pageTitle="Azure Active Directory B2B コラボレーション プレビューの CSV ファイルの形式 | Microsoft Azure"
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

# Azure Active Directory (Azure AD) B2B コラボレーション プレビュー用の CSV ファイルの形式

Azure AD B2B コラボレーションのプレビュー バージョンでは、パートナー ユーザーの情報が指定されている CSV ファイルを、Azure AD ポータルを介してアップロードする必要があります。 CSV ファイルには、以下の必須ラベルと、必要に応じてオプションのフィールドが含まれている必要があります。 サンプルの CSV ファイル (下記) を変更して使用できますが、1 行目のラベルのスペルと列の順序は変更しないでください。

>[AZURE.NOTE] (電子メール、DisplayName...) などのラベルの最初の行が正常に解析する CSV ファイルに必要です。 スペルは、以下で指定したフィールドと一致している必要があります。 これらのラベルによって、その下にある内容が識別されます。 必須ではないオプションのフィールドのラベルは、CSV ファイルから削除できます。 列全体を空のままにしてもかまいません。

## 必要なフィールド: <br/>
**電子メール:** 招待されたユーザーの電子メール アドレス。 <br/>
**DisplayName:** 招待されたユーザー (通常は、姓と名) の表示名。<br/>
**InviteContactUsUrl:** 、招待されたユーザーが組織にお問い合わせする場合に、電子メールの招待に含める URL です。<br/>

## 省略可能なフィールド: <br/>
**InviteAppID:**  の電子メールへの招待と同意のページのブランドを使用するアプリケーションの ID。<br/>
**InviteAppResources:** Appid を企業のアプリケーションでユーザーの割り当て。 呼び出して Appid は PowerShell で取得可能 `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InviteGroupResources:** にユーザーを追加するグループの Objectid。 呼び出して Objectid は PowerShell で取得可能 `Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteReplyURL:** への招待の承認後に招待されたユーザーを特定する URL。 会社固有の URL があります (たとえば [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/))。 このオプション フィールドを指定しないと、招待されたユーザーはアプリ アクセス パネルに送られ、そこから選択されている会社のアプリに移動できますます。 アプリケーション アクセス パネルの URL の形式は、  `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`です。<br/>
**言語:** 指定されていない場合、既定値として英語が招待の電子メールおよび引き換えのエクスペリエンスを言語です。 その他の 10 には、コードは、言語がサポートされています。<br/>
1. de: ドイツ語<br/>
2. es: スペイン語<br/>
3. fr: フランス語<br/>
4. その: イタリア語<br/>
5. ja: 日本語<br/>
6. ko: 韓国語<br/>
7. PT-BR: ポルトガル語 (ブラジル)<br/>
8. ru: ロシア語<br/>
9. zh HANS: 簡体字中国語<br/>
10. それ: 繁体字中国語<br/>

## サンプル CSV ファイル
CSV ファイルのサンプルを次に示します。これは、用途に合わせて変更できます。

>[AZURE.NOTE] コピーし、メモ帳に貼り付けて '.csv' というファイル拡張子を付けて保存します。 その後 Excel で編集します。 1 行目がラベルの表形式です。

>[AZURE.NOTE] Excel で、ラベルを指定し、その下にある列を設定する省略可能なフィールドを追加します。

```
Email,DisplayName,InviteAppID,InviteReplyUrl,InviteAppResources,InviteGroupResources,InviteContactUsUrl
wharp@contoso.com,Walter Harp,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,http://azure.microsoft.com/services/active-directory/,,,http://azure.microsoft.com/services/active-directory/
jsmith@contoso.com,Jeff Smith,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,http://azure.microsoft.com/services/active-directory/,,,http://azure.microsoft.com/services/active-directory/
bsmith@contoso.com,Ben Smith,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,http://azure.microsoft.com/services/active-directory/,,,http://azure.microsoft.com/services/active-directory/
```

## 関連記事:
Azure B2B コラボレーションに関する他の記事を参照してください。

- [Azure AD B2B コラボレーションとは](active-directory-b2b-what-is-azure-ad-b2b.md)
- [動作のしくみ](active-directory-b2b-how-it-works.md)
- [詳細なチュートリアル](active-directory-b2b-detailed-walkthrough.md)
- [外部ユーザー トークンの形式](active-directory-b2b-references-external-user-token-format.md)
- [外部ユーザー オブジェクト属性の変更](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [現在のプレビューの制限事項](active-directory-b2b-current-preview-limitations.md)

