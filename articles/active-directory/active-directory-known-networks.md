<properties 
    pageTitle="既知のネットワーク |　Microsoft Azure" 
    description="既知のネットワークを構成すると、"複数の地域からのサインイン" レポートおよび "疑わしいアクティビティを示す IP アドレスからのサインイン" レポートに、組織が所有する IP アドレスが含まれないようにすることができます。" 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="msStevenPo"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="markvi"/>

# 既知のネットワーク


Azure Active Directory のアクセスおよび使用状況レポートを使用すると、組織のディレクトリの整合性とセキュリティを表示できます。 ディレクトリ管理者は、この情報を使用して、リスクを軽減するために適切に計画できるように、セキュリティ上のリスクがある箇所をより適切に確認できます。

可能であればを"*複数の地域からのサインイン*「と」*不審なアクティビティの IP アドレスからのサインイン*"レポートが、組織で実際に所有されている IP アドレスを正しくフラグします。 

これは、たとえば次の場合に起こります。 

- ボストン支社のユーザーがサンフランシスコのデータ センターにリモートでサインインすると、"複数の地域からのサインイン" レポートがトリガーされます。 

- 組織のユーザーが間違ったパスワードで複数回サインインしようとすると、"疑わしいアクティビティを示す IP アドレスからのサインイン" レポートがトリガーされます。 

こうした場合に誤解を招くセキュリティ レポートが生成されないようにするには、組織の パブリック IP アドレスの一覧に既知の IP アドレス範囲を追加する必要があります。    


###組織のパブリック IP アドレス範囲を追加するには、次の手順に従います。 

1.  サインオン、 [Azure 管理ポータル](https://manage.windowsazure.com)します。

2.  左のウィンドウで **Active Directory**します。 <br><br>![Cloud App Discovery のしくみ](./media/active-directory-known-networks/known-netwoks-01.png)

3.   **ディレクトリ** ] タブで、ディレクトリを選択します。

4.  上部にあるメニュー [ **構成**します。 <br><br>![Cloud App Discovery のしくみ](./media/active-directory-known-networks/known-netwoks-02.png)

5.  [構成] タブに移動 **組織パブリック IP アドレス範囲** <br><br>![クラウド アプリケーション検出の動作のしくみ](./media/active-directory-known-networks/known-netwoks-03.png)

6.  クリックして **既知の IP アドレス範囲を追加**します。

7.  表示されたら、ダイアログ ボックスで、アドレスの範囲を追加し、完了したときに、チェック ボタンをクリックします。 <br><br>![Cloud App Discovery のしくみ](./media/active-directory-known-networks/known-netwoks-04.png)


**その他のリソース**


* [アクセスおよび使用状況レポートを表示します。](active-directory-view-access-usage-reports.md)
* [不審なアクティビティのある IP アドレスからのサインイン](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [複数の地域からのサインイン](active-directory-reporting-sign-ins-from-multiple-geographies.md)



