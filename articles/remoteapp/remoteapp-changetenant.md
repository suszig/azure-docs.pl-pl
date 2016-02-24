
<properties
    pageTitle="Azure RemoteApp で Azure Active Directory テナントを変更する | Microsoft Azure"
    description="Azure RemoteApp に関連付けられている Azure Active Directory テナントの変更方法について説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/04/2015"
    ms.author="elizapo" />



# Azure RemoteApp で Azure Active Directory テナントを変更する

Azure RemoteApp では Azure Active Directory (Azure AD) を使用してユーザーのアクセスを許可します。 Azure RemoteApp で使用可能な唯一の Azure AD テナントは、Azure サブスクリプションに関連付けられているテナントです。 関連付けられているサブスクリプションを表示する、 **設定** 、ポータル ページです。 見て、 **ディレクトリ** 列に、 **サブスクリプション** ] タブをクリックします。

> [AZURE.NOTE] この変更を成功させるには、最初にすべての Azure RemoteApp コレクションから既存の Azure Active Directory テナントからすべてのユーザーを削除します。 これを行うには、Azure ポータルに移動して、 **Azure RemoteApp** タブし、すべての Azure RemoteApp コレクションを開きます。 移動して、 **ユーザー** タブし、現在の Azure Active Directory テナントに属しているユーザーを削除します。 すべての既存の Azure RemoteApp コレクションに対して繰り返します。 この作業を行わないと、コレクションを作成またはパッチできません。

別のテナントを使用する場合は、この手順を使用して、サブスクリプションの関連付けを変更します。

1. ポータルで、Azure RemoteApp コレクションへのアクセス許可を付与した Azure AD ユーザーを削除します。 (これを行う手順については、上の注を参照してください。)


2. サービス管理者として、Microsoft アカウント (以前の Live ID) を設定します。 (サービス管理者になっているかわかりませんか。 クリックするとわかります **設定]、[管理者**.)それでは、変更方法です。
    1. 画面の右上隅のユーザーをクリックし、クリックして **明細の表示**します。
    2. サブスクリプションをクリックします。 新規のページで下へスクロールし、をクリックし、 **サブスクリプションの詳細を編集** 右にします。 (正確に言うと、真ん中の一番下の右です。)
    3. ユーザーの Microsoft アカウントを入力します。このユーザーはサービス管理者にする必要があります。

3. ポータルからサインアウトし、前の手順で指定した Microsoft アカウントを使用して再度サインインします。


4. をクリックして **新規アプリ]、[サービス]、[Active Directory Directory]、[ユーザー設定]-> [作成**します。
5.  **ディレクトリ**, 、選択 **既存のディレクトリを使用して**します。 これで、ポータルからサインアウトするように選択したしようとしている **今すぐサインアウトする準備ができました**します。
6. 追加するディレクトリのグローバル管理者としてポータルに再度サインインします。 (グローバル管理者になっていなかった場合、サインインしてサインアウトした後にグローバル管理者になります。)
7. サインインすると、サブスクリプションで既存の AD テナントを使用するか問われます。 クリックして **続行**, 、] をクリックし、 **今すぐサインアウト**します。
5. ここでもにもう一度サインインしに戻る **設定]、[サブスクリプション**します。 サブスクリプションを選択し、クリックして **ディレクトリの編集**します。 使用する Azure AD テナントを選択します。



これで新しい Azure AD テナントを使用し、Azure サブスクリプションへのアクセスを制御したり、Azure RemoteApp 内のユーザー アクセスを構成したりできます。

