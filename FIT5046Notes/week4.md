# Week4 Notes

## Content

- Fragment

- Shared Preference

- LiveData & ViewModel

当销毁一个页面的时候，需要新建一个类将数据保存，存数据的两种方式: SP和livedata, SP是永久储存，livadata是短暂储存，当关闭activity, livedata就清空了。如果数据量比较少且需要经常储存，就用SP。数据量大且一直需要更新的数据就放到livedata里。

## Fragment

- 是一个迷你的activity，也可以实现view和逻辑
- 有自己的生命周期，不过和调用此fragment的activity息息相关
- 每一个fragment在使用时都必须和一个activity绑定
- 可以复用
- 模块化必要的组成部分
- 可控性: 更好的传递参数

### Fragment lifecycle

onAttach(): Fragment和Activity相关联时调用

- 可以通过该方法获取Activity引用，还可以通过getArguments()获取参数

onCreate(): Fragment被创建时调用

onCreateView(): 创建Fragment的布局

onViewCreated(): Observe live data and update view

onViewStateRestored(): 调取onSaveInstanceState()存储的view

onStart(): 当Fragment可见时调用

onResume(): 当Fragment可见且可交互时调用

- 和onPause相对，对话框消失时激活

onPause(): 当Fragment不可交互但可见时调用。--弹出对话框

onStop(): 当Fragment不可见时调用

onSaveInstanceState(): 存储当前view

onDestroyView(): 当Fragment的UI从视图结构中移除时调用

onDestroy():销毁Fragment时调用

onDetach():当Fragment和Activity解除关联时调用

<https://developer.android.com/guide/fragments/lifecycle>

### FragmentContainerView

- 所有的Fragment建议都存在FragmentContainerView中
- 使用时直接放置在layout中

### Fragment Manager

To add or replace fragments in the activity, we use FragmentManager and FragmentTransaction

FragmentManager is responsible for attaching fragments to their host activity and detaching them when the fragment is no longer in use

Calling the replace() method removes an existing fragment in a container and replaces it with an instance of a new fragment class

The add() method will not remove the previous fragment

    FragmentManager fragmentManager = getSupportFragmentManager

    FragmentTransaction fragmentTransaction = fragmentManager beginTransaction();

    fragmentTransaction.replace(R.id.fragment_container_view, nextFragment);

    fragmentTransaction.commit();

## Shared Preference

Android平台上一个轻量级的存储类(持久类储存)

本地XML(Key-value格式)文件，用来保存应用的一些常用配置

作业里我们可以保存登录的用户信息(用户信息加密，密码加密)

ragment /activity 跳转时候的储存地址

可进一步封装提升代码可读性和聚合性

**SP存储数据**

1. 初始化SP，key代表这个SP的表名，mode是这个sp允许的访问范围

`sharedPreferences = context.getSharedPreferences(KEY, Context.MODE_PRIVATE);`

2. 调用Edit, 编辑SP

`SharedPreferences.Editor editor = sharedPreferences.edit();`
`editor.putString(key, value);`

3. 更改SP之后要apply或者commit (get读取数据时候不用)

`editor.commit;` or `editor.apply();`

**SP读取数据**

1. 初始化SP，key代表这个SP的表名，mode是这个sp允许的访问范围

`sharedPreferences = context.getSharedPreferences(KEY, Context.MODE_PRIVATE)`

2. 调用getBoolean / getInt / getString 读取数据，defaultValue是作为value 为null的时候的返回值(可选var)

`sharedPreferences.getBoolean(key, defaultValue)`

3. 不用apply或者commit

## LiveData & View Model

**LiveData**

暂时存储数据的一种class格式

可以implement observe观察方法，来监听数据的改变，其call back method是onChanged()

使用其子类MutableLiveData来改变live data中的数据 (setValue()，postValue())

**View Model**

跨fragment生存的一种暂时存储数据的object类，和activity绑定

通常hold live data object

使用场景举例:

- 执行耗时操作时，可以使view model和fragment/activity脱离，后台下载

- 横竖屏切换时，fragment会经历销毁和重建，而储存在view model中的数据不会销毁
