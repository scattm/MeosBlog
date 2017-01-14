[Bài tiếng  Việt](http://meonhun.com/post/155431741009/thủ-thuật-về-ngày-trên-sql-server)

This article provide some queries that will help you get special date values:

|**Value**  |**Query**|
|---|---|
|First date of month |*SELECT CAST(DATEADD(m, DATEDIFF(m, 0, GETDATE()), 0) AS DATE)*  |
|Last date of month  |*SELECT CAST(DATEADD(d, -1, DATEADD(m, DATEDIFF(m, -1, GETDATE()), 0)) AS DATE)* |
|First date of year |*SELECT CAST(DATEADD(yy, DATEDIFF(yy, 0, GETDATE()), 0) AS DATE)*  |
|First date of uyear  |*SELECT CAST(DATEADD(d, -1, DATEADD(yy, DATEDIFF(yy, -1, GETDATE()), 0)) AS DATE)* |
