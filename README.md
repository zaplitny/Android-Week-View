Android Week View
=================

**Android Week View** is an Android library for displaying highly customizable calendar views within your app.

![](images/screen-shot-new.png)

Features
---------
* Display single- or multi-day calendar views in your app
* Extensive styling customization possible
* Live preview in Android Studio’s layout editor
* Infinite horizontal scrolling
* Interactive via click and scroll listeners
* Interoperable with Kotlin

Usage
---------
1. Add the JitPack repository to your project-level build file and the dependency to the module-level build file.
```groovy
// build.gradle (project-level)
allprojects {
 repositories {
  // ...
  maven { url 'https://jitpack.io' }
 }
}

// build.gradle (app-level)
implementation 'com.github.thellmund:Android-Week-View:3.4.2'
```

2. Add `WeekView` in your XML layout.
```xml
<com.alamkanak.weekview.WeekView
    android:id="@+id/weekView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:defaultEventColor="@color/primaryColor"
    app:eventTextColor="@color/white"
    app:timeColumnTextSize="12sp"
    app:hourHeight="60dp"
    app:timeColumnPadding="8dp"
    app:timeColumnTextColor="@color/light_blue"
    app:timeColumnBackgroundColor="@color/white"
    app:headerRowPadding="12dp"
    app:columnGap="8dp"
    app:numberOfVisibleDays="3"
    app:headerRowBackgroundColor="@color/light_gray"
    app:dayBackgroundColor="@color/white"
    app:todayBackgroundColor="@color/light_blue"/>
```

3. Prepare the class of objects that you want to display in `WeekView` by implementing `WeekViewDisplayable<T>`.
```kotlin
data class Event(
        val id: Long,
        val title: String,
        val startTime: Calendar,
        val endTime: Calendar,
        val location: String,
        val color: Int,
        val isAllDay: Boolean,
        val isCanceled: Boolean
) : WeekViewDisplayable<Event> {

    override fun toWeekViewEvent(): WeekViewEvent<Event> {
        val style = WeekViewEvent.Style.Builder()
                .setBackgroundColor(color)
                .setTextStrikeThrough(isCanceled)
                .build()

        return WeekViewEvent.Builder<Event>()
                .setId(id)
                .setTitle(title)
                .setStartTime(startTime)
                .setEndTime(endTime)
                .setLocation(location)
                .setAllDay(isAllDay)
                .setStyle(style)
                .setData(this) // This is necessary for onEventClick(data) to work
                .build()
    }

}

```

4. Configure `WeekView` in code.
```kotlin
val weekView = findViewById<WeekView>(R.id.weekView)
weekView.setOnEventClickListener { calendarItem, eventRect ->
    // ...
}

// WeekView has infinite horizontal scrolling. Therefore, you need to provide the events 
// of a month whenever that the currently displayed month changes.
weekView.setMonthChangeListener { startDate, endDate ->
    database.getCalendarItemsInRange(startDate, endDate)
}
```

--- 

Check out the [wiki](https://github.com/thellmund/Android-Week-View/wiki) for more information about customization options, available listeners and public methods.

Take a look at the [sample app](https://github.com/thellmund/Android-Week-View/tree/develop/sample) for more details on how to use `WeekView`.

See the [changelog](https://github.com/thellmund/Android-Week-View/blob/develop/CHANGELOG.md) for new functionality and updates to the API.

---

You can find the [original README](https://github.com/alamkanak/Android-Week-View) here.

License
----------

    Copyright 2014 Raquib-ul-Alam

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
