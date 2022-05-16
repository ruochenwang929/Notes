# Week 2 - Android Spinner

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
                        #后续可以改成login activity
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
                                                #activity_main.xml里的LinearLayout
        View view = binding.getRoot();
        setContentView(view);
        #把view加载到当前的activity里面

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
