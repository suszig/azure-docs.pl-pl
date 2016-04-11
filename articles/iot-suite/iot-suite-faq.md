<properties
  pageTitle="Azure IoT Suite に関する FAQ | Microsoft Azure"
  description="IoT Suite に関してよく寄せられる質問です。"
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
   
# よく寄せられる質問

### サブスクリプションには何個の DocumentDB インスタンスをプロビジョニングできますか?

5 個です。 サポート チケットを作成してこの上限を引き上げることができますが、既定では、サブスクリプションごとにプロビジョニングできる DocumentDB インスタンスは 5 個のみです。 その結果、サブスクリプションごとにプロビジョニングできるリモート監視構成済みソリューションも最大で 5 個です。

### サブスクリプションでプロビジョニングできる無料の Bing マップ API の数はいくつですか?

2 個です。 サブスクリプションで作成できる無料の Bing マップ API は 2 個だけです。 リモート監視ソリューションは、既定では無料の Bing マップ API でプロビジョニングされます。 その結果、変更なしでサブスクリプションにプロビジョニングできるリモート監視ソリューションは最大 2 個です。

### Azure ポータルでのリソース グループの削除と、azureiotsuite.com の構成済みソリューションで削除をクリックすることの違いは何ですか?

- 構成済みのソリューションを削除すると [azureiotsuite.com][lnk-azureiotsuite], 、構成済みのソリューションを作成したときにプロビジョニングされていたすべてのリソースを削除する、その他のリソースをリソース グループに追加した場合は、これらも削除します。 

- 内のリソース グループを削除した場合、 [Azure ポータル][lnk-azure-portal], 、そのリソース グループ内のリソースを削除するだけ; で構成済みのソリューションに関連付けられている Azure Active Directory アプリケーションを削除する必要がありますも、 [Azure クラシック ポータル][lnk-classic-portal]します。

### AAD テナントはどうすれば削除できますか?

Eric Golpe ブログの投稿を参照してください [Azure AD テナントを削除するためのチュートリアル][lnk-delete-aad-tennant]します。


[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
