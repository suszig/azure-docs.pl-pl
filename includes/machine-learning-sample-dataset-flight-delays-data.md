米国運輸省の TranStats データ コレクションから取得した旅客機の定時運航データ (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">定刻</a>)。<p></p>データセットでは、2013 年 4 月から 10 月の期間について説明します。Azure ML Studio にアップロードする前に、データセットの次のように処理されました:<ul><li>米国本土の 70 位まで空港だけをカバーするデータセットはフィルタ リングされ</li><li>Cancelled フライトは 15 分以上の遅延として分類</li><li>Diverted フライトはフィルターで外さ</li><li>次の列を選択: 年、月、DayofMonth、DayOfWeek、Carrier、OriginAirportID、DestAirportID、CRSDepTime、DepDelay、DepDel15、CRSArrTime、ArrDelay、ArrDel15、取り消されました</li></ul>




