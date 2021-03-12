# SyncFusionListViewHeader 접근

보통 ListView의 Header는 x:Name으로 바로 접근이 가능하다.

하지만 SyncfusionListview 는 Header가 Template로 관리되기 때문에 x:Name으로 접근이 불가능하다.
이벤트로 접근이 가능은 하나 이벤트를 쓰지 않고 최상의 Listview로 HeaderTemplate를 가지고오는 방법이다.

```

var ListViewinHeader = listView.Children.Where(x => x.ToString() == "Syncfusion.ListView.XForms.ExtendedScrollView").FirstOrDefault() //extendedscrollview
             .LogicalChildren.Where(x => x.ToString() == "Syncfusion.ListView.XForms.VisualContainer").FirstOrDefault() //visualContainer
             .LogicalChildren.Where(x => x.ToString() == "Syncfusion.ListView.XForms.HeaderItem").FirstOrDefault() //headeritem
             
             //여기서부터 View in Header 시작
             
```

이와 마찬가지로 여러 라이브러리를 혼용하여 쓸경우엔 가장 하위 View에 이벤트를걸고 모든 Parent를 찾아가며 그 경로에 따라 Children을 찾아가면 된다.

