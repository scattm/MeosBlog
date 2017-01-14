[English artical here](http://meonhun.com/post/155476713549/sql-server-tricks-for-special-date)

Khi đang sử dụng SQL Server, đôi lúc bạn sẽ cần đến những giá trị ngày tháng đặc biệt như ngày đầu tháng, đầu năm. Bài viết sau sẽ giúp bạn lấy được một số giá trị qua câu truy vấn.

|**Giá trị**  |**Câu truy vấn**|
|---|---|
|Ngày đầu tháng |*SELECT CAST(DATEADD(m, DATEDIFF(m, 0, GETDATE()), 0) AS DATE)*  |
|Ngày cuối tháng  |*SELECT CAST(DATEADD(d, -1, DATEADD(m, DATEDIFF(m, -1, GETDATE()), 0)) AS DATE)* |
|Ngày đầu năm |*SELECT CAST(DATEADD(yy, DATEDIFF(yy, 0, GETDATE()), 0) AS DATE)*  |
|Ngày cuối năm  |*SELECT CAST(DATEADD(d, -1, DATEADD(yy, DATEDIFF(yy, -1, GETDATE()), 0)) AS DATE)* |
