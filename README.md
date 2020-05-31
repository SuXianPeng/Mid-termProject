#期中项目实验 NotePad-master<br>
======================
* 此README文件用于介绍本次期中项目实验，介绍大体分为两部分，第一部分为本次实验进行的整体过程与大致结果，第二部分为本次实验中的一些要点与源码的介绍：<br>
## 第一部分
-----------
* 本次实验的大体过程为：<br>
* 1.进行工程的导入，但在导入工程时发生错误，通过记事本更改NotePad-master工程的，build.gradle(project)、build.gradle(app)、gradle-wrapper.properties文件为能正常编译的文件内容，并去掉AndroidManifest.xml中的<uses-sdk android:minSdkVersion="11" />语句再进行工程的导入，最终编译成功。<br>
* 2.进行第一个功能的实现：第一步在noteslist_item布局文件中增加一个id为text2的TextView用于显示记事本的修改时间，其代码如下：<br>
```
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@android:id/text2"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:paddingLeft="5dip"
    android:singleLine="true"
    />

```
第二步修改NotePadProvide类中的READ_NOTE_PROJECTION字符串数组，添加创建、修改时间的映射，修改后的代码如下：<br>
```
private static final String[] READ_NOTE_PROJECTION = new String[] {
        NotePad.Notes._ID,               // Projection position 0, the note's id
        NotePad.Notes.COLUMN_NAME_NOTE,  // Projection position 1, the note's content
        NotePad.Notes.COLUMN_NAME_TITLE, // Projection position 2, the note's title
        NotePad.Notes.COLUMN_NAME_CREATE_DATE,//@ Projection position 3, the note's create time
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//@ Projection position 4, the note's modification time
};
```
第三步修改NotesList类对PROJECTION数组增加修改时间映射，在oncreate方法中实现适配器的组装增加获取到的修改时间与第一步新增的text2之间的装配，此时显示的时间为毫秒单位的当前时间；<br>
修改代码如下：<br>
```
private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE, // 1
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE//增加修改时间的映射
};
//-----------------------------------------------------------------------------
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
int[] viewIDs = { android.R.id.text1,android.R.id.text2 };
```
运行后效果图如下：<br>
![时间显示1](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E6%97%B6%E9%97%B4%E6%98%BE%E7%A4%BA1.PNG)<br>

第四步修改NoteEditor类中的update方法，先获取到系统的时间再将该时间转化为年月日-时分秒的字符串格式，再将该时间写入修改时间数据中去，从而使得最终显示时间为年月日-时分秒形式；<br>
实现代码如下：<br>
```
// 进行时间获取及格式的转换，最终将得到的时间赋值到数据表中的modification时间
ContentValues values = new ContentValues();
SimpleDateFormat dateformat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String dateStr = dateformat.format(System.currentTimeMillis());
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateStr);

```
运行后效果图如下：<br>
![时间显示2](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E6%97%B6%E9%97%B4%E6%98%BE%E7%A4%BA2.PNG)<br>

* 3.再次修改noteslist_item.xml布局文件，新增ImageView，使其显示为左边小图标、右边标题与修改时间的显示形式，更加美观<br>
整个修改后的noteslist_item.xml实现代码如下：<br>
```
<LinearLayout xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/noteimage"
        android:src="@drawable/app_notes"
        >
    </ImageView>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <TextView xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@android:id/text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:gravity="center_vertical"
            android:paddingLeft="5dip"
            android:singleLine="true"
            />
        <TextView xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@android:id/text2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center_vertical"
            android:paddingLeft="5dip"
            android:singleLine="true"
            />
        </LinearLayout>

</LinearLayout>
```
运行后效果图如下（实际上，上一张截图就是实现该布局后的截图）：<br>
![时间戳](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E6%97%B6%E9%97%B4%E6%88%B3.PNG)<br>
至此第一个功能已完善的完成<br>

* 4.进行第二个功能的实现：第一步在menu文件夹下创建main_menu.xml文件来配置菜单栏的搜索框及样式，其中搜索小图标为自己做的小图标，即为存放在drawable文件夹下的search.PNG文件,配置搜索框代码如下：<br>
```
<item
    android:id="@+id/search"
    android:icon="@drawable/search"
    android:title="Search"
    android:actionViewClass="android.widget.SearchView"
    android:showAsAction="always"
></item>

```

