<properties 
    pageTitle="Stream Analytics: 入力と出力のログイン資格情報の交換 |Microsoft Azure" 
    description="Stream Analytics の入力と出力の資格情報を更新する方法について説明します。"
    keywords="login credentials"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="12/04/2015" 
    ms.author="jeffstok"/>

#ログイン資格情報の交換

##要約
現在、Azure Stream Analytics では、ジョブの実行中に入力/出力の資格情報を置き換えることができません。

Azure Stream Analytics では、最後の出力からのジョブの再開がサポートされていますが、ジョブを停止して開始し、ログイン資格情報を交換するまでの遅延を最小限に抑えるためのプロセス全体を共有することを考えました。

##パート 1: 新しい資格情報セットの準備
このパートは、次の入力/出力に適用できます。

* BLOB ストレージ
* Event Hubs
* SQL Database
* テーブル ストレージ

その他の入力/出力の場合は、パート2 へ進んでください。

###BLOB ストレージとテーブル ストレージ
1.  Azure 管理ポータルでストレージ] 拡張に移動します。  
![図 1][graphic1]
2.  ジョブで使用されるストレージを見つけて、そこに移動します。  
![図 2][graphic2]
3.  アクセス キーの管理コマンドをクリックします。  
![図 3][graphic3]
4.  [プライマリ アクセス キーとセカンダリのアクセス キーの間で **選択ジョブで使用されていない**します。
5.  再生成をヒットします。  
![図 4][graphic4]
6.  新しく生成されたキーをコピーします。  
![図 5][graphic5]
7.  パート 2 に進みます。

###Event Hubs
1.  Azure 管理ポータルで、Service Bus] 拡張に移動します。  
![図 6][graphic6]
2.  ジョブで使用される Service Bus 名前空間を見つけて、そこに移動します。  
![図 7][graphic7]
3.  ジョブが Service Bus 名前空間で共有アクセス ポリシーを使用している場合は、手順 6. に進みます。  
4.  イベント ハブ] タブに移動します。  
![図 8][graphic8]
5.  ジョブで使用されているイベント ハブを見つけて、そこに移動します。  
![図 9][graphic9]
6.  [構成] タブに移動します。  
![図 10][graphic10]
7.  [ポリシー名] の一覧で、ジョブによって使用される共有アクセス ポリシーを見つけます。  
![図 11][graphic11]
8.  主キーとセカンダリ キー間 **選択ジョブで使用されていない**します。  
9.  再生成をヒットします。  
![図 12][graphic12]
10. 新しく生成されたキーをコピーします。  
![図 13][graphic13]
11. パート 2 に進みます。  

###SQL Database

>[AZURE.NOTE] 注: SQL Databse サービスに接続する必要があります。 Azure の管理ポータルで管理機能を使用して実行する方法を説明しますが、SQL Server Management Studio などのクライアント側のツールを使用することもできます。

1.  Azure 管理ポータルで、SQL データベース] 拡張に移動します。  
![図 14][graphic14]
2.  ジョブで使用される SQL データベースを検索し、 **サーバーをクリックして** 同じ行にリンクします。  
![graphic15][graphic15]
3.  管理コマンドをクリックします。  
![図 16][graphic16]
4.  データベース Master」と入力します。  
![図 17][graphic17]
5.  ユーザー名、パスワードを入力し、[ログ] をクリックします。  
![図 18][graphic18]
6.  [新しいクエリ] をクリックします。  
![図 19][graphic19]
7.  < Login_name > をユーザー名に置き換えると、置換は、次のクエリを入力します。 <enterStrongPasswordHere> 新しいパスワード。  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  [実行] をクリックします。  
![図 20][graphic20]
9.  戻る手順 2 およびこの時間をクリックして、データベース。  
![図 21][graphic21]
10. 管理コマンドをクリックします。  
![図 22][graphic22]
11. ユーザー名、パスワードを入力し、ログ] をクリックします。  
![図 23][graphic23]
12. [新しいクエリ] をクリックします。  
![図 24][graphic24]
13. < User_name > を (たとえば < login_name > の指定した同じ値を提供することができます)、このデータベースのコンテキストでこのログインを識別する名前に置き換え、< login_name > を新しいユーザー名に置き換えて、次のクエリを入力します。  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. [実行] をクリックします。  
![図 25][graphic25]
15. ここで、元のユーザーと同じロールと権限を持つ新しいユーザーを指定する必要があります。
16. パート 2 に進みます。

