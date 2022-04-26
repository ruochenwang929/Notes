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

constraint layout: 每个组件至少需要两个参数来确定它在屏幕中的位置

UI设计：谷歌搜 XXXX material design

## 项目背景设计

res.drawable中添加background.xml

    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item>
            <shape>
                <gradient
                    android:angle="90"
                    android:startColor="@color/lemonchiffon"
                    android:endColor="@color/ivory"
                    #所用color已在values -> colors.xml中添加
                />
            </shape>
        </item>
    </selector>

## LoginActivity.java

    public class LoginActivity extends AppCompatActivity {

        private ActivityLoginBinding binding;

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            binding = ActivityLoginBinding.inflate(getLayoutInflater());
            View view = binding.getRoot();
            setContentView(view);
        }
    }

## activity_login.xml

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:background="@drawable/background"
            #与drawable -> background.xml关联
        android:gravity="center">
            #居中

        <androidx.cardview.widget.CardView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:cardCornerRadius="25dp">
                #边框设置成圆角   

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_margin="15dp"
                android:orientation="vertical">

                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="login"
                    android:gravity="center"
                    android:textStyle="bold"
                    android:textColor="@color/black"
                    android:textSize="30sp" />

                    <com.google.android.material.textfield.TextInputLayout
                        android:layout_width="300dp"
                        android:layout_height="wrap_content"
                        android:padding="5dp"
                        style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox"
                        android:hint="@string/label1">

                        <com.google.android.material.textfield.TextInputEditText
                            android:id="@+id/usernameInputText"
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content">
                        </com.google.android.material.textfield.TextInputEditText>

                    </com.google.android.material.textfield.TextInputLayout>

                    <com.google.android.material.textfield.TextInputLayout
                        android:layout_width="300dp"
                        android:layout_height="wrap_content"
                        android:padding="5dp"
                        app:endIconMode="password_toggle"
                            #显示/隐藏密码
                        style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox"
                        android:hint="@string/label2">

                        <com.google.android.material.textfield.TextInputEditText
                            android:id="@+id/passwordInputText"
                            android:layout_width="match_parent"
                            android:layout_height="wrap_content">
                            android:inputType="textPassword"
                        </com.google.android.material.textfield.TextInputEditText>

                    </com.google.android.material.textfield.TextInputLayout>

            </LinearLayout>

        </androidx.cardview.widget.CardView>

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:backgroundTint="#A1D3FB"
            android:insetTop="6dp"
            android:text="Login"
            android:textColor="@color/black"
            />
    </LinearLayout>