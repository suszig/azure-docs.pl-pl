<properties 
   pageTitle="Azure Data Lake Store に格納されているデータのセキュリティ保護 | Azure" 
   description="グループおよびアクセス制御リストを使用して Azure Data Lake Store 内のデータをセキュリティ保護する方法を説明する" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/11/2015"
   ms.author="nitinme"/>

# Azure Data Lake Store に格納されているデータのセキュリティ保護

Azure Data Lake Store のデータをセキュリティで保護するには 3 つの手順が必要です。

1. まず、Azure Active Directory (AAD) でセキュリティ グループを作成します。 Azure ポータルで、これらのセキュリティ グループを使用してロールベースのアクセス制御 (RBAC) を実装します。 詳細については、次を参照してください。 [Microsoft Azure でのアクセス制御の役割に基づいた](role-based-access-control-configure.md)します。

2. AAD セキュリティ グループを Azure Data Lake Store アカウントに割り当てます。 これにより、ポータルから Data Lake Store アカウントへのアクセス、およびポータルまたは API による管理操作が制御されます。

3. AAD セキュリティ グループをアクセス制御リスト (ACL) として Data Lake Store ファイル システムに割り当てます。

この記事では、Azure ポータルを使用して上記タスクを実行する方法について説明します。 

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure Data Lake ストア アカウント**します。 1 つを作成する方法については、次を参照してください [Azure Data Lake ストアを使ってみる。](data-lake-store-get-started-portal.md)

## Azure Active Directory でセキュリティ グループを作成する

AAD セキュリティ グループを作成する方法と、グループにユーザーを追加する方法については、次を参照してください。 [Azure Active Directory でセキュリティ グループを管理する](active-directory-accessmanagement-manage-groups.md)です。

## ユーザーまたはセキュリティ グループを Azure Data Lake Store アカウントに割り当てる

ユーザーまたはセキュリティ グループを Azure Data Lake Store アカウントに割り当てる場合は、Azure ポータルと Azure リソース マネージャー API を使用して、アカウントに関する管理操作へのアクセスを制御します。 

1. Azure Data Lake Store アカウントを開きます。 左側のウィンドウから次のようにクリックします。 **参照**, 、] をクリック **データストア湖**, 、湖のデータ ストア] ブレードからの [ユーザーまたはセキュリティ グループを割り当てるするアカウント名] をクリックします。

2. Data Lake Store アカウントのブレードで、ユーザー アイコンをクリックします。

    ![セキュリティ グループを Azure Data Lake Store アカウントに割り当てる](./media/data-lake-store-secure-data/adl.select.user.icon.png "Assign security group to Azure Data Lake Store account")

3.  **ユーザー** 既定リストによってブレード **サブスクリプション管理者** 所有者としてグループ化します。 

    ![ユーザーとロールを追加する](./media/data-lake-store-secure-data/adl.add.group.roles.png "Add users and roles")
 
    グループを追加し関連するロールを割り当てるには 2 つの方法があります。

    * アカウントにユーザーまたはグループを追加して、ロールを割り当てる。あるいは、
    * ロールを追加し、ロールにユーザーまたはグループを割り当てる。

    このセクションでは、グループを追加してからロールを割り当てるという 1 番目の方法を説明します。 同様の手順を実行して、ロールを選択してからグループをそのロールに割り当てることもできます。
    