##パート 2: Stream Analytics ジョブの停止
1.  Azure 管理ポータルで、Stream Analytics] 拡張に移動します。  
![図 26][graphic26]
2.  ジョブを見つけ、そこに移動します。  
![図 27][graphic27]
3.  入力タブまたはでは、入力または出力で資格情報を交換するかどうかに基づいて [出力] タブに移動します。  
![図 28][graphic28]
4.  停止コマンドをクリックし、ジョブが停止したことを確認します。  
![図 29][graphic29]
ジョブを停止するを待ちます。
5.  資格情報の交換し、そこに移動する入力/出力を見つけます。  
![図 30][graphic30]
6.  パート 3 に進みます。

##パート 3: Stream Analytics ジョブの資格情報の編集

###BLOB ストレージとテーブル ストレージ
1.  ストレージ アカウント キー] フィールドを見つけ、新しく生成されたキーを貼り付けます。  
![図 31][graphic31]
2.  [保存] コマンドをクリックし、変更の保存を確定します。  
![図 32][graphic32]
3.  変更内容を保存すると、接続テストが自動的に開始され、テストに成功したことを確認します。
4.  パート 4 に進みます。

###Event Hubs
1.  イベント ハブ ポリシー キー] フィールドを見つけ、新しく生成されたキーを貼り付けます。  
![図 33][graphic33]
2.  [保存] コマンドをクリックし、変更の保存を確定します。  
![図 34][graphic34]
3.  変更内容を保存すると、接続テストが自動的に開始され、テストに成功したことを確認します。
4.  パート 4 に進みます。

###Power BI
1.  [承認の更新] をクリックします。  
* ![図 35][graphic35]
* 次の確認メッセージが表示されます。  
* ![図 36][graphic36]
2.  [保存] コマンドをクリックし、変更の保存を確定します。  
![図 37][graphic37]
3.  変更内容を保存すると、接続テストが自動的に開始され、テストに成功したことを確認します。
4.  パート 4 に進みます。

###SQL Database
1.  ユーザー名とパスワードのフィールドを検索し、新しく作成された資格情報セットをそこに貼り付けます。  
![図 38][graphic38]
2.  [保存] コマンドをクリックし、変更の保存を確定します。  
![図 39][graphic39]
3.  変更内容を保存すると、接続テストが自動的に開始され、テストに成功したことを確認します。  
4.  パート 4 に進みます。

##パート 4: 最後に停止した時刻からのジョブの開始
1.  入力/出力から移動します。  
![図 40][graphic40]
2.  Start コマンドをクリックします。  
![図 41][graphic41]
3.  最終停止時刻を選択し、[ok] をクリックします。  
 ![図 42][graphic42]
4.  パート 5 に進みます。  

##パート 5: 以前の資格情報セットの削除
このパートは、次の入力/出力に適用できます。
* BLOB ストレージ
* Event Hubs
* SQL Database
* Table Storage

###BLOB ストレージとテーブル ストレージ
ジョブで以前に使用されていたアクセス キーに対してパート 1 を繰り返し、現在未使用のアクセス キーを更新します。

###Event Hubs
ジョブで以前に使用されていたキーに対してパート 1 を繰り返し、現在未使用のキーを更新します。

###SQL Database
1.  パート 1 の手順 7. および < previous_login_name > をジョブで使用されていたユーザー名に置き換えて次のクエリでは型からクエリ ウィンドウに戻り。  
`DROP LOGIN <previous_login_name>`  
2.  [実行] をクリックします。  
    ![図 43][graphic43]  

次の確認メッセージが表示されます。 

    Command(s) completed successfully.

## 問い合わせ
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 


