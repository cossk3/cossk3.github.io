---
layout: post
title: "[Flutter] Flutter 앱에 홈 화면 위젯 추가하기"
categories: ["Language", "Flutter"]
tags: ["flutter", "flutter home widget", "플러터"]
---

flutter 앱에 홈 위젯을 추가해보고 싶은데 막상 예제만 보며 파악하려 하니 이해도 안되고 너무 어려웠다. 그래서 하나씩 차근차근 예제를 따라해보려고 한다.

---

# 위젯이란?
Flutter 앱에서의 위젯은 Flutter 프레임워크를 사용하여 생성된 UI 구성요소이다. 앱을 열지 않고도 앱 정보에 대한 보기를 제공하는 앱의 미니 버전이라고도 볼 수 있다.

# iOS 설정

```
flutter project₩ open ios/Runner.xcworkspace
```
Flutter 프로젝트 경로에서 Xcode를 열어준다.

<p width="100%">
  <img src="https://github.com/KKM2222/todo/assets/44231144/d7328ed0-0a8e-4d28-b252-0015f1e789b9" width="100%">
</p>

위 이미지처럼 `Target` > `+ 버튼` > `Widget Extension` > `Check 해제` 절차로 `Widget Extension`을 추가해준다.

# Android 설정

<p width="100%">
  <img src="https://github.com/KKM2222/todo/assets/44231144/6ead376c-b7d2-47d7-a035-77a79576e156" width="50%">
</p>

위 이미지처럼 3개의 파일을 추가해준다.
```
- res/layout/news_widget.xml
- res/xml/news_widget_info.xml
- java/com/example/.../NewsWidget.kt
```

###### res/layout/news_widget.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/app_widget_description"
    android:initialKeyguardLayout="@layout/news_widget"
    android:initialLayout="@layout/news_widget"
    android:minWidth="180dp"
    android:minHeight="180dp"
    android:previewImage="@drawable/example_appwidget_preview"
    android:previewLayout="@layout/news_widget"
    android:resizeMode="horizontal|vertical"
    android:targetCellWidth="3"
    android:targetCellHeight="3"
    android:updatePeriodMillis="1000"
    android:widgetCategory="home_screen" />
```


추가했다면, `AndroidManifest.xml` 파일에 아래 코드를 추가해준다. 추가할 때 `application` 태그 안에 추가해주기!
```xml
<receiver android:name=".NewsWidget">
    <intent-filter>
        <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
    </intent-filter>

    <meta-data
        android:name="android.appwidget.provider"
        android:resource="@xml/news_widget_info" />
</receiver>
```

# 앱 데이터를 홈 위젯으로 보내기
#### iOS App Groups 추가하기

<p width="100%">
  <img src="https://github.com/KKM2222/todo/assets/44231144/e65f05ce-c7f7-4bae-8f37-977f1a15fb52" width="80%">
</p>

Xcode에서 `Runner Target`과 `NewsWidgetExtension Target` 둘 다 `+ Capability`를 통해서 `App Groups`를 추가해준다. 둘 다 같은 group id여야 한다.

#### home_widget 라이브러리 사용하기

###### pubspec.yaml 파일 수정
```yaml
dependencies:
  flutter:
    sdk: flutter
  home_widget: ^0.3.0
```

###### dart 파일 수정
```dart
import 'package:home_widget/home_widget.dart';

const String appGroupId = 'group.test.newswidget';
const String iOSWidgetName = 'NewsWidgets';
const String androidWidgetName = 'NewsWidget';

...

void updateHeadline(NewsArticle newHeadline) {
  // Save the headline data to the widget
  HomeWidget.saveWidgetData<String>('headline_title', newHeadline.title);
  HomeWidget.saveWidgetData<String>(
      'headline_description', newHeadline.description);
  HomeWidget.updateWidget(
    iOSName: iOSWidgetName,
    androidName: androidWidgetName,
  );
}

...
```
`appGroupId` 변수에 Xcode에서 설정해줬던 App Group Id를 넣어주면 된다.


```dart
class _MyHomePageState extends State<MyHomePage> {
  @override
  void initState() {
    super.initState();

    // Set the group ID
    HomeWidget.setAppGroupId(appGroupId);

    final newHeadline = getNewsStories()[0];
    updateHeadline(newHeadline);
  }

  ...
}
```
그리고 `initState()` 함수에 위에서 생성한 `updateHeadline()` 함수를 선언해준다.

```dart
floatingActionButton: FloatingActionButton.extended(
        onPressed: () {
            ...
            updateHeadline(widget.article);
        },
        label: const Text('Update Homescreen'),
      ),
```
`FloatingActionButton`을 누르면 `updateHeadline()` 함수를 호출할 수 있도록 코드를 작성한다. 이렇게 되면, 버튼을 누를 시 홈 화면 위젯의 데이터가 업데이트된다.

#### iOS 코드 업데이트

<p width="100%">
  <img src="https://github.com/KKM2222/todo/assets/44231144/0b263c68-0974-4312-8fd1-54c21c3af11c" width="50%">
</p>

`Widget Extension`을 생성했다면 위 이미지처럼 위젯 관련 코드가 생성된다.

###### NewsWidgets.swift 파일 수정
```swift
struct NewsArticleEntry: TimelineEntry {
    let date: Date
    let title: String
    let description:String
}
```
`SimpleEntry` 구조체를 `NewsArticleEntry` 구조체로 변경해준다. 위 구조체는 업데이트 시 홈 위젯에 전달할 수신 데이터를 정의한다.

```swift
struct NewsWidgetsEntryView : View {
    var entry: Provider.Entry