第二步在NoteList类中的onCreateOptionsMenu方法将第一步定义的菜单栏资源对象进行获取并实例化为SearchView对象进行搜索框相关属性的设置以及提交、输入值的响应事件进行监听与处理和设置点击×退出搜索框监听与响应事件，最终完成搜索框的模糊查询，退出会重新搜索当前存在的笔记记录功能，完整的实现了搜索功能；实现上述功能的代码如下<br>
```
public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate menu from XML resource
    MenuInflater inflater = getMenuInflater();
    inflater.inflate(R.menu.list_options_menu, menu);

    // Generate any additional actions that can be performed on the
    // overall list.  In a normal install, there are no additional
    // actions found here, but this allows other applications to extend
    // our menu with their own actions.
    Intent intent = new Intent(null, getIntent().getData());
    intent.addCategory(Intent.CATEGORY_ALTERNATIVE);
    menu.addIntentOptions(Menu.CATEGORY_ALTERNATIVE, 0, 0,
            new ComponentName(this, NotesList.class), null, intent, 0, null);

    //获取菜单搜索宽样式，并通过SearchView对象进行搜索框相关属性的设置
    // 以及提交、输入值的响应事件进行监听与处理
    getMenuInflater().inflate(R.menu.main_menu,menu);
    SearchView mysearchview=(SearchView) menu.findItem(R.id.search).getActionView();
    mysearchview.setQueryHint("搜索");//设置提示信息
    mysearchview.setOnQueryTextListener(new SearchView.OnQueryTextListener(){
        @Override
        //当提交搜索框内容后执行的方法
        public boolean onQueryTextSubmit(String query) {
            search(query);//查询数据库中匹配该结果的数据

            return false;
        }

        @Override
        //当搜索框内内容改变时执行的方法
        public boolean onQueryTextChange(String query) {

            return false;
        }

    });
    //设置点击×退出搜索框监听与响应事件
    mysearchview.setOnCloseListener(new SearchView.OnCloseListener() {
        //添加点击关闭的监听事件，使得点击关闭后搜索出现有的所有笔记
        @Override
        public boolean onClose() {
            refresh();
            return false;
        }
    });


    return super.onCreateOptionsMenu(menu);
}

//对提交的信息进行模糊查询返回查询到的结果，该编码实际为再次的有条件获取数据库信息并用适配器进行适配展现出来
void search(String key)
{
    Intent intent = getIntent();


    if (intent.getData() == null) {
        intent.setData(NotePad.Notes.CONTENT_URI);
    }

    getListView().setOnCreateContextMenuListener(this);

    //通过设置查询条件，达到模糊查询的实现
    String selection=NotePad.Notes.COLUMN_NAME_TITLE+" LIKE ?";
    String[] selectionArgs={"%"+key+"%"};

    Cursor cursor = managedQuery(
            getIntent().getData(),
            PROJECTION,
            selection,
            selectionArgs,
            NotePad.Notes.DEFAULT_SORT_ORDER
    );

    String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;

    int[] viewIDs = { android.R.id.text1,android.R.id.text2 };

    SimpleCursorAdapter adapter
            = new SimpleCursorAdapter(
            this,                             // The Context for the ListView
            R.layout.noteslist_item,          // Points to the XML for a list item
            cursor,                           // The cursor to get items from
            dataColumns,
            viewIDs
    );
    setListAdapter(adapter);

}
//再次的获取数据库中所有笔记信息并用适配器进行适配展现出来（实质为再次执行OnCreate中的部分代码）
void refresh(){
    Intent intent = getIntent();


    if (intent.getData() == null) {
        intent.setData(NotePad.Notes.CONTENT_URI);
    }

    getListView().setOnCreateContextMenuListener(this);

    String selection=null;
    String[] selectionArgs=null;

    Cursor cursor = managedQuery(
            getIntent().getData(),
            PROJECTION,
            selection,
            selectionArgs,
            NotePad.Notes.DEFAULT_SORT_ORDER
    );

    String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;

    int[] viewIDs = { android.R.id.text1,android.R.id.text2 };

    SimpleCursorAdapter adapter
            = new SimpleCursorAdapter(
            this,                             // The Context for the ListView
            R.layout.noteslist_item,          // Points to the XML for a list item
            cursor,                           // The cursor to get items from
            dataColumns,
            viewIDs
    );
    setListAdapter(adapter);

}

```
实现搜索截图1：<br>
![搜索1](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E6%90%9C%E7%B4%A21.PNG)<br>
实现搜索截图2：<br>
![搜索2](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E6%90%9C%E7%B4%A22.PNG)<br>

