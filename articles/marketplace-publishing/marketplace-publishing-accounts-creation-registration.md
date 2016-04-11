<properties
   pageTitle="発行者アカウントの作成と登録 | Microsoft Azure"
   description="承認のうえで、Azure Marketplace でさまざまなプランの種類を販売できるように Microsoft 販売者アカウントを作成する手順。"
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/06/2015"
   ms.author="hascipio"/>

# Microsoft 販売者アカウントの作成
この記事では、Azure Marketplace 向けの承認済みの Microsoft 販売者になるために必要なアカウントの作成と登録プロセスについて説明します。

## 1.Microsoft アカウントの作成
> [AZURE.WARNING] 発行プロセスを開始するには、Microsoft アカウントを作成する必要があります。 登録して、両方へのサインインにこのアカウントを使用する、 **Microsoft デベロッパー センター** と **Azure 発行ポータル**します。 複数の Azure Marketplace サービスに対して 1 つの Microsoft アカウントを使用してください。 各サービスおよびプランに対し、個別のアカウントを作成する必要はありません。

ユーザー名を形成するアドレスは、自分のドメインに配置してくださいし、(publishing@example.com) など、IT チームによって制御されます。 支払いや税金に関する情報、またレポートは、このアカウントを通じて配信されます。

  > [AZURE.WARNING] "Azure"と"Microsoft"などの単語は、Microsoft アカウントの登録はサポートされていません。 アカウントの作成および登録プロセスを完了するには、これらの単語を使用しないでください。

### このサンプルについての指示

1. 会社のドメイン内で、配布リスト (DL) またはセキュリティ グループ (SG) を作成します。
  - DL にオンボーディング チームを追加します。
  - DL は確認用メールを受信するためにライブである必要があります。
  - 電子メール アドレスとして、この dl marketplace@example.com を使用します。
  - これは、内部システムで完了する必要があります。