    var body: some View {
      VStack {
        Text(entry.title)
        Text(entry.description)
      }
    }
}
```
기존 날짜를 보여주던 View 대신 뉴스 기사 헤드라인과 설명을 표시하도록 위 코드로 수정해준다.

```swift
struct Provider: TimelineProvider {

// Placeholder is used as a placeholder when the widget is first displayed
    func placeholder(in context: Context) -> NewsArticleEntry {
//      Add some placeholder title and description, and get the current date
      NewsArticleEntry(date: Date(), title: "Placeholder Title", description: "Placeholder description")
    }

// Snapshot entry represents the current time and state
    func getSnapshot(in context: Context, completion: @escaping (NewsArticleEntry) -> ()) {
      let entry: NewsArticleEntry
      if context.isPreview{
        entry = placeholder(in: context)
      }
      else{
        //      Get the data from the user defaults to display
          let userDefaults = UserDefaults(suiteName: "group.test.newswidget")
        let title = userDefaults?.string(forKey: "headline_title") ?? "No Title Set"
        let description = userDefaults?.string(forKey: "headline_description") ?? "No Description Set"
        entry = NewsArticleEntry(date: Date(), title: title, description: description)
      }
        completion(entry)
    }

//    getTimeline is called for the current and optionally future times to update the widget
    func getTimeline(in context: Context, completion: @escaping (Timeline<Entry>) -> ()) {
//      This just uses the snapshot function you defined earlier
      getSnapshot(in: context) { (entry) in
// atEnd policy tells widgetkit to request a new entry after the date has passed
        let timeline = Timeline(entries: [entry], policy: .atEnd)
                  completion(timeline)
              }
    }
}
```
`func placeholder` 메서드는 사용자가 홈 위젯을 선택하기 전 미리 보여줄 때의 자리 표시자 항목을 생성한다.
`func getSnapshot` 메서드는 사용자 기본 값에서 데이터를 읽고 현재 시간에 대한 항목을 생성한다.
`func getTimeline` 메서드는 타임라인 항목을 반환한다. 콘텐츠를 업데이트할 예측 가능한 시점이 있을 때 도움이 되는 메서드다. 위 코드에서 사용한 `.atEnd` 메서드는 현재 시간이 지나면 데이터를 새로 고치도록 홈 위젯에 지시한다.

#### Android 코드 업데이트
###### res/layout/news_widget.xml
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:id="@+id/widget_container"
   style="@style/Widget.Android.AppWidget.Container"
   android:layout_width="wrap_content"
   android:layout_height="match_parent"
   android:background="@android:color/white"
   android:theme="@style/Theme.Android.AppWidgetContainer">
   
   <TextView
       android:id="@+id/headline_title"
       style="@style/Widget.Android.AppWidget.InnerView"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_alignParentStart="true"
       android:layout_alignParentLeft="true"
       android:layout_marginStart="8dp"
       android:layout_marginLeft="8dp"
       android:background="@android:color/white"
       android:text="Title"
       android:textSize="20sp"
       android:textStyle="bold" />

   <TextView
       android:id="@+id/headline_description"
       style="@style/Widget.Android.AppWidget.InnerView"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_below="@+id/headline_title"
       android:layout_alignParentStart="true"
       android:layout_alignParentLeft="true"
       android:layout_marginStart="8dp"
       android:layout_marginLeft="8dp"
       android:layout_marginTop="4dp"
       android:background="@android:color/white"
       android:text="Title"
       android:textSize="16sp" />

</RelativeLayout>
```

###### app/java/.../NewsWidget.kt
```kotlin
package com.mydomain.homescreen_widgets

import android.appwidget.AppWidgetManager
import android.appwidget.AppWidgetProvider
import android.content.Context
import android.widget.RemoteViews

import es.antonborri.home_widget.HomeWidgetPlugin

class NewsWidget : AppWidgetProvider() {
    override fun onUpdate(
            context: Context,
            appWidgetManager: AppWidgetManager,
            appWidgetIds: IntArray,
    ) {
        for (appWidgetId in appWidgetIds) {
            // Get reference to SharedPreferences
            val widgetData = HomeWidgetPlugin.getData(context)
            val views = RemoteViews(context.packageName, R.layout.news_widget).apply {

                val title = widgetData.getString("headline_title", null)
                setTextViewText(R.id.headline_title, title ?: "No title set")

                val description = widgetData.getString("headline_description", null)
                setTextViewText(R.id.headline_description, description ?: "No description set")
            }

            appWidgetManager.updateAppWidget(appWidgetId, views)
        }
    }
}
```

# 결과
<p width="100%">
  <img src="https://github.com/KKM2222/todo/assets/44231144/fa27cbc2-00b8-4fcd-b109-acf7929d1acc" width="80%">
</p>

버튼을 누르니 기사 헤드라인 데이터가 홈 위젯으로 잘 보내진다.