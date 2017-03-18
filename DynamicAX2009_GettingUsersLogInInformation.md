Old systems are old, today I was given a task to get AX users last login time. There are not so much information in the internet since this version of Dynamic AX is too old. Here is how you can do it:

    WITH LOGIN_HIS AS
    (
        SELECT
            USERID,
            CREATEDDATETIME LOGINDATETIME,
            LOGOUTDATETIME,
            COMPUTERNAME,
            RN = ROW_NUMBER() OVER (PARTITION BY USERID, COMPUTERNAME ORDER BY CREATEDDATETIME DESC)
        FROM SYSUSERLOG
    )
    SELECT
        ID USERID,
        [NAME],
        COMPUTERNAME,
        LOGINDATETIME LASTLOGINDATETIME,
        LOGOUTDATETIME LASTLOGOUTDATETIME
    FROM USERINFO ui
        JOIN LOGIN_HIS lh
            ON ui.ID = lh.USERID
                AND lh.RN = 1

The query also provide you the computer name of logged in users. If you want to look further, maybe you will interest in `SYSCLIENTSESSIONS` or `SYSSERVERSESSIONS`.