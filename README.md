# Data-Binding-Android

Data binding is the process of binding or establishing a connection between the app UI and the data being displayed. If done right, the UI would automatically update to reflect changes whenever the underlying data changes. Data Binding is done declaratively rather than programmatically. 

## Enable Data Binding | Gradle

add the following tt the top of build.gradle (app)


```kotlin
apply plugin: 'kotlin-kapt'

android {
    ...
    dataBinding {
        enabled = true
    }
    ...
    
    dependencies {
        ...
        kapt "com.android.databinding:compiler:<android_plugin_version>"
        ...
    }
}
```
Once you have enabled data binding, a binding class is generated for each layout file. The name of the class is base on the name of the layout file.

i.e: 
- main_activity -> MainActivityBinding::class.java
- fragment_first -> FragmentFirstBinding::class.java

The generated class, holds all the bindings from the layout properties

 ## Data Binding | Activity
 Data Binding is done declaratively rather than programmatically. Inside the XML file we add the layout tag as the parent ViewHolder, add the <data> tag with the variables containing that whill provide data to the view of our choice.
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
        <variable
            name="viewmodel"
            type="com.marcdanielregistre.MainViewModel" />
    </data>
  
<LinearLayout
    ...>
    <TextView
        ...
        android:text="@{viewmodel.welcomeMsg}"
        />
    
    <Button
        ...
        android:text="@{viewmodel.btnTitle}"
        android:onClick="@{() -> viewmodel.btnClicked()}"
        />
</LinearLayout>
```
## Data Binding | Fragment
