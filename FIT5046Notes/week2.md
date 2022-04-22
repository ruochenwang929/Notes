# Week 2 notes

## activity_main.xml

    <LinearLayout 
        xmlns:android="http://schemas.android.com/apk/res/android" #固定用法
        android:id="@+id/activity_main" #每个组件都要有自己的id
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <EditText #为用户添加一个输入框
            android:id="@+id/editText"
            android:layout_width="wrap_content" #内容有多少就分配多少空间
            android:layout_height="wrap_content"
            android:inputType="text"    #text表示啥都能输
            android:hint="@string/hint"> #与strings.xml关联

        </EditText>

    </LinearLayout>

## AndroidManifest.xml

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher" 
                        #APP图标，在res->mipmap->ic_launcher.xml下
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
                        #图标的形状，方形的或者圆形的
            android:supportsRtl="true"
            android:theme="@style/Theme.AssignmentDemo">
                        #主题
            <activity
                android:name=".MainActivity"
                        #模拟机启动时直接调用main activity
                android:exported="true">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>

## Spinner: MainActivity.java

    public class MainActivity extends AppCompatActivity {

    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        View view = binding.getRoot();
        setContentView(view);

        List<String> list = new ArrayList<String>();
        list.add("Toy Story");
        list.add("Up");
        list.add("Shrek");

        final ArrayAdapter<String> spinnerAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_spinner_item, list);
        binding.movieSpinner.setAdapter(spinnerAdapter);

        binding.addButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                String newMovie=binding.editText.getText().toString(); //捕获用户的输入
                spinnerAdapter.add(newMovie);       //加入到arraylist中
                spinnerAdapter.notifyDataSetChanged(); //内容改变了就刷新，不需要通过重启app来刷新数据
                binding.movieSpinner.setSelection(spinnerAdapter.getPosition(newMovie));
                                    //强制更改选项内容，setSelection只能接受int值
            }
        });

        binding.clearButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                binding.editText.setText("");
            }
        });
    }
    }

constraint layout: 每个组件至少需要两个参数来确定它在屏幕中的位置
