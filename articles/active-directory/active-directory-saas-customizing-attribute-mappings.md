<properties
    pageTitle="属性マッピングのカスタマイズ | Microsoft Azure"
    description="Azure Active Directory における SaaS アプリの属性マッピングとは何かと、この属性マッピングをビジネス ニーズに合わせて変更する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2015"
    ms.author="markusvi"/>


# 属性マッピングのカスタマイズ


Microsoft Azure AD では、Salesforce、Google Apps、およびその他のサードパーティの SaaS アプリケーションへのユーザー プロビジョニングのサポートを提供します。 サード パーティの SaaS アプリケーションでユーザー プロビジョニングを有効にした場合、Microsoft Azure 管理ポータルでは「属性マッピング」と呼ばれる構成の形でその属性値を管理します。

Azure AD ユーザー オブジェクトと各 SaaS アプリのユーザー オブジェクトの間には、構成済みの一連の属性マッピングが存在します。 アプリによっては、グループや連絡先といった他のタイプのオブジェクトを管理するものもあります。 <br> 
 ビジネス ニーズに応じて、既定の属性マッピングをカスタマイズできます。 つまり、既存の属性マッピングを変更、削除したり、新規の属性マッピングを作成したりすることができます。

Azure AD ポータルでこの機能にアクセスするには、SaaS アプリケーションのツールバーで [属性] をクリックします。

> [AZURE.NOTE]  **属性** リンクは、ユーザー プロビジョニングの SaaS アプリケーションに対して有効にした場合のみです。 


![Salesforce][1] 


ツールバーで [属性] をクリックすると、SaaS アプリケーションで現在構成されているマッピングのリストが表示されます。

次のスクリーンショットはその例です。



![Salesforce][2]  


上記の例であることがわかります、 **firstName** Salesforce での管理オブジェクトの属性が設定されますが、 **givenName** のリンクされた値 Azure AD オブジェクトです。

属性マッピングをカスタマイズする場合、またはカスタマイズした設定を既定の構成に戻す場合は、アプリケーション下部のツールバーで関連するボタンをクリックします。


![Salesforce][3]  


SaaS アプリケーションが正常に機能するために必要となる属性マッピングがあります。 
 関連する属性マッピングを持つ属性の表で **はい** 値、 **ために必要な** 属性です。 属性マッピングが必須の場合は削除できません。 この場合、 **削除** 機能は使用できません。

既存の属性マッピングを変更するには、マッピングを選択し、順にクリックして **編集**します。 すると、選択した属性マッピングを変更できるダイアログ ページが表示されます。


![属性マッピングの編集][4]  



## 属性マッピングの種類について


属性マッピングでは、サードパーティの SaaS アプリケーションに属性を設定する方法を管理します。 
次の 4 つの異なるマッピングの種類がサポートされます。

- **直接** – ターゲットの属性が Azure AD でのリンク オブジェクトの属性の値に設定します。


- **定数** -ターゲット属性が指定した文字列が設定されます。


- **式** -スクリプトのような式の結果に基づいてターゲットの属性を設定します。 
詳細については、次を参照してください。 [Azure Active Directory における属性マッピングの式の記述に](active-directory-saas-writing-expressions-for-attribute-mappings.md)します。


- **[なし]** -ターゲット属性のまま変更されていません。 ただし、ターゲットの属性が空の場合は、指定した既定値が設定されます。



カスタム属性マッピングの概念がサポートこれら 4 つの基本的な属性マッピングの種類に加えて、 **既定** 値の割り当て。 既定値の割り当てでは、Azure AD にもターゲット オブジェクトにも値がない場合にも、ターゲットの属性にかならず値を設定します。

Microsoft Azure AD では、同期プロセスの非常に効率的な実装を提供します。 
 初期化された環境では、更新が必要なオブジェクトのみが同期サイクル中に処理されます。 
 属性マッピングの更新は、同期サイクルのパフォーマンスに影響を与えます。 
 これは、属性マッピングの構成を更新するには、すべての管理オブジェクトを再評価する必要があるためです。 
 このため、推奨されるベスト プラクティスとして、属性マッピングに対する連続的な変更の回数は最小限に抑えてください。



[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png

