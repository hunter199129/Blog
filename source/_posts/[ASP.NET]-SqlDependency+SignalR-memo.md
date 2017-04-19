---
title: '[ASP.NET] SqlDependency+SignalR memo'
date: 2017-04-19 10:07:00
tags: [ASP.NET, SqlDependency, SignalR, Work]
---
近期老闆想弄一個像是**Dashboard**和**Messenger**的服務，所以我就趁勢研究了一下這東西在asp.net中該怎麼做，主要是透過兩個asp.net的元件: 

* [SqlDependency](https://msdn.microsoft.com/zh-tw/library/62xk7953(v=vs.110).aspx): 用Sql**監視**DB中的資料，若資料變更則觸發動作。

* [SignalR](https://www.asp.net/signalr): Microsoft專為asp.net出的real-time的連線框架，簡化WebSocket, long-polling等real-time方法的連線過程。且會找到client端能用的最佳連線方式，若client端browser支援WebSocket就用WebSocket，否則就用long-polling。

實作的是一個登入就會通知的WebApp。登入時Sql的資料就會變動，這時候就可以藉由LoginTime的update當作SqlDependency的Trigger，在透過SignalR傳送通知到Web browser。

#SqlDependency

先附上一小段實作的code
    
`Default.aspx.cs`

    public static void SqlListener()
        {
            string sql = @"select UserID, LoginTime from dbo.UserTable";
            using (SqlConnection conn = new SqlConnection(connectionString))
            {
                conn.Open();
                using (SqlCommand command = new SqlCommand(sql, conn))
                {
                    try
                    {
                        SqlDependency dependency = new SqlDependency(command);

                        dependency.OnChange += new OnChangeEventHandler(NotificationFunc);

                        if (conn.State == System.Data.ConnectionState.Closed)
                            conn.Open();
                        SqlDataReader reader = command.ExecuteReader();
                        conn.Close();
                    }
                    catch (Exception ex)
                    {

                    }
                }
            }
        }
        

     static void NotificationFunc(object sender, SqlNotificationEventArgs e)
        {
            MyHub1 nHub = new MyHub1();
            
            string sql = "select top 1 UserID, LoginTime from dbo.UserTable order by login_t desc";
            
            using (SqlConnection conn = new SqlConnection(connectionString))
            {
                conn.Open();
                using (SqlCommand command = new SqlCommand(sql, conn))
                {
                    SqlDataReader reader = command.ExecuteReader();

                    while (reader.Read())
                    {
                        nHub.Send(String.Format("User: {0}登入! 登入時間為: {1}", reader.GetString(0), reader.GetDateTimeOffset(1)));
                    }
                }
            }

            //要再呼叫一次SqlListener();
            SqlListener();
        }

* 要用SqlDependency前一定要看DB有沒有開啟Service Broker設定

      SELECT name, is_broker_enabled FROM sys.databases

  若沒有開就用下面的Sql指令打開

      ALTER DATABASE DBNAME SET ENABLE_BROKER

  若速度太慢，可以加上`WITH ROLLBACK IMMEDIATE`，但要注意若這時有在做Transaction就會被取消。

* 要監視的Sql command不能用*去做篩選，否則會無止境刷新，但是可以用全部的欄位。

* Sql用的Table一定要是完整名稱，即`dbo.UserTable`。

* 在`OnChangeEventHandler`的函式要再去呼叫這支監視程式。

# SignalR

`MyHub1.cs`

    public class MyHub1 : Hub
    {
        public void Send(string message)
        {
            IHubContext context = GlobalHost.ConnectionManager.GetHubContext<MyHub1>();
            context.Clients.All.GetMessage(message);
        }
    }

`Startup.cs`

    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            app.MapSignalR();
        }
    }

`Default.aspx`

    <script type="text/javascript">
        $(function () {
            var notification = $.connection.myHub1;
            //debugger;
            notification.client.getMessage = function (message) {
                document.getElementById('text').innerHTML += message + "<br>";
            };
            $.connection.hub.start().done(function () {
                notification.server.send("Hello");
            });
        });
    </script>

SignalR的部分沒什麼特別的，就不多說。

`Global.asax.cs`

        protected void Application_Start(object sender, EventArgs e)
        {
            System.Data.SqlClient.SqlDependency.Start(connectionString);
            
            //若在Page_Load呼叫，reload後會重複建立好幾個SqlDependency物件，就會有重覆通知的bug
            WebForm1.SqlListener();
        }
        protected void Application_End(object sender, EventArgs e)
        {
            System.Data.SqlClient.SqlDependency.Stop(connectionString);

        }

這邊就是設定當WebApp打開的時候要開啟監視，關閉時要停止。

值得一提的是，若把SqlListener()函式在Page_Load時就呼叫，會有一個BUG，就是Reload過後會產生多個SqlDependency物件，當觸發一次登入條件時，會產生多個通知訊息。

所以這邊才要用`Static`把它包起來，丟到`Application_Start`裡面執行，這樣就能確保他只有一個物件會產生。

總之這個練習就是熟悉一下SignalR跟SqlDependency的運作，現在比較了解是要怎麼實作了，不然這兩個東西網路上資源實在不好懂...。

## Ref: 

- [Database Change Notifications in ASP.NET using SignalR and SqlDependency](http://techbrij.com/database-change-notifications-asp-net-signalr-sqldependency)

- [Terry 筆記本 - [C#] 監聽資料庫更新狀態 (SqlDependency)](http://terryweng2050.blogspot.tw/2016/08/c-sqldependency-change.html)

- [Youtube - Database Change Notifications in ASP NET using SignalR and SqlDependency](https://www.youtube.com/watch?v=30m-7wpmbrc)

- [使用 SqlDependency 來偵測變更](https://msdn.microsoft.com/zh-tw/library/62xk7953(v=vs.110).aspx)

- [Raymond Chu .NET - SignalR 與 SqlDependency 建立ASP.NET realtime應用
](https://dotblogs.com.tw/hznraymond/2014/01/24/142134)
