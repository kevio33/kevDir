## Fragment



### 1.概述

>  Fragment表示应用界面中可重复使用的一部分。Fragment 定义和管理自己的布局，具有自己的生命周期，并且可以处理自己的输入事件。**Fragment 不能独立存在，而是必须由 Activity 或另一个 Fragment 托管**。Fragment 的视图层次结构会成为宿主的视图层次结构的一部分，或附加到宿主的视图层次结构。

### 2.模块化

> Fragment 允许您将界面划分为离散的区块，从而将模块化和可重用性引入 Activity 的界面。Activity 是围绕应用的界面放置全局元素（如抽屉式导航栏）的理想位置。相反，Fragment 更适合定义和管理单个屏幕或部分屏幕的界面。

例如：

假设有一个响应各种屏幕尺寸的应用。在较大的屏幕上，该应用应显示一个静态抽屉式导航栏和一个采用网格布局的列表。在较小的屏幕上，该应用应显示一个底部导航栏和一个采用线性布局的列表。在 Activity 中管理所有这些变化因素可能会很麻烦。将导航元素与内容分离可使此过程更易于管理。然后，Activity 负责显示正确的导航界面，而 Fragment 采用适当的布局显示列表。

<img src="Android--其他组件.assets/fragment-screen-sizes.png" alt="同一屏幕的采用不同屏幕尺寸的两个版本。" style="zoom:67%;" />

### 3.创建Fragment

#### 创建环境

