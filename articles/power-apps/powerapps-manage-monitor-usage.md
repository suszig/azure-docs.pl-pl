<properties
    pageTitle="PowerApps Enterprise でアプリの使用状況を確認する |Microsoft Azure"
    description="すべてのアプリ、API、ユーザー、アプリの要求、更新のアクセス許可を Azure ポータルに表示する"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>



# PowerApps を管理し保護する

App Service Environment を作成し、API とその接続を追加します。 これで、組織のユーザーが、API とその接続を使用できるようになります。 また、組織で作成されるすべてのアプリを管理することもできます。 設定できるオプションは次のとおりです。

- PowerApps、Web Apps、Logic Apps、Mobile Apps など、App Service Environment にあるさまざまなアプリを表示する。
- 特定のアプリで使用される API をすべて表示する。
- App Service Environment にあるアプリへのユーザー アクセスを表示し、管理する。
- API とその接続に対するユーザー アクセスを表示し、管理する。

App Service Environment は、Web Apps や Logic Apps などの他のアプリを追加するために提供されています。 PowerApps Enterprise を開くことで、これらのアプリを表示し、管理することができます。


## PowerApps 管理者を追加する

PowerApps Enterprise が有効になり、使用する準備が完了したら、管理者を追加し、App Service Environment にある他のアプリを監視することができます。

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **[設定]** を選択します。
3. **設定**, [ **Admin**:  
![][1]
4. **[ユーザー]** で、**[追加]** を選択します。
5. 選択、 **所有者** ロール。  
![][2]
    > [AZURE.IMPORTANT] PowerApps 管理者として割り当てているユーザーがある場合には、**[所有者]** ロールを必ず選択してください。 表示されているその他のロールは、PowerApps を管理するためのフル アクセスをユーザーに付与しません。 

6. ユーザーまたはグループを選択します。
7. **[OK]** を選択して手順を完了します。

PowerApps Enterprise に管理者を追加すると、管理者として追加したユーザーおよびグループは次のことを行えるようになります。

- 他のユーザーを PowerApps 管理者として追加する。
- すべてのアプリと、そのユーザー アクセスを管理する。
- 請求を変更することはできません。

> [AZURE.IMPORTANT] PowerApps 管理者は、App Service Environment のリソース グループに対する所有者のロールが与えられるまで、App Service の環境を変更することができません。 これを行うには、次を参照してください。 [今回](powerapps-get-started-azure-portal.md)します。

App Service Environment のリソース グループに所有者ロールが付与されると、PowerApps 管理者は次のことも行えるようになります。

- API とその接続を作成し、構成する。
- App Service Environment を含む、PowerApps の設定に変更を行う。
- その他のユーザーとグループを追加し、Api、その接続では、app service 環境への役割とアクセス許可に付けます。


## PowerApps とその他の種類のアプリを管理する

PowerApps と App Service Environment を有効にすると、Web Apps や Logic Apps のような他のアプリを同じ App Service Environment に追加できるようになります。 これを行うと、アプリが PowerApps で作成されたアプリと一緒に *[すべてのアプリ]* に表示されます。 アプリの種類をクリックすることで、アプリの概要を確認できます。

### PowerApps を表示し、管理する

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **すべてのアプリ** タイルで、 **PowerApps**:  
![][3]
3. アプリを選択して、次のようなアプリの詳細を表示します。
    - アプリが使用する API
    - アプリにアクセスできるユーザーおよびグループ
    - アプリの分析 (近日対応予定)

#### アプリを追加する

Azure ポータルからアプリを追加することはできません。 現時点に移動、 [PowerApps ポータル](http://go.microsoft.com/fwlink/p/?LinkId=715583)します。

#### PowerApps で作成されたアプリを削除する

PowerApps 管理者は PowerApps で作成されたアプリや App Service Environment にあるその他の種類のアプリを含めたすべてのアプリを削除することができます。 アプリを削除する選択、 **すべてのアプリ** を並べて表示して、アプリケーションを選択し、[ **削除**:    
![][4]


#### アプリを使用するためのアクセス権をユーザーまたはグループに付与する

PowerApps 管理者は、PowerApps との間でユーザーおよびグループの追加および削除を行うことができます。

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **すべてのアプリ** タイルで、 **PowerApps**:  
![][3]
3. **[サービス デスク]** などのアプリを選択します。
4. **設定**, [ **アプリ ユーザーのアクセス権**:  
![][5]
5. **[追加]** を選択し、新しいユーザーまたはグループを追加します。
6. 次のロールを選択します。
    - 編集可能
    - 表示可能
7. ユーザーまたはグループを選択します。
8. **[OK]** を選択して手順を完了します。

### Logic Apps を表示し、管理する

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **すべてのアプリ** タイルで、 **Logic apps**:  
![][8]
3. Logic Apps を選択して、アプリの詳細を表示します。 適切なロジック アプリを一覧表示 PowerApps の修正のサブスクリプションを選択したことを確認します。  
![][7]
    > [AZURE.IMPORTANT] パブリック プレビューでは、[参照] ブレードの Logic Apps の数と、メインの [PowerApps] ブレードに表示される数とで不整合がある場合があります。 これは予期されることです。 ポータルでは、すべてのホスティング プランのすべての Logic Apps が表示され、PowerApps でデプロイした App Service Environment の Logic Apps はフィルター処理されていません。 この動作は、今後の更新で修正される予定です。

    * * を Logic apps とその管理方法に関する詳細については、次を参照してください [手順](https://azure.microsoft.com/documentation/services/app-service/logic/)。 * *。

### Web Apps を表示し、管理する

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **すべてのアプリ** タイルで、 **Web アプリの**:  
![][9]

    * * を web アプリとその管理方法に関する詳細については、次を参照してください [手順](https://azure.microsoft.com/documentation/services/app-service/web/)。 * *。

### Mobile Apps を表示し、管理する

1. [Azure ポータル](https://portal.azure.com/), 、開かれている **PowerApps**します。
2. **すべてのアプリ** タイルで、 **モバイルアプリ**:  
![][10]

    * * をモバイル アプリとその管理方法に関する詳細については、次を参照してください [手順](https://azure.microsoft.com/documentation/services/app-service/mobile/)。 * *。


## セキュリティ オプションを確認する

実行内容によって、さまざまなセキュリティ メソッドが使用されます。 次の内容を知っておく必要があります。

- **サブスクリプション管理者**: このような管理者は、請求を制御し、PowerApps Enterprise への会社のサインアップを担当します。 サブスクリプション管理者のみが、会社の Azure サブスクリプション内での PowerApps の有効化を要求できます。

- **ランタイム ユーザー アクセス**: ランタイム ユーザー アクセスには 3 つの種類があります。
    - **アプリ ユーザーのアクセス権**: このアクセス許可では、アプリのユーザーがアプリを*編集可能*かどうか、アプリを*表示可能*かどうかを制御します。
    - **API ユーザー アクセス**: このアクセス許可では、ランタイム アクセスを制御します。 ユーザーにこのアクセス許可が与えられている場合、そのユーザーはアプリで API を使用することができます。 ユーザーには API をランタイムに使用するためのアクセス許可がある場合も、ない場合もあります。
    - **接続ユーザー アクセス**: 接続に使用できるランタイム ユーザーのアクセス許可は *[表示可能]* および *[編集可能]* です。 API (または接続プロファイル) 追加の接続を作成すると、ユーザーとグループこれら特定アクセス許可を割り当てます。  
        ![][6]

        For example, you can give the Sales group within your company *Can edit* permission to a connection of a SQL connector API. User with *Can edit* permission will be able to use the connection in their apps as well as edit the connection configuration. User with *Can view* permission will be able to use the connection in their apps but can't modify the connection configuration such as connection string. 


- **ロールベースのアクセス制御** (RBAC): 多くの Azure サービスでは、ロールベースのアクセス制御を使用して誰が何をできるかを決定します。 PowerApps では、RBAC は次のような場合に使用されます。
    - 初めて PowerApps ポータルに入る場合。PowerApps の管理者となるべきユーザーを追加して管理することができます。
    - App Service Environment を作成する場合。PowerApps にユーザーまたはグループを追加したり、PowerApps からユーザーまたはグループを削除したりできます。 たとえば、会社内の特定の管理者グループを *所有者*ロールに追加することで、API と接続を作成できるようになります。 この API と接続は、PowerApps で作成されたアプリに追加されます。
    - Web Apps、Logic Apps、Mobile Apps のようなアプリにユーザーを追加する場合。 これらのユーザーのロールを選択できます。

        Adding users and assigning roles is just like using [Role-based access control](../role-based-access-control-configure.md) within Azure. Some roles include:   

     ロール| 説明
    --- | ---
     共同作成者| ユーザーへのアクセス権の付与を除く、すべてを管理します。
     閲覧者| すべてを閲覧できますが、変更を加えることはできません。
     所有者| すべてを管理し、ユーザーにアクセス権を付与することができます。


これらのロールを使用して、userA に日常的に使用する Twitter アプリへの**表示可能**のアクセス許可を、userB に ShuttleBus アプリへの**編集可能**のアクセス許可を与えることができます。 userB にはすべての API へのアクセス権を付与することができます。 これらの権限で詳細を把握したり、特定のロールのユーザーを全員追加したりできます。 これは、ビジネス ニーズによって異なります。


## まとめと次のステップ

このトピックでは、PowerApps を管理するためのさまざまなオプションと、PowerApps に実装されているセキュリティ メソッドについて説明しました。

これで、Azure ポータルのエクスペリエンスが構成されたので、アプリの作成を始めましょう。 最初に次のような記事を読むことをお勧めします。

- [PowerApps でテンプレートからのアプリを作成します。](http://go.microsoft.com/fwlink/p/?LinkId=715536)
- [PowerApps 内のデータからアプリを作成します。](http://go.microsoft.com/fwlink/?LinkId=715539)
- [PowerApps で最初からアプリを作成します。](http://go.microsoft.com/fwlink/p/?LinkId=715538)



[1]: ./media/powerapps-manage-monitor-usage/addadmin.png 
[2]: ./media/powerapps-manage-monitor-usage/selectrole.png 
[3]: ./media/powerapps-manage-monitor-usage/PowerApps.png 
[4]: ./media/powerapps-manage-monitor-usage/deleteapp.png 
[5]: ./media/powerapps-manage-monitor-usage/appuseraccess.png 
[6]: ./media/powerapps-manage-monitor-usage/selectpermission.png 
[7]: ./media/powerapps-manage-monitor-usage/alllogicapps.png 
[8]: ./media/powerapps-manage-monitor-usage/logicapps.png 
[9]: ./media/powerapps-manage-monitor-usage/webapps.png 
[10]: ./media/powerapps-manage-monitor-usage/mobileapps.png 

