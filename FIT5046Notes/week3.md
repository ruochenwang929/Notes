# Week 3 Notes

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

## Intent

**Multiple Activities**

- Android applications can use multiple activities and multiple fragments
- When multiple activities are used, the first activity (MainActivity) starts the second activity using an Intent
- An Intent provides runtime binding between separate components, such as two activities

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