> 片段需要依赖于 [AndroidX 片段库](https://developer.android.google.cn/jetpack/androidx/releases/fragment)。您需要将[Google Maven 存储库](https://developer.android.google.cn/studio/build/dependencies#google-maven)添加 到您的项目`build.gradle`文件中以包含此依赖项。

```groovy
buildscript {
    ...

    repositories {
        google()
        ...
    }
}

allprojects {
    repositories {
        google()
        ...
    }
}
```

> 要将 AndroidX Fragment 库包含到您的项目中，请在您的应用程序`build.gradle`文件中添加以下依赖项：

```groovy
dependencies {
    def fragment_version = "1.3.5"

    // Java language implementation
    implementation "androidx.fragment:fragment:$fragment_version"
}
```

#### 创建Fragment类

要创建定义自己布局的最小片段，请将片段的布局资源提供给基本构造函数，如以下示例所示：

```java
class ExampleFragment extends Fragment {
    public ExampleFragment() {
        super(R.layout.example_fragment);
    }
    
    //添加Fragment的布局
     @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.left_fragment, container,false);
        return view;
    }
}
```

#### 专业Fragemnt基类

**DialogFragment**

> 显示浮动对话框。使用此类创建对话框是在[`Activity`](https://developer.android.google.cn/reference/android/app/Activity)类中使用对话框辅助方法的一个很好的替代方法 ，因为片段会自动处理`Dialog`,详情参考--https://developer.android.google.cn/guide/fragments/dialogs



#### 将Fragment添加到Activity

> 您可以通过在活动的布局文件中定义片段或在活动的布局文件中定义片段容器，然后以编程方式从活动中添加片段，将片段添加到活动的视图层次结构中。在任何一种情况下，您都需要添加一个 [`FragmentContainerView`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentContainerView) 定义片段应放置在活动视图层次结构中的位置。强烈建议始终使用 a `FragmentContainerView`作为片段的容器，因为 `FragmentContainerView`包括其他视图组`FrameLayout`不提供的特定于片段的修复程序。

##### (1)通过XML文件添加

```xml
<!-- res/layout/example_activity.xml -->
<androidx.fragment.app.FragmentContainerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:name="com.example.ExampleFragment" />
```

> 该`android:name`属性指定`Fragment`要实例化的类名。

##### (2)编程方式添加

```xml
<!-- res/layout/example_activity.xml -->
<androidx.fragment.app.FragmentContainerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

在`FragmentActivity`中可以获得 FragmentManager的实例，该实例 [`FragmentManager`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentManager)可用于创建`FragmentTransaction`. 然后，可以在`onCreate()`使用[`FragmentTransaction.add()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#add(int, java.lang.Class, android.os.Bundle))，在活动的方法中实例化片段，`ViewGroup`在布局中传递容器的ID 和要添加的片段类，然后提交事务，如以下示例所示：

```java
public class ExampleActivity extends AppCompatActivity {
    public ExampleActivity() {
        super(R.layout.example_activity);
    }
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                .setReorderingAllowed(true)
                .add(R.id.fragment_container_view, ExampleFragment.class, null)
                .commit();
        }
    }
}
```

 **传递参数**

```java
public class ExampleActivity extends AppCompatActivity {
    public ExampleActivity() {
        super(R.layout.example_activity);
    }
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (savedInstanceState == null) {
            Bundle bundle = new Bundle();//放参数
            bundle.putInt("some_int", 0);

            getSupportFragmentManager().beginTransaction()
                .setReorderingAllowed(true)
                .add(R.id.fragment_container_view, ExampleFragment.class, bundle)
                .commit();
        }
    }
}
```

**通过requireArguments()获取**

```java
class ExampleFragment extends Fragment {
    public ExampleFragment() {
        super(R.layout.example_fragment);
    }

    @Override
    public void onViewCreated(@NonNull View view, Bundle savedInstanceState) {
        int someInt = requireArguments().getInt("some_int");//获取参数
        ...
    }
}
```

### 4.Fragment管理器

#### (1)概述

> FragmentManager类负责对应用的 Fragment 执行一些操作，如添加、移除或替换它们，以及将它们添加到返回堆栈。`FragmentManager` 管理 Fragment 返回堆栈。在运行时，`FragmentManager` 可以执行添加或移除 Fragment 等返回堆栈操作来响应用户互动。每一组更改作为一个单元（称为 [`FragmentTransaction`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction)）一起提交。

**在 Activity 中访问**

每个 FragmentActivity及其子类（如 [AppCompatActivity]都可以通过[getSupportFragmentManager()] 方法访问 FragmentManager。

**在 Fragment 中访问**

Fragment 也能够托管一个或多个子 Fragment。在 Fragment 内，您可以通过 [`getChildFragmentManager()`]获取对管理 Fragment 子级的 `FragmentManager` 的引用。如果您需要访问其宿主 `FragmentManager`，可以使用 [`getParentFragmentManager()`]。

下面是各个宿主之间FragmentManager之间的关系

![每个宿主都有与其关联的 FragmentManager，用于管理其子 Fragment](Android--其他组件.assets/manager-mappings.png)

> 每个宿主都有与其关联的 `FragmentManager`，用于管理其子 Fragment。

#### (2)返回堆栈

**FragmentManager管理 Fragment 返回堆栈**。在运行时，FragmentManager可以执行添加或移除 Fragment 等返回堆栈操作来响应用户互动。每一组更改作为一个单元（称为 FragmentTransactio）一起提交。

当用户按设备上的“返回”按钮时，或者当您调用 FragmentManager.popBackStack()时，最上面的 Fragment 事务会从堆栈中弹出。换句话说，**事务是反转的。如果堆栈上没有更多 Fragment 事务，并且您没有使用子 Fragment，则返回事件会向上传递到 Activity。**

当您对事务调用 addToBackStack()时，请注意，事务可以包括任意数量的操作，如添加多个 Fragment、替换多个容器中的 Fragment，等等。**弹出返回堆栈时，所有这些操作会作为一项原子化操作反转**。如果您在调用 `popBackStack()` 之前提交了其他事务，并且您没有对事务使用 addToBackStack()，则这些操作不会反转。因此，在一个 FragmentTransaction中，应避免让影响返回堆栈的事务与不影响返回堆栈的事务交织在一起。

如果您在执行移除 Fragment 的事务时未调用 addToBackStack()，则提交事务时会销毁已移除的 Fragment，用户无法返回到该 Fragment。**如果您在移除某个 Fragment 时调用了 addToBackStack()，则该 Fragment 只会 STOPPED，稍后当用户返回时它会 RESUMED**。请注意，在这种情况下，其视图会被销毁。

#### (3)执行事务

如需在布局容器中显示 Fragment，请使用 `FragmentManager` 创建 `FragmentTransaction`。在事务中，您随后可以对容器执行 [`add()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#add(int, java.lang.Class, android.os.Bundle)) 或 [`replace()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#replace(int, java.lang.Class, android.os.Bundle)) 操作。

例如，一个简单的 `FragmentTransaction` 可能如下所示：

```java
FragmentManager fragmentManager = getSupportFragmentManager();
fragmentManager.beginTransaction()
    .replace(R.id.fragment_container, ExampleFragment.class, null)
    .setReorderingAllowed(true)
    .addToBackStack("name") // name can be null
    .commit();
```

> setReorderingAllowed(true)--可优化事务中涉及的 Fragment 的状态变化，以使动画和过渡正常运行。
>
> 调用 `addToBackStack()`会将事务提交到返回堆栈。用户稍后可以通过按“返回”按钮反转事务并恢复上一个 Fragment。如果您在一个事务中添加或移除了多个 Fragment，弹出返回堆栈时，所有这些操作都会撤消。在 `addToBackStack()` 调用中提供的可选名称使您能够使用 [`popBackStack()`]弹回到该特定事务。

#### (4)查找现有Fragment

①您可以使用 findFragmentById()获取对布局容器中当前 Fragment 的引用。

```java
FragmentManager fragmentManager = getSupportFragmentManager();
fragmentManager.beginTransaction()
    .replace(R.id.fragment_container, ExampleFragment.class, null)
    .setReorderingAllowed(true)
    .addToBackStack(null)
    .commit();

...

ExampleFragment fragment =
        (ExampleFragment) fragmentManager.findFragmentById(R.id.fragment_container);
```

②或者，您也可以为 Fragment 分配一个唯一的标记，并使用 findFragmentByTag()获取引用。您可以在布局中定义的 Fragment 上使用 `android:tag` XML 属性来分配标记，也可以在 `FragmentTransaction` 中的 `add()` 或 `replace()` 操作期间分配标记。

```java
FragmentManager fragmentManager = getSupportFragmentManager();
fragmentManager.beginTransaction()
    .replace(R.id.fragment_container, ExampleFragment.class, null, "tag")
    .setReorderingAllowed(true)
    .addToBackStack(null)
    .commit();

...

ExampleFragment fragment = (ExampleFragment) fragmentManager.findFragmentByTag("tag");
```

#### (5)支持多个返回堆栈

在某些情况下，您的应用可能需要支持多个返回堆栈。一个常见示例是，您的应用使用底部导航栏。`FragmentManager` 可让您通过 `saveBackStack()` 和 `restoreBackStack()` 方法支持多个返回堆栈。这两种方法使您可以通过保存一个返回堆栈并恢复另一个返回堆栈来在返回堆栈之间进行交换。



> `saveBackStack()` 的工作方式类似于使用可选 `name` 参数调用 `popBackStack()`：弹出指定事务以及堆栈上在此之后的所有事务。不同之处在于 `saveBackStack()` [会保存弹出事务中所有 fragment 的状态](https://developer.android.google.cn/guide/fragments/saving-state)。

例如，假设您之前使用 `addToBackStack()` 提交 `FragmentTransaction`，从而将 fragment 添加到返回堆栈：

```java
supportFragmentManager.beginTransaction()
  .replace(R.id.fragment_container, ExampleFragment.class, null)
  // setReorderingAllowed(true) and the optional string argument for
  // addToBackStack() are both required if you want to use saveBackStack().
  .setReorderingAllowed(true)
  .addToBackStack("replacement")
  .commit();
```

在这种情况下，您可以通过调用 `saveState()` 来保存此 fragment 事务和 `ExampleFragment` 的状态：

```java
supportFragmentManager.saveBackStack("replacement");
```

> **注意**：您只能将 `saveBackStack()` 用于调用 `setReorderingAllowed(true)` 的事务，以确保可以将事务还原为单一原子操作。

您可以使用相同的名称参数调用 `restoreBackStack()`，以**恢复所有弹出的事务以及所有保存的 fragment 状态**：

```java
supportFragmentManager.restoreBackStack("replacement");
```

#### (6)为Fragment提供依赖项

添加 Fragment 时，您可以手动实例化 Fragment 并将其添加到 `FragmentTransaction`。

```java
// Instantiate a new instance before adding
ExampleFragment myFragment = new ExampleFragment();
fragmentManager.beginTransaction()
    .add(R.id.fragment_view_container, myFragment)
    .setReorderingAllowed(true)
    .commit();
```

> 当应用遇到配置更改期间(例如屏幕旋转)，Activity 及其所有 Fragment 都会被销毁，然后使用最适用的 [Android 资源](https://developer.android.google.cn/guide/topics/resources/providing-resources#BestMatch)重新创建。

> 默认情况下，`FragmentManager` 使用框架提供的 [`FragmentFactory`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentFactory) 实例化 Fragment 的新实例。此默认工厂使用反射来查找和调用 Fragment 的无参数构造函数。这意味着，您无法使用此默认工厂为 Fragment 提供依赖项。这也意味着，默认情况下，在重新创建过程中，不会使用您首次创建 Fragment 时所用的任何自定义构造函数。
>
> 如需为 Fragment 提供依赖项或使用任何自定义构造函数，您必须创建自定义 `FragmentFactory` 子类，然后替换 [`FragmentFactory.instantiate`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentFactory#instantiate(java.lang.ClassLoader, java.lang.String))。您随后可以将 `FragmentManager` 的默认工厂替换为您的自定义工厂，它随后用于实例化 Fragment。

详情参考文档:https://developer.android.google.cn/guide/fragments/fragmentmanager#dependencies

### 5.FragmentTransaction

> 在运行时，a [`FragmentManager`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentManager) 可以添加、删除、替换和使用片段执行其他操作以响应用户交互。您提交的每组片段更改都称为一个*事务*，您可以使用[`FragmentTransaction`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction) 该类提供的 API 指定要在事务内部执行的操作 。您可以将多个操作组合到一个事务中——例如，一个事务可以添加或替换多个片段。当您在同一屏幕上显示多个同级片段时，此分组非常有用，例如拆分视图。

一个简单的事务，并且提交

```java
FragmentManager fragmentManager = ...
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

// Add operations here

fragmentTransaction.commit();//提交
```

#### (1)add/remove/replace/detach/attach

- 要将片段添加到`FragmentManager`，请调用 [`add()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#add(int, java.lang.Class, android.os.Bundle)) 事务。此方法接收片段*容器*的 ID ，以及您要添加的片段的类名。添加的片段被移动到`RESUMED` 状态。
- 要从主机中**删除片段**，请调用 [`remove()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#remove(androidx.fragment.app.Fragment))，传入从片段管理器通过`findFragmentById()`或检索的片段实例`findFragmentByTag()`。如果之前将片段的视图添加到容器中，则此时会从容器中删除该视图。移除的片段被移动到**`DESTROYED`状态**。
- 使用 [`replace()`](https://developer.android.google.cn/reference/androidx/fragment/app/FragmentTransaction#replace(int, java.lang.Class, android.os.Bundle)) 一个新的片段类，您提供的实例，以取代在容器中的现有片段。调用`replace()`相当于调用`remove()`容器中的一个片段并将新的片段添加到同一个容器中。
- detach()方法，将一个fragment视图删除，但是**不摧毁fragment(与remove最大区别)**，就如stopped状态一样，fragmentmanager保存此fragment状态
- attach()方法，将一个已经分离的fragment重新attach，并且创建UI视图

#### (2)commit

提交事务是异步的，他会放在UI主线程执行，如果你需要立即执行，可以调用**commitNow();**

但是commitNow()方法与addToBackStack方法并不兼容，这时候你可以使用executePendingTransactions()来执行以及commit但还未执行的事务。

对于大多数项目，commit已经足够。

#### (3)执行顺序

当配合动画时，执行顺序尤为重要

```java
getSupportFragmentManager().beginTransaction()
        .setCustomAnimations(enter1, exit1, popEnter1, popExit1)
        .add(R.id.container, ExampleFragment.class, null) // gets the first animations
        .setCustomAnimations(enter2, exit2, popEnter2, popExit2)
        .add(R.id.container, ExampleFragment.class, null) // gets the second animations
        .commit()
```

#### (4)显示和隐藏fragment视图

使用`show()`和`hide()`来分别显示和隐藏fragment的视图

### 6.动画

动画可以使用两种框架来实现：

- Animation/Animator
- Transition Framework

**注意：涉及一种以上动画类型强烈建议使用TransitionFramework**

#### (1)使用animation

例子：

点击一个fragment，另一个fragment从右侧划出，定义为 [补间动画资源](https://developer.android.google.cn/guide/topics/resources/animation-resource#Tween)

**文件位置：**

> res/anim/*filename*.xml

![进入和退出动画。 当前片段淡出，而下一个片段从右侧滑入。](Android--其他组件.assets/enter-exit-animation.gif)

```xml
<!-- res/anim/fade_out.xml -->
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="1"
    android:toAlpha="0" />
```

```xml
<!-- res/anim/slide_in.xml -->
<?xml version="1.0" encoding="utf-8"?>
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="100%"  
    android:toXDelta="0%" />
```

在返回上一个fragment时，为返回堆栈设置动画，称为popEnter和popOut

![popEnter 和 popExit 动画。 当前片段向右滑出屏幕，而前一个片段淡入。](Android--其他组件.assets/pop-animations.gif)

```xml
<!-- res/anim/slide_out.xml -->
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromXDelta="0%"
    android:toXDelta="100%" />
```

```xml
<!-- res/anim/fade_in.xml -->
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:fromAlpha="0"
    android:toAlpha="1" />
```

然后通过 FragmentTransaction.setCustomAnimations()调用

```java
Fragment fragment = new FragmentB();
getSupportFragmentManager().beginTransaction()
    .setCustomAnimations(
        R.anim.slide_in,  // enter
        R.anim.fade_out,  // exit
        R.anim.fade_in,   // popEnter
        R.anim.slide_out  // popExit
    )
    .replace(R.id.fragment_container, fragment)//即将进入的fragment
    .addToBackStack(null)
    .commit();
```

#### (2)使用transition

transition是一款很不错的动画框架。

例子：

我们想要实现和以上相同的效果

```java
<!-- res/transition/fade.xml -->
<fade xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"/>
```

```xml
<!-- res/transition/slide_right.xml -->
<slide xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="@android:integer/config_shortAnimTime"
    android:slideEdge="right" />
```

定义好了资源文件之后，通过setEnterTransition()和setExitTransitiond()调用

```java
public class FragmentA extends Fragment {
    @Override
    public View onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        TransitionInflater inflater = TransitionInflater.from(requireContext());
        setExitTransition(inflater.inflateTransition(R.transition.fade));
    }
}

public class FragmentB extends Fragment {
    @Override
    public View onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        TransitionInflater inflater = TransitionInflater.from(requireContext());
        setEnterTransition(inflater.inflateTransition(R.transition.slide_right));
    }
}
```

### 7.生命周期

每个可能的`Lifecycle`状态都在[`Lifecycle.State`](https://developer.android.google.cn/reference/androidx/lifecycle/Lifecycle.State)枚举中表示 。

- [`INITIALIZED`]
- [`CREATED`]
- [`STARTED`]
- [`RESUMED`]
- [`DESTROYED`]

生命周期以及回调(包含Fragment的回调和view的回调)

![片段生命周期状态及其关系片段的生命周期回调和片段的视图生命周期](Android--四大组件、存储.assets/fragment-view-lifecycle.png)

> 当用户准备离开fragment且这时候fragment还可见，调用onPause回调
>
> 当用户已经离开fragment且这时候fragment不可见，调用onStop()回调

![img](Android--四大组件、存储.assets/145991-dc5d005d99c94464.png)

#### onAttach()和onDetach

- onAttach回调在被添加至FragmentManager后被调用，此时FragmentManager已经开始管理Fragment，这是可以通过findFragmentById()获取该fragment
- onDetach方法在fragment被remove之后调用，这时候不能通过findFragmentById()获取该fragment

### 8.保存Fragment相关状态

下表列出了会导致 Fragment 丢失状态的操作，以及各种类型的状态是否会在发生这些更改后持续存在。表中提及的状态类型如下：

- 变量：Fragment 中的局部变量。
- 视图状态：Fragment 中**一个或多个视图所拥有**的任何数据。
- SavedState：此 Fragment 实例固有的数据，这些数据应保存在 `onSaveInstanceState()` 中。
- NonConfig：从外部来源（如服务器或本地代码库）提取的数据，或提交后发送到服务器的用户创建的数据。

通常，将变量视为与 SavedState 相同，但下表对这两者进行了区分，以说明各种操作对每种状态类型所产生的影响。

![image-20210629161933422](Android--四大组件、存储.assets/image-20210629161933422.png)

#### (1)SavedState数据保存方式

```java
@Override
public void onSaveInstanceState(@NonNull Bundle outState) {
    super.onSaveInstanceState(outState);
    outState.putBoolean(IS_EDITING_KEY, isEditing);
    outState.putString(RANDOM_GOOD_DEED_KEY, randomGoodDeed);
}
```

如需恢复 `onCreate(Bundle)` 中的状态，请从捆绑包中检索存储的值：

```java
@Override
public void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    if (savedInstanceState != null) {
        isEditing = savedInstanceState.getBoolean(IS_EDITING_KEY, false);
        randomGoodDeed = savedInstanceState.getString(RANDOM_GOOD_DEED_KEY);
    } else {
        randomGoodDeed = viewModel.generateRandomGoodDeed();
    }
}
```

#### (2)NonConfig

应将 NonConfig 数据放置在 Fragment 之外，如在 [`ViewModel`](https://developer.android.google.cn/reference/androidx/lifecycle/ViewModel) 中。在前面的示例中，`seed`（NonConfig 状态）是在 `ViewModel` 中生成的。维护其状态的逻辑由 `ViewModel` 拥有。

```java
public class RandomGoodDeedViewModel extends ViewModel {
    private Long seed = ... // Generate the seed

    private String generateRandomGoodDeed() {
        String goodDeed = ... // Generate a random good deed using the seed
        return goodDeed;
    }
}
```

#### (3)其他

当出现如下情况时：

![img](Android--四大组件、存储.assets/145991-1808ce5c8e7bcb2c.jpeg)

根据上面的生命周期图你将会看到Fragment从后退栈中返回时，会回调 onDestroyview 方法和 onCreateview 方法。不管怎样，显然在这种情况 onSaveInstanceState 方法没有被调用。结果就是 UI 里的所有都没有了，然后默认按照 Layout XML 文件中定义的来重新创建。

这种方法我们可以使用Argument, 它会随着 Fragment 一直保存着。

```java
Bundle savedState;
@Override
public void onActivityCreated(Bundle savedInstanceState) {
   super.onActivityCreated(savedInstanceState);
   // Restore State Here
   if (!restoreStateFromArguments()) {
      // First Time running, Initialize something here
   }
}
@Override
public void onSaveInstanceState(Bundle outState) {
   super.onSaveInstanceState(outState);
   // Save State Here
   saveStateToArguments();
}
@Override
public void onDestroyView() {
   super.onDestroyView();
   // Save State Here
   saveStateToArguments();
}
private void saveStateToArguments() {
   savedState = saveState();
   if (savedState != null) {
      Bundle b = getArguments();
      b.putBundle(“internalSavedViewState8954201239547”, savedState);
   }
}
private boolean restoreStateFromArguments() {
   Bundle b = getArguments();
   savedState = b.getBundle(“internalSavedViewState8954201239547”);
   if (savedState != null) {
      restoreState();
      return true;
   }
   return false;
}
/////////////////////////////////
// Restore Instance State Here
/////////////////////////////////
private void restoreState() {
   if (savedState != null) {
      // For Example
      //tv1.setText(savedState.getString(“text”));
   }
}
//////////////////////////////
// Save Instance State Here
//////////////////////////////
private Bundle saveState() {
   Bundle state = new Bundle();
   // For Example
   //state.putString(“text”, tv1.getText().toString());
   return state;
}
```

> 详情参考--https://www.jianshu.com/p/75dc2f51cd63

### 9.Fragment通信

> 参见--https://developer.android.google.cn/guide/fragments/communicate
>
> [Fragment的四种跳转](https://www.jianshu.com/p/ab1cb7ddf91f)

- **1.从同一个Activiy的一个Fragment跳转到另外一个Fragment**

  > ```java
  > getActivity().getSupportFragmentManager() 
  >     .beginTransaction()
  >     .replace(R.id.xx, new XxxFragment(), null) 
  >     .addToBackStack(null)
  >     .commit();
  > ```

- **2.从一个Activity的Fragment跳转到另外一个Activity**

  > ```java
  > Intent intent = new Intent(getActivity(),OtherActivity.class); 
  > startActivity(intent);
  > ```

- **3.从一个Activity跳转到另外一个Activity的Fragment上**

  > ```java
  > //Activity1
  > Intent intent = new Intent(OtherActivity.this, MainActivity.class);
  > intent.putExtra("id",1);
  > startActivity(intent);
  > 
  > //Activity2
  > int id = getIntent().getIntExtra("id", 0);
  > if (id == 1) {      
  >     getSupportFragmentManager()
  >         .beginTransaction()
  >         .replace(R.id.fragment_container,new YourFragment())
  >         .addToBackStack(null)
  >         .commit(); 
  > }
  > ```

- **4.从一个Activity的Fragment跳转到另外一个Activity的Fragment上**

  > ```java
  > Intent intent = new Intent(getActivity(), MainActivity.class);
  > ```
  >
  > 

## 三、Navigation