# Spinner

![1607416524471](D:\typora\图片\1607416524471.png)



```xml
<Spinner
    android:id="@+id/planets_spinner"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
```



 微调框选项不限来源，但您必须通过 `SpinnerAdapter` 提供这些选项，例如：若通过数组获取选项，则提供方式应为 `ArrayAdapter`；若通过数据库查询获取选项，则提供方式应为 `CursorAdapter`。 

```xml
<!--例如，如果预先确定了微调框的可用选项，则可通过字符串资源文件中定义的字符串数组来提供这些选项：-->
<!--必须建在values下面，且必须以resources为根部-->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="planets_array">
        <item>Mercury</item>
        <item>Venus</item>
        <item>Earth</item>
        <item>Mars</item>
        <item>Jupiter</item>
        <item>Saturn</item>
        <item>Uranus</item>
        <item>Neptune</item>
    </string-array>
</resources>
```

![1607419175916](D:\typora\图片\1607419175916.png)

 对于如上所示数组，您可以在 `Activity` 或 `Fragment` 中使用以下代码，从而使用 `ArrayAdapter` 实例为微调框提供该数组： 

```java
Spinner spinner = (Spinner) findViewById(R.id.spinner);

/*借助 createFromResource() 方法，您可以从字符串数组创建 ArrayAdapter。此方法的第三个参数是布局资源，该参数定义了所选选项在微调框控件中的显示方式。simple_spinner_item 布局是平台提供的默认布局，除非您想为微调框外观定义自己的布局，否则应使用此布局。*/
ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
        R.array.planets_array, android.R.layout.simple_spinner_item);//array会爆红，但是运行结果没有错

/*调用 setDropDownViewResource(int)，从而指定适配器用于显示微调框选择列表的布局（simple_spinner_dropdown_item 是平台定义的另一种标准布局）。*/

adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);

/*setAdapter() 将适配器应用到 Spinner*/
spinner.setAdapter(adapter);
```

