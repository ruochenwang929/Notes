# Week 3 - Android Views and Intent

## Outline

- Views and Event Handling (Button example)
- View Binding
- Kotlin
- Intent and Multiple Activities
- Bundle
- Parcelable

## Events and Event Listeners

- Events are created from interactions with view objects like clicking a button
- Event Listeners can capture user interactions with UI
- An event listener is an interface in the View class
- An event listener contains a single callback method
  - Callback methods will be called when the View to which the listener has been registered is triggered by user interaction with the UI item
- View.OnClickListener is an event listener and its callback method is onClick()

https://developer.android.com/reference/android/view/View.OnClickListener

## View Binding

- View Binding was introduced recently by Android Jetpack
- It automatically creates a binding class for each XML layout file
- The auto-generated binding class implements ViewBinding
- ViewBinding is an interface that binds the views (e.g. a button) in a layout XML to their declared names (e.g. addButton)
- It provides an easier way to work with UI elements (views) instead of using findViewById
- To use View Binding, you must set view binding to true in the module level gradle file

## View Binding (cont’d)

Step 1: declare a private variable based on the name of the XML file using the PascalCase and add ‘Binding’ at the end, e.g. activity_main.xml becomes ActivityMainBinding

`private ActivityMainBinding binding;`

Step 2: create an instance of the binding class by invoking the static inflate() method to inflate the layout XML file (activity_main.xml) and create view objects from it

`binding = ActivityMainBinding.inflate(getLayoutInflater());`

Step 3: get a reference to the root view and pass it to setContentView() to make it the active view on the screen

    View view = binding.getRoot();
    setContentView(view);

Step 4: now we can access any views without using findViewById()

`binding.editMessage.setText("");`

https://developer.android.com/topic/libraries/view-binding

## View Binding Advantages

The advantages of View Binding over using findViewById():

- Reduces the boilerplate code
- Null Safety - View binding creates direct references to views so it eliminates the risk of a null pointer exception
- Type safety: The fields in each binding class have types matching the views, so it avoids a class cast exception

## Kotlin Properties

Properties in Kotlin classes are declared as var or val

Property Type is optional if it can be inferred from the initializer

E.g. var num = 1

- num has type Int and it is inferred here
- Kotlin’s default getter and setter

**var** is used for mutable properties, e.g. var message: String=“Hello"

**val** is used for read-only properties e.g. val message: String=“Hello"

Properties must be initialized or custom accessors must be provided

## Null Safety (Nullable types vs Non-null Types)

- Kotlin’s null safety aims to eliminate NullPointerException (NPE) of Java

- In Kotlin, a regular property cannot hold null (**non-null** types)

        var message: String=“Hello"
        message= null //generates an error at compile time

- To allow nulls and declare a property as **nullable**, we use ‘?’

        var message: String? =“Hello”
        message = null //it is now OK

  - When using nullable references:
  - Option 1: check for null in condition, e.g. if (message != null)
  - Option 2: use a safe call by using this symbol **?.**
  `println(message?.length)`
  - Option 3: Use the !! operator to convert any value to a non-null type and throw an exception if the value is null (behaves the same as Java)
  `val l = b!!.length`

## Lateinit Modifier

The **lateinit** modifier can be used with properties **declared as non-null** so they can be initialized later

- It can be only used on mutable (**var**) properties (not used with val)
- It **cannot** be used with a **primitive type**
- E.g. `private **lateinit** var binding: ActivityMainBinding`

## Kotlin Classes

- The primary constructor of a class can be part of the class header class
`class Person(var value: Double) { /*...*/ }`
- The primary constructor cannot contain any code so the initialization code is placed in initializer blocks, e.g. `**init**{ if(value<1) value=1.0 }`
- By default, Kotlin classes are final. To make a class inheritable, use the **open** keyword e.g. `**open** class Person`
- A single colon character ( : ) instead of the Java **extends** keyword
- No need for the **new** keyword, and to create a reference to an anonymous inner class, it uses **‘object’**
- In Kotlin,all methods are functions (fun)
- A colon **:** is used in the function for the return type, e.g. fun doubleIt(x: Int): Int { ...}

For more information refer to: https://kotlinlang.org/docs/reference/

## Multiple Activities

- Android applications can use multiple activities and multiple fragments
- When multiple activities are used, the first activity (MainActivity) starts the second activity using an **Intent**
- An Intent provides runtime binding between separate components, such as two activities

