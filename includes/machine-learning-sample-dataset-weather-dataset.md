Noaa の 1 時間ごとの陸上の気象観測 (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">陸上 201304 からデータをマージ</a>)。<p></p>気象データには、航空気象観測、2013 年 4 月から 10 月の期間の観測が含まれます。 Azure ML Studio にアップロードする前に、データセットの次のように処理されました:<ul><li>気象観測ステーション Id が対応する空港 Id にマップされて</li><li>70 位までの空港に関連付けられていない気象観測所はフィルターで外さ</li><li>日付列が独立した年、月、および日の列に分割されました</li><li>次の列を選択: AirportID、年、月、日、時、TimeZone、SkyCondition、可視性、WeatherType、DryBulbFarenheit、DryBulbCelsius、WetBulbFarenheit、WetBulbCelsius、DewPointFarenheit、DewPointCelsius、RelativeHumidity、WindSpeed、WindDirection、ValueForWindCharacter、StationPressure、PressureTendency、PressureChange、SeaLevelPressure、RecordType、HourlyPrecip、Altimeter</li></ul>