2. 新しい Chrome Incognito または Internet Explorer InPrivate ブラウズ セッションを開き、既存のアカウントにサインインしていないことを確認します。
3. DL の電子メールを使用して Microsoft アカウントを登録します。
 - Microsoft アカウントを登録できます [https://signup.live.com/signup.aspx](https://signup.live.com/signup.aspx)します。
 - 電子メール アドレスとして marketplace@example.com を使用します。
 - Marketplace@example.com が、Microsoft アカウントの ID。
4. 登録時は、有効な電話番号を使用します。 ID 検証が必要な場合は、システムによりテキスト メッセージまたは自動通話として検証コードが送信されます。
5. DL に送信された電子メール アドレスを確認します。
6. これで、Microsoft デベロッパー センターで新しい Microsoft アカウントを使用する準備ができました。

> [AZURE.TIP] 配布リストを使用すると、複数の人の支払い情報のレポートの重要な電子メール通知を受け取ることができます。 また、Microsoft アカウントの所有権を譲渡できるようにし、1 人のユーザーに関連付けられないようにすることもできます。

## 2.Microsoft デベロッパー センター アカウントの作成
Microsoft デベロッパー センターは、会社情報を登録するために 1 回だけ使用します。 登録者は、合法的な企業の代表者である必要があります。また、ID を検証する方法として、個人情報を提供する必要があります。 登録時にユーザーが、会社用に共有されている Microsoft アカウントを使用する必要があります **Azure 発行ポータルでは、同じアカウントを使用する必要があります。** 作成する前に、会社がまだ Microsoft デベロッパー センターのアカウントを所有していないことを確認してください。 作成中は、会社の住所情報、銀行口座情報、および税金に関する情報が収集されます。 通常、これらは財務またはビジネス用連絡先から取得できます。

> [AZURE.IMPORTANT] プランの作成および展開のさまざまな段階を通過するために、次の販売者プロファイル コンポーネントを行う必要があります。


| 販売者のプロファイル | ドラフトを開始するには | ステージング | 無料とソリューション テンプレートの発行 | 商用プランの発行 |
|----|----|----|----|----|
|会社登録 | 必須 | 必須 | 必須 | 必須 |
|税プロファイル ID | 省略可能 | 省略可能 | 省略可能 | 必須 |
|銀行口座 | 省略可能 | 省略可能 | 省略可能 | 必須 |


> [AZURE.NOTE] Bring Your 独自ライセンス (BYOL) は、バーチャル マシンにのみサポートされているし、は見なされません、 **無料** を提供します。


### 会社アカウントの登録
1. またはを開きます新しい Internet Explorer InPrivate ブラウズ セッション Chrome Incognito、個人のアカウントにないサインインしていることを確認します。

2. 移動して [http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)します。

3. 企業登録 (marketplace@example.com など) の Microsoft アカウントでサインインします。

    ![図][img-signin]

4. [アカウントの保護にご協力ください] ウィザードを完了します。これにより、電話番号または電子メール アドレスから ID が検証されます。

    ![図][img-verify]

5. "登録アカウント情報] セクションで、選択、 **アカウントの国/地域** ] ドロップダウン メニューからです。

    > [AZURE.WARNING] **「販売の」国:** Azure Marketplace で、サービスを販売するために、登録されているエンティティは上記の国承認済み「販売元」のいずれかからである必要があります。 この制限は、支払いおよび課税上の理由から生じます。 「販売元」の国の範囲は今後拡大される予定です。 詳細については、次を参照してください。、  [Marketplace への参加ポリシー](http://go.microsoft.com/fwlink/?LinkID=526833)します。

6. 「アカウントの種類」を選択して **個別** または **会社**します。

    <!-- Insert screenshot -->

    > [AZURE.IMPORTANT] 理解勘定科目の種類と、これは最適であるかを選択すると、ページを参照してください [アカウントの種類、場所、および料金](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)

7. 入力、 **発行者表示名**, 、通常、会社の名前。

    > [AZURE.TIP] 現時点では、Azure 発行ポータルでは、発行者表示名は使用しません。 登録プロセスを完了するために入力する必要があります。

8. アカウントの連絡先情報を入力します。

    > [AZURE.IMPORTANT] それが使用されるため、検証プロセス、会社のデベロッパー センターで承認するのには、正確な連絡先情報を入力してください。

9. 会社のアカウントにもする必要がありますの連絡先情報を提供する、 **会社の承認者**, 、人、組織の代わりにアカウントを作成する権限があることを確認できます。 完了したら、クリックして **次** 上に移動する、 **「支払いセクション」**します。

10. アカウントの料金を支払うための支払い情報を入力します。 登録コストが免除されるプロモーション コードがある場合は、ここで入力できます。 ない場合は、クレジット カード情報 (またはサポートされている市場であれば PayPal 情報) を入力します。 完了したら、クリックして **次** 上に移動する、 **「レビュー画面」**します。

11. アカウント情報を見直し、すべて正しいことを確認します。 次を読みの条件に同意、 **Microsoft Azure Marketplace 発行者契約**します。 チェック ボックスをオンにすると、これらの条件を読んで同意したと見なされます。

12. をクリックして **完了** をお客様の登録を確認します。 確認メッセージが開発者の電子メール アドレスに送信されます。

13. をのみ無料サービスを発行する予定の場合はクリックして **Azure Marketplace の発行ポータルに移動** し、このドキュメントのセクション 3 にスキップできます [公開ポータルでアカウントを登録する](#3-register-your-account-in-the-publishing-portal)です。

14. 商用オファーの公開を計画している場合はクリックして **アカウント情報を更新** 税金とバンキング デベロッパー センター アカウントに情報を完了する必要があります。

> [AZURE.IMPORTANT] ステージング環境で、オファーを正確にテストすることはできませんし、実稼働環境に税金と銀行口座の情報を完了しなくても、サービスをプッシュすることはできません。

セクション 3 に移動することができます、税および銀行後で情報を更新する場合は、 [公開ポータルでアカウントを登録する](#3-register-your-account-in-the-publishing-portal), 、されるもので、Azure 発行ポータルでのリンクを使用して、後でもう一度です。

### 税金と銀行取引情報の追加
 購入向けに商用プランを発行する場合は、支払い情報と税金情報を追加し、デベロッパー センターでの検証のために送信する必要があります。 無料プラン (または、BYOL プラン) のみを発行する場合は、この情報を追加する必要はありません。 後ほど追加することはできますが、税金情報の検証に時間がかかります。 購入向けに商用プランを提供することが確定している場合は、これらの情報を可能な限り早く入力することをお勧めします。

**銀行口座情報**

1. サインイン、 [Microsoft デベロッパー センター](http://dev.windows.com/registration?accountprogram=azure) Microsoft アカウントを使用します。

2. をクリックして **支払いアカウント** 左側のメニューで [ **支払方法を選択する** ] をクリックして **銀行口座** または **PayPal**します。

    > [AZURE.IMPORTANT] Marketplace で顧客が購入した商用オファーがあれば、これは、アカウントでこれらの購入の支払いを受信します。

3. 支払情報を入力し、クリックして **保存** したら。

    > [AZURE.IMPORTANT] 更新またはお客様の支払いアカウントを変更する必要がある場合、同じ手順に従います上で新しい情報で現在の情報を交換します。 支払いアカウントを変更すると、最大で支払い期間 1 回分の遅延が支払いに発生する可能性があります。 この遅延は、支払いアカウントが初めて設定された場合と同様に、アカウント変更の確認が必要になるために発生します。 アカウントが確認された後も、お支払いは引き続き一括払いになります。期日が現在の支払い期間内にある支払いは、次の支払い期間に追加されます。

4. クリックして **次**します。

**税金情報**

1. サインイン、 [Microsoft デベロッパー センター](http://dev.windows.com/registration?accountprogram=azure) (必要な場合)、Microsoft アカウントを使用します。

2. クリックして **プロファイルの税** 、左側のメニュー。

3.  **税フォームの設定** ページで、国または永続的な常駐する必要がある地域を選択し、国またはプライマリ市民権を保持する領域を選択します。 クリックして **次**します。

4. 税の詳細を入力し、クリックして **次**します。

> [AZURE.WARNING] Microsoft デベロッパー センター アカウントに、税金と銀行口座の情報を完了しなくても、コマーシャルは、運用環境へのプッシュできなきます。

## 3.発行ポータルでのアカウントの登録
Azure 発行ポータルは、プランの発行と管理に使用されます。 発行ポータルでは、プランの作成プロセスで役に立つ、有用な情報を検索できます。

> [AZURE.WARNING] 同じ会社の Microsoft デベロッパー センターの登録で使用されている Microsoft アカウントを使用する必要があります。 マスター発行者アカウントの作成を支援するため、さらにユーザーを追加できます。

1.  新しい Chrome Incognito または Internet Explorer InPrivate ブラウズ セッションを開き、個人用アカウントにサインインしていないことを確認します。

2.  移動して [http://publish.windowsazure.com](http://publish.windowsazure.com)します。

3.  企業登録 (つまり、marketplace@example.com) の Microsoft アカウントを使用してサインインし、必要に応じて、共同管理者を追加することができます。

  > [AZURE.TIP] 参加のポリシーを参照して、 [Azure の web サイト](http://azure.microsoft.com/support/legal/marketplace/participation-policies/)します。

  > デベロッパー センターの登録に関する問題がある場合は、次に示すように、サポート チケットをログインしてください。
  1. 連絡先 [サポート](https://support.microsoft.com/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&supportregion=en-us&pesid=15635&ccsid=635847950577064286)します。
  2. 選択 **デベロッパー センター**します。
  3. 選択 **プロファイル**します。
  4. 連絡方法を選択する。






## 次のステップ
これで、アカウントが作成され、登録されたので、Azure Marketplace に発行したいアーティファクトのタイプ (仮想マシン、開発者サービス、データ サービス、ソリューション テンプレート) をクリックします。 それぞれのプランを発行する方法については、次の記事のいずれかを参照してください。

|| 仮想マシン イメージ | 開発者サービス | データ サービス | ソリューション テンプレート |
|----|-----|-----|-----|-----|
|**ステップ 2: プランの作成** |[一般的な技術面以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術面以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術面以外の前提条件](marketplace-publishing-pre-requisites.md)| [一般的な技術面以外の前提条件](marketplace-publishing-pre-requisites.md)|
|| [VM イメージの技術的な前提条件][link-single-vm-prereq] | 開発者サービスの技術的な前提条件 | [データ サービスの技術的な前提条件](marketplace-publishing-data-service-creation-prerequisites.md)  | [ソリューション テンプレートの技術的な前提条件](marketplace-publishing-solution-template-creation-prerequisites.md) |
|| [VM イメージの発行ガイド][link-single-vm] | 開発者サービスの発行ガイド | [データ サービスの発行ガイド](marketplace-publishing-data-service-creation.md) | [ソリューション テンプレートの発行ガイド](marketplace-publishing-solution-template-creation.md) |
|| [Azure Marketplace のマーケティング コンテンツ ガイド][link-pushstaging] | [Azure Marketplace のマーケティング コンテンツ ガイド][link-pushstaging] | [Azure Marketplace のマーケティング コンテンツ ガイド][link-pushstaging] | [Azure Marketplace のマーケティング コンテンツ ガイド][link-pushstaging] |

## 関連項目
- [Microsoft Azure Marketplace へのプランの発行方法](marketplace-publishing-getting-started.md)

[img-msalive]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-live.jpg
[img-email]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-verifyemail.jpg
[img-sd-url]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-incognito.jpg
[img-signin]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-login.jpg
[img-verify]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-verify.jpg
[img-sd-top]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-details.jpg
[img-sd-info]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal.jpg
[img-sd-type]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-type.jpg
[img-sd-mktg1]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det1.jpg
[img-sd-mktg2]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det2.jpg
[img-sd-addr]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-add.jpg
[img-sd-legal]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-cmp.jpg
[img-sd-submit]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-approval.jpg

[link-msdndoc]: https://msdn.microsoft.com/library/jj552460.aspx
[link-sellerdashboard]: http://sellerdashboard.microsoft.com/
[link-pubportal]: https://publish.windowsazure.com
[link-single-vm]:marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]:marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]:marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]:marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]:marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]:marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]:marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]:marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]:marketplace-publishing-push-to-staging.md


