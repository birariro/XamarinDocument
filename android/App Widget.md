# App Widget
App Widget의 제약 조건 이나 기능들을 알아보고<br/>
구현해볼것이다.<br/>



# 구현

## 1. App Widget Layout
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    ...
    ...
    ...
</LinearLayout>
```
위젯또한 다른 엑티비티와 같이 Layout 밑에 xml 파일로 정의한다.<br/>
위젯의 Layout 는 RemoteViews를 기반으로 하기때문에 어느정도 제약이 있다.<br/>
Layout는 Frame , Linear, Relative, Grid 만 가능하다.<br/>

또한 구성 요소로는 기본적인 Button, ImageButton, ListView, GridView, AnalogClock 등 가능한것이 제약이있으니 확인하고 사용해야한다.<br/>


## 2. provider
```xml
<?xml version="1.0" encoding="utf-8"?>
<appwidget-provider
    android:initialLayout="@layout/widget"
    android:minWidth="146dp"
    android:minHeight="72dp"
    android:updatePeriodMillis="86400000"
    android:resizeMode="horizontal|vertical"
    xmlns:android="http://schemas.android.com/apk/res/android" />
```
Widget_provider.xml 은 Resources/xml 폴더 밑에 생성한다.<br/>

이중 위젯의 사이즈를 구하는 부분이 중요한 부분인데.<br/>
한 칸을 74dp로 잡고 구하게 되고 다 구한 값에서 위젯의 가장자리를 빼기 위해 2dp를 뺸값으로 넣는다.<br/>
즉 가로 74는 한 칸이고 거기서 가장자리 2dp를 뺀 72가 한 칸이고<br/>
세로 148은 두 칸이고 거기서 가장자리 2dp를 뺀 146이 두 칸이다.<br/>

## 3. AppWidget.cs
```C#
namespace XamarinWidget.Droid
{
    [BroadcastReceiver(Label = "위젯이름")]
    [IntentFilter(new string[] { "android.appwidget.action.APPWIDGET_UPDATE"
                                , "android.appwidget.action.APPWIDGET_DELETED"
                                , "android.appwidget.action.APPWIDGET_DISABLED"
                                , "android.appwidget.action.APPWIDGET_ENABLED"
                                , "android.appwidget.action.APPWIDGET_UPDATE_OPTIONS"
                                , "android.action.AnnouncementClick"
                                , "android.action.REFRESH_ACTION"})]
    [MetaData("android.appwidget.provider", Resource = "@xml/appwidgetprovider")]
    public class AppWidget : AppWidgetProvider
    {
        public static string REFRESH_ACTION = "android.action.Refresh_ACTION";
        private static string CLICK_ACTION = "android.action.Click_ACTION";
        public override void OnUpdate(Context context, AppWidgetManager appWidgetManager, int[] appWidgetIds)
        {
           
            var me = new ComponentName(context, Java.Lang.Class.FromType(typeof(AppWidget)).Name);
            appWidgetManager.UpdateAppWidget(me, BuildRemoteViews(context, appWidgetIds)); 

        }


  
        private RemoteViews BuildRemoteViews(Context context, int[] appWidgetIds)
        {
            
            var widgetView = new RemoteViews(context.PackageName, Resource.Layout.widget);
         
            SetTextViewText(widgetView, null);
            RegisterClicks(context, appWidgetIds, widgetView);
			
            return widgetView;
        }


        private void SetTextViewText(RemoteViews widgetView, string[] widgetData)
        {
            widgetView.SetTextViewText(Resource.Id.widgetOrderCount, "1");
            widgetView.SetTextViewText(Resource.Id.widgetQuestionCount, "2"); 
            widgetView.SetTextViewText(Resource.Id.widgetClaimCount, "4"); 

        }

        private void RegisterClicks(Context context, int[] appWidgetIds, RemoteViews widgetView)
        {
            /*  
            var intent = new Intent(context, typeof(AppWidget));
            intent.SetAction(AppWidgetManager.ActionAppwidgetUpdate);
            intent.PutExtra(AppWidgetManager.ExtraAppwidgetIds, appWidgetIds);
            var piBackground = PendingIntent.GetBroadcast(context, 0, intent, PendingIntentFlags.UpdateCurrent);
            widgetView.SetOnClickPendingIntent(Resource.Id.Refreshicon, piBackground);*/


            widgetView.SetOnClickPendingIntent(Resource.Id.Refreshicon, GetPendingSelfIntent1(context, appWidgetIds));

            // Register click event for the Announcement-icon
            widgetView.SetOnClickPendingIntent(Resource.Id.widgetBackground, GetPendingSelfIntent(context, Resource.Id.widgetBackground));


        }


        public override void OnRestored(Context context, int[] oldWidgetIds, int[] newWidgetIds)
        {
            base.OnRestored(context, oldWidgetIds, newWidgetIds);
        }

        private PendingIntent GetPendingSelfIntent(Context context, int id)
        {
            var intent = new Intent(context, typeof(AppWidget));
            intent.SetAction(CLICK_ACTION);
            intent.PutExtra("viewId", id);
            return PendingIntent.GetBroadcast(context, id, intent, 0);
        }
        private PendingIntent GetPendingSelfIntent1(Context context, int[] appWidgetIds)
        {
            var intent = new Intent(context, typeof(AppWidget));
            intent.SetAction(REFRESH_ACTION);
            intent.PutExtra("appWidgetIds", appWidgetIds);
            return PendingIntent.GetBroadcast(context, 0, intent, 0);
        }

        public override void OnReceive(Context context, Intent intent)
        {
            base.OnReceive(context, intent);


            var packageManager = context.PackageManager;
            Console.WriteLine("Action :" + intent.Action);

            string action = intent.Action;
            if (action.Equals(CLICK_ACTION))
            {

            }
            else if (action.Equals(REFRESH_ACTION))
            {

            }

        }
    }
}
```


Xamarin 특성상 IntentFilter을 Class 파일 에 기입한다.
또한
```C#
widgetView.SetOnClickPendingIntent(Resource.Id.Refreshicon, GetPendingSelfIntent1(context, appWidgetIds));
...
this.OnUpdate(context, AppWidgetManager.GetInstance(context), ids);
```
위와같이 브로드캐스트를 받아 업데이트를 처리해줘도 되고
```C#
var intent = new Intent(context, typeof(AppWidget));
intent.SetAction(AppWidgetManager.ActionAppwidgetUpdate);
intent.PutExtra(AppWidgetManager.ExtraAppwidgetIds, appWidgetIds);
var piBackground = PendingIntent.GetBroadcast(context, 0, intent, PendingIntentFlags.UpdateCurrent);
widgetView.SetOnClickPendingIntent(Resource.Id.Refreshicon, piBackground);
```
Flags를 통해 처리할수도있다.
