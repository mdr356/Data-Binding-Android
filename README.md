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

## ViewModel
We are using a viewmodel as the data holder, and it will be use to populate the UI.
```kotlin
class MainViewModel : ViewModel() {
    var welcomeMsg : MutableLiveData<String> = MutableLiveData()
    var btnTitle  : MutableLiveData<String> = MutableLiveData()

    init {
        setMessage()
    }

    private fun setMessage() {
        welcomeMsg.value = "Welcome"
        btnTitle.value = "Click Me"
    }

    fun btnClicked() {
        Log.d("MainViewModel", "Button Clicked")
    }
}
```

 ## Data Binding | XML
Data Binding is done declaratively rather than programmatically. Inside the XML file we add the layout tag as the parent ViewHolder, add the <data> tag with the variables containing that whill provide data to the view of our choice.

**main_activity.xml**
    
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
### Binding
*android:text="@{viewmodel.welcomeMsg}"*
*android:text="@{viewmodel.btnTitle}"*

**These variable welcomeMsg, btnTitle, are connected with the view and any changes would be reflected in the view.**

### Two-way data binding
*android:onClick="@{() -> viewmodel.btnClicked()}"*
Data Binding is able to Receive data changes, and listen to user updates/inputs.

## Data Binding | Activity
```kotlin

class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        binding = DataBindingUtil.setContentView(
             this, R.layout.activity_main)

         setSupportActionBar(binding.toolbar)
         binding.viewmodel = ViewModelProvider(this).get(MainViewModel::class.java);
        
    }
    ...
}
```