## Intent

- An Intent can be used to **start an activity**

  - The Intent constructor takes two parameters, a Context and a Class

    At the activity level:
    `Intent intent = new Intent(this, SecondActivity.class);`
    Inside the View block:

        Intent intent = new Intent(MainActivity.this, SecondActivity.class);
        startActivity(intent);

**public Intent (Context, Class)**
For the first parameter we need to provide the current context.
The second parameter is the Class parameter, to which the system delivers the Intent (the activity we want to start)

- An intent can be used to **start a Service**

  - A Service is a component that performs operations in the background without a user interface (e.g. downloading a file)

- An intent can be used to start a **broadcast** (a message that any app can receive)
- An Intent can also be used to **pass data between activities**

## Passing Data

- When you have multiple activities you most likely need to pass data between them
- You can use objects of:
  - Intent
  - Bundle

### Using an Intent to Pass Primitive Data

- An intent not only allows you to start another activity, but it enables sending data to the other activity
- You can add extra data using the putExtra() method that requires two parameters (the key name and its value)
- putExtra() supports different types of data (see API), examples:

        intent.putExtra(String name, String value)
        intent.putExtra(Stringname,doublevalue)

- An example how to pass data using an intent in the first activity:

        Intent intent = new Intent(MainActivity.this, SecondActivity.class);
        intent.putExtra(“message”, msg);
        startActivity(intent);

### Getting Data from an Intent

- To get data from the intent in the second activity, first you need to use **getIntent( )**
  - e.g. `Intent intent=getIntent();`
- Then from the intent, you can retrieve the data that it is carrying by using a right method that matches the type of data
  - e.g. `String msg = intent.getStringExtra(“message”);`
- More examples of get methods:
  - public int getIntExtra (String name, int defaultValue)
    `int count = intent.getIntExtra(“count", 0);`

  - public double getDoubleExtra (String name, double defaultValue)
    `double price = intent.getDoubleExtra("price", 0.00);`

### Use Intent to Receive Results

- In the first activity, we use the **registerForActivityResult( )** API that:
  - takes two parameters: an **ActivityResultContract** and an **ActivityResultCallback**
  - returns an **ActivityResultLauncher** which is used to launch the second activity
- An **ActivityResultContract** defines the input type (Intent here) needed to produce a result
`public final class ActivityResultContracts.StartActivityForResult extends ActivityResultContract`

  - ActivityResultContracts.StartActivityForResult(): the Intent as an input and ActivityResult as an output
- An **ActivityResultCallback** is an interface with a single method, onActivityResult(), that takes an object of the output type defined in the ActivityResultContract

## Event

**Event 事件**: User 和 Device 的所有交互都叫event, 点击，滑动屏幕，放大缩小等

**Event Listener 事件监听**:

在代码上抓取点击事件 -> 用代码编辑点击后边代表的逻辑

使用回调方法callBack() 来实现逻辑

    binding.clearButton.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            #onClick就是回调方法
            binding.editText.setText("");
        }
    }); 


**Intent**: 用于activity之间的跳转（当然不仅仅是activity，安卓四大组件之间的跳转都能用Intent）

- An Intent can be used to **start an activity**
  - The Intent constructor takes two parameters, a Context (上下文，可以简单理解为当前所在的view) and a Class
:star: At the activity level: `Intent intent = new Intent(this, SecondActivity.class);`
(第一个参数是context上下文，第二个参数是目标class)
:star: Inside the View block: `Intent intent = new Intent(MainActivity.this, SecondActivity.class);`
`startActivity(intent);` 开始跳转

- An intent can be used to **start a Service**
  - A Service is a component that performs operations in the background without a user interface (e.g. downloading a file)
- An intent can be used to **start a broadcast** (a message that any app can receive)
- An Intent can also be used to **pass data between activities**

:blue_heart: The Intent constructor takes two parameters, a Context and a Class.

### Using an Intent to Pass Primitive Data

- An intent not only allows you to start another activity, but it enables sending data to the other activity
- You can add extra data using the putExtra() method that requires two parameters (the key name and its value)
- `putExtra()` supports different types of data (see API), examples: `intent.putExtra(String name, String value)`
`intent.putExtra(Stringname,doublevalue)`
- An example how to pass data using an intent in the first activity: `Intent intent = new Intent(MainActivity.this, SecondActivity.class);` `intent.putExtra(“message”, msg);`
`startActivity(intent);`

