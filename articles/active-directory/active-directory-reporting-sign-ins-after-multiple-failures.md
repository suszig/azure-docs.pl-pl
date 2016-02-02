<properties
    pageTitle="複数のエラー後のサインイン"
    description="サインイン試行に何度も連続して失敗した後に正常にサインインしたユーザーを示すレポート。"
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2015"
    ms.author="saah;kenhoff"/>


# 複数のエラー後のサインイン

このレポートは、サインイン試行に何度も連続して失敗した後に、正常にサインインしたユーザーを示します。原因が考えられます: <ul><li>ユーザーがパスワードを忘れた</li><li>ユーザーは、正しいパスワードを推測ブルート フォース攻撃の被害者になって</li></ul><p>このレポートの結果には表示数を連続して失敗したサインインの試行に成功したサインインする前に、最初の成功したサインインに関連付けられたタイムスタンプ</p><p><b>レポート設定</b>: レポートに表示されることができる前に行う必要のある回連続して失敗したサインインの最小数を構成することができます。この設定を変更する場合、その変更は、既存のレポートに現在表示されている失敗した既存のサインインには適用されないことに注意してください。変更は、すべての今後のサインインに適用されます。このレポートの変更は、ライセンスを付与された管理者がのみ行うことができます。


![複数のエラー後のサインイン](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)