第二部分
---------------
对本次实验的一些要点细节及变化的介绍
* 1.本次实验将记事本应用的创建、保存、删除图标的样式进行修改，将android：变为app：之后，上述图标都不在显示于菜单栏界面上，而是以文字的形式在点击右侧的三个点后显示：<br>
实现代码：

```
<item android:id="@+id/menu_add"
      android:icon="@drawable/ic_menu_compose"
      android:title="@string/menu_add"
      android:alphabeticShortcut='a'
    app:showAsAction="always" />
<item android:id="@+id/menu_save"
      android:icon="@drawable/ic_menu_save"
      android:alphabeticShortcut='s'
      android:title="@string/menu_save"
    app:showAsAction="ifRoom|withText" />
<item android:id="@+id/menu_delete"
      android:icon="@drawable/ic_menu_delete"
      android:title="@string/menu_delete"
    app:showAsAction="ifRoom|withText" />

```
例图：<br>
![例图](https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/image.png)<br>

* 2.对搜索框实现的模糊搜索实现要点，主要实现思路是进行搜索框的提交事件进行监听并进行响应方法的定义与执行，在响应定义方法中使用oncreate方法中进行数据查询与数据装配的方法进行搜索内容的查询，不同之处在于，将selection条件设为标题名与 搜索内容相似条件，selectionArgs参数条件设为 %搜索内容% 的形式进行搜索，具体代码为（若想看该部分完整代码见第一部分的第四点的第二步所展示出的代码）：<br>
```
//当提交搜索框内容后执行的方法
public boolean onQueryTextSubmit(String query) {
    search(query);//查询数据库中匹配该结果的数据

    return false;
}

//通过设置查询条件，达到模糊查询的实现,也是与oncreate方法中相关代码唯一不同的地方
String selection=NotePad.Notes.COLUMN_NAME_TITLE+" LIKE ?";
String[] selectionArgs={"%"+key+"%"};

```

* 3.进行点击搜索框内的×号，退出搜索框并显示已有的笔记内容操作实现要点，对点击×退出搜索框操作进行事件监听和响应方法，在响应方法中使用oncreate方法中进行数据查询与数据装配的方法进行搜索内容的查询即可，即selection条件与selectionArgs参数条件都为null，具体代码为（若想看该部分完整代码见第一部分的第四点的第二步所展示出的代码）：<br>
```

//设置点击×退出搜索框监听与响应事件
mysearchview.setOnCloseListener(new SearchView.OnCloseListener() {
    //添加点击关闭的监听事件，使得点击关闭后搜索出现有的所有笔记
    @Override
    public boolean onClose() {
        refresh();
        return false;
    }
});

String selection=null;
String[] selectionArgs=null;


```
退出截图1：<br>
![退出1]https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E9%80%80%E5%87%BA1.PNG)<br>

退出截图2：<br>
![退出2]https://github.com/SuXianPeng/Mid-termProject/blob/master/%E9%A1%B9%E7%9B%AE%E7%9B%B8%E5%85%B3%E6%88%AA%E5%9B%BE/%E9%80%80%E5%87%BA2.PNG)<br>


* 4进行标准时间显示的实现要点，事先将时间转化为标准年月日、时分秒时间格式，在将此时间赋值到数据库中的修改时间即可，注意获取的系统时间是模拟器上显示的时间而不是本机电脑显示的时间,实现代码为:<br>
```
// 进行时间获取及格式的转换，最终将得到的时间赋值到数据表中的modification时间
ContentValues values = new ContentValues();
SimpleDateFormat dateformat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String dateStr = dateformat.format(System.currentTimeMillis());
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateStr);

```



* 至此本次实验完成，以上即为对本次实验完成的结果以及过程等进行描述，若想知道更加详细的内容，请查看源代码或源文件。