### Getting Data from an Intent

- To get data from the intent in the second activity, first you need to use `getIntent()`
e.g. `Intent intent=getIntent();`
- Then from the intent, you can retrieve the data that it is carrying
by using a right method that matches the type of data
e.g. `String msg = intent.getStringExtra(“message”);`
- More examples of get methods:
`public int getIntExtra (String name, int defaultValue)`
`int count = intent.getIntExtra(“count", 0);`
`public double getDoubleExtra (String name, double defaultValue)`
`double price = intent.getDoubleExtra("price", 0.00);`

### Intent and Bundle

- With intents, to send a set of data items, you can use a Bundle

- We recommend that you use the Bundle class to set primitives known to the OS on Intent objects. The Bundle class is highly optimized for marshalling and unmarshalling using parcels

- Multiple data items can be added to one Bundle object and then it can be added to the Intent by calling `putExtras()`:
`Bundle bundle=new Bundle();`
`bundle.putString(“name”, “Helen”);`
`bundle.putString(“surname”, “Jones”);`
`bundle.putString(“phone”, “9902000”);`
`intent.putExtras (bundle);`
`startActivity(intent);`

- To retrieve the data from the bundle in the second activity:

`Bundle bundle=getIntent(). getExtras();`
`String name=bundle.getString(“name”);`

### Pass Objects between Activities

- We can pass objects between activities or processes as **Parcelable** objects, e.g. a student object if it is Parcelable

- An Android Parcelable is an interface that allows writing data and object references to a Parcel and then restoring it from a Parcel

- The object’s class must implement Parcelable

- The Parcelable objects can be added to a Bundle (and then added to an Intent) and passed between activities

### Passing Objects Using a Bundle

- To add a parcelable
`bundle.putParcelable (“student1", student);`
`intent.putExtras(bundle);`

- To retrieve a parcelable in the other activity
`Bundle bundle = getIntent().getExtras();`
`bundle.getParcelable (“student1");`

- But **this works only if the student class is Parcelable**

Object 传递需要把 java object 转换成 parcelable, parcelable 是一个类似 serializable 的序列化格式，但更高效，原理是bundle只接受序列化数据，不接受 java object

## Assignment Notes

### AndroidmMnifest.xml

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AssignmentDemo">
        <activity
            android:name=".SignupActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize"/>
            #每新建一个activity，都需要在manifest中进行注册

        <activity
            android:name=".LoginActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            #键盘弹出时适应界面

            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

### activity_signup.xml

    <ScrollView
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/background"
        android:gravity="center">
        #用于上下拉动页面

        <LinearLayout
          ......
        </LinearLayout>

    </ScrollView>

# 界面跳转

Login -> Sign up

    private ActivityLoginBinding binding;

      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          binding = ActivityLoginBinding.inflate(getLayoutInflater());
          View view = binding.getRoot();
          setContentView(view);

          binding.signupTextView.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  Intent intent = new Intent(LoginActivity.this, SignupActivity.class);
                  startActivity(intent);
              }
          });
      }

Sign up -> Login

    private ActivitySignupBinding binding;

      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          binding = ActivitySignupBinding.inflate(getLayoutInflater());
          View view = binding.getRoot();
          setContentView(view);

          binding.backToLoginView.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  startActivity(new Intent(SignupActivity.this, LoginActivity.class));
              }
          });
      }

After validation

    if (isValidEmail)
    {
        //UI线程
        binding.progressBar.setVisibility(View.VISIBLE);

        //后端线程
        try {
            Thread.sleep(5000);
        } catch(Exception e)
        {
            e.printStackTrace();
        }

        //UI线程
        Toast.makeText(SignupActivity.this, "Sign up successfully", Toast.LENGTH_SHORT).show();
        binding.progressBar.setVisibility(View.GONE);
    }

解决办法

    if (isValidEmail)
    {
        binding.progressBar.setVisibility(View.VISIBLE);

        new Thread(() -> {
            try {
                Thread.sleep(5000);
            } catch (Exception e) {
                e.printStackTrace();
            }

            runOnUiThread(() -> {
                Toast.makeText(SignupActivity.this, "Sign up successfully", Toast.LENGTH_SHORT).show();
                binding.progressBar.setVisibility(View.GONE);
            });
        }).start();
    }