4.  **ユーザー** ブレードで、をクリックして **追加** を開くには、 **アクセスを追加する** ブレードです。  **アクセスを追加する** ブレードで、] をクリックして **ロールを選択**, 、ユーザー/グループのロールを選択します。

     ![ユーザーのロールを追加する](./media/data-lake-store-secure-data/adl.add.user.1.png "Add a role for the user")

     **所有者** と **の共同作業者** ロールが data lake アカウント上のさまざまな管理機能へのアクセスを提供します。 Data Lake 内のデータと対話するユーザーについては、該当するユーザーを **[閲覧者]** ロールに追加できます。 これらのロールの適用範囲は、Azure Data Lake Store アカウントに関連する管理操作に限定されます。

    データ操作の場合、ユーザーが行える操作はファイル システムの個々のアクセス許可によって定義されます。 そのため、閲覧者ロールを持つユーザーはアカウントに関連付けられた管理設定しか表示できません。しかし、場合によっては、そのユーザーに割り当てられているファイル システムのアクセス許可に基づいて、データの読み取りおよび書き込みができます。 湖のデータ ストアのファイル システムのアクセス許可が説明されている [Azure Data Lake ストアのファイル システムに Acl として、セキュリティ グループを割り当てる](#filepermissions)します。



5.  **アクセスを追加する** ブレードで、をクリックして **ユーザーを追加する** を開くには、 **ユーザーを追加** ブレードです。 このブレードで、前に Azure Active Directory で作成したセキュリティ グループを検索します。 検索対象のグループが多数存在する場合は、上部にあるテキスト ボックスを使用してグループ名をフィルター処理できます。 クリックして **選択**します。

    ![セキュリティ グループを追加する](./media/data-lake-store-secure-data/adl.add.user.2.png "Add a security group")

    使用して招待できるを一覧表示されていないグループまたはユーザーを追加する場合、 **招待** アイコンと、ユーザー/グループの電子メール アドレスを指定します。

6. Click **OK**. 次に示すように追加したセキュリティ グループが表示されます。

    ![追加されたセキュリティ グループ](./media/data-lake-store-secure-data/adl.add.user.3.png "Security group added")

7. これで、ユーザーまたはセキュリティ グループは、Azure Data Lake Store アカウントにアクセスできるようになりました。 特定のユーザーにアクセス権を付与する場合は、対象ユーザーをセキュリティ グループに追加します。 同様に、ユーザーのアクセス権を取り消す場合は、セキュリティ グループから対象ユーザーを削除します。 アカウントには複数のセキュリティ グループを割り当てることもできます。 

## <a name="filepermissions"></a>Azure Data Lake ストアのファイル システムに Acl としてユーザーまたはセキュリティ グループを割り当てる

Azure Data Lake ファイル システムにユーザーまたはセキュリティ グループを割り当てて、Azure Data Lake Store に格納されたデータに対するアクセス制御を設定します。 現在のリリースでは、ファイル システムのルート ノードに ACL を設定できるだけです。

1. Data Lake ストア アカウントのブレードでクリックして **データ エクスプ ローラー**します。

    ![Data Lake Store アカウントにディレクトリを作成する](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Create directories in Data Lake account")

2.  **データ エクスプ ローラー** ブレードで、自分のアカウントのルートをクリック、[アカウント] ブレードで、 **アクセス** アイコン。

    ![Data Lake ファイル システムに ACL を設定する](./media/data-lake-store-secure-data/adl.acl.1.png "Set ACLs on Data Lake file system")

3.  **アクセス** ブレードが標準的なアクセスと、ルートに既に割り当てられているカスタムのアクセスを一覧表示します。 クリックして、 **追加** レベルのカスタマイズの Acl を追加するアイコン。

    ![標準アクセスとカスタム アクセスを一覧表示する](./media/data-lake-store-secure-data/adl.acl.2.png "List standard and custom access")

    * 標準アクセスは UNIX 形式のアクセスです。この場合は、読み取り、書き込み、実行 (rwx) を 3 つの個別のユーザー クラス (所有者、グループ、その他) に指定します。
    * カスタム アクセスは POSIX ACL に対応します。この場合、ファイルの所有者またはグループだけでなく、特定の名前付きユーザーまたはグループにもアクセス許可を設定することができます。
    
    詳細については、次を参照してください。 [HDFS Acl](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)します。 

4. クリックして、 **追加** を開くアイコン、 **カスタム アクセスの追加** ブレードです。 このブレードで、[ **[ユーザーまたはグループ**, 、し **[ユーザーまたはグループ** ブレードで、Azure Active Directory で作成したセキュリティ グループを探します。 検索対象のグループが多数存在する場合は、上部にあるテキスト ボックスを使用してグループ名をフィルター処理できます。 追加するグループをクリックして **選択**します。

    ![グループを追加する](./media/data-lake-store-secure-data/adl.acl.3.png "Add a group")

5. をクリックして **アクセス許可の選択**, 、そのグループに割り当てる] をクリックするアクセス許可を選択 **OK**します。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-secure-data/adl.acl.4.png "Assign permissions to group")


    >[AZURE.NOTE] The Execute permission is required for enumeration of directories and is often required when providing a user or group read-only access to data.


6.  **カスタム アクセスの追加** ブレードで、をクリックして **[ok]**します。 関連付けられているアクセス許可を持つ、新しく追加されたグループが表示されるよう、 **アクセス** ブレードです。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-secure-data/adl.acl.5.png "Assign permissions to group")

    > [AZURE.IMPORTANT] 現在のリリースでは、下にある 12 エントリだけ配置できます **独自のアクセス**します。 12 を超えるユーザーを追加する場合は、セキュリティ グループを作成し、セキュリティ グループにユーザーを追加し、それらのセキュリティ グループに Data Lake Store アカウントに対するアクセス権を付与します。　

7. 必要に応じて、グループを追加した後で、アクセス許可を変更することもできます。 各種類のアクセス許可 (Read、Write、Execute) のチェック ボックスをオフにするかオンにするかは、セキュリティ グループにアクセス許可を割り当てるか、セキュリティ グループからアクセス許可を削除するかに応じて決定します。 をクリックして **保存** 、変更を保存または **破棄** 変更を元に戻す。

## Azure Data Lake Store アカウントのセキュリティ グループを削除する

Azure Data Lake Store アカウントからセキュリティ グループを削除する場合は、Azure ポータルと Azure リソース マネージャー API を使用して、アカウントに関する管理操作へのアクセスを変更するだけです。

1. Data Lake Store アカウントのブレードで、ユーザー アイコンをクリックします。

    ![セキュリティ グループを Azure Data Lake アカウントに割り当てる](./media/data-lake-store-secure-data/adl.select.user.icon.png "Assign security group to Azure Data Lake account")

2.  **ユーザー** ブレードを削除するセキュリティ グループ] をクリックします。

    ![削除するセキュリティ グループ](./media/data-lake-store-secure-data/adl.add.user.3.png "Security group to remove")

3. セキュリティ グループのブレードで、次のようにクリックします。 **削除**します。

    ![削除されたセキュリティ グループ](./media/data-lake-store-secure-data/adl.remove.group.png "Security group removed")

## Azure Data Lake Store ファイル システムからセキュリティ グループ ACL を削除する

Azure Data Lake Store ファイル システムからセキュリティ グループの ACL を削除する場合は、Data Lake Store ストア内のデータへのアクセスを変更します。

1. Data Lake ストア アカウントのブレードでクリックして **データ エクスプ ローラー**します。

    ![Data Lake アカウントにディレクトリを作成する](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Create directories in Data Lake account")

2.  **データ エクスプ ローラー** ブレードで、自分のアカウントのルートをクリック、[アカウント] ブレードで、 **アクセス** アイコン。

    ![Data Lake ファイル システムに ACL を設定する](./media/data-lake-store-secure-data/adl.acl.1.png "Set ACLs on Data Lake file system")

3.  **アクセス** ブレードから、 **独自のアクセス** セクションで、セキュリティ グループを削除する] をクリックします。  **独自のアクセス** ブレードで、をクリックして **削除** ] をクリックし、 **[ok]**します。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-secure-data/adl.remove.acl.png "Assign permissions to group")


## 関連項目

- [Azure Data Lake Store の概要](data-lake-store-overview.md)
- [Azure Storage BLOB から Data Lake Store へのデータのコピー](data-lake-store-copy-data-azure-storage-blob.md)
- [Data Lake Store で Azure Data Lake Analytics を使用する](data-lake-analytics-get-started-portal.md)
- [Data Lake Store で Azure HDInsight を使用する](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Azure PowerShell で Data Lake Store の使用を開始する](data-lake-store-get-started-powershell.md)
- [.NET SDK で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-net-sdk.md)

