# List

这是一个基于**Cocos Creator（JS）**写的**虚拟列表**组件。本组件是配合Cocos Creator本身的**滚动窗结构**去写的，所以在编辑器中操作会很方便，**所见即所得**。支持**分帧渲染**、可选**滑动模式**和**选择模式**。

--------------------------------------------------------------------------------

## 使用说明

1. 在编辑器中，创建一个**ScrollView**（也就是**ScrollView->Mask->Content**这样层级结构的节点！）。
2. 将**List组件**拖拽到**ScrollView**节点上。
3. 设置**模板Item**，选择**TemplateType**，可切换**模板类型**，请按需选择。
4. 设置**滑动模式（SlideMode）**，`NORMAL=通常`，`ADHERING=粘附`（可用来制作**分页**效果）。
5. 设置是否为**虚拟列表（Virtual）**，默认为`true`，如果为`false`，则跟普通列表没有区别。
6. 可选设置**逐帧渲染（FrameByFrameRenderNum）**，该数量为**每帧渲染的数量**。
7. 设置**渲染器（RenderEvent）**，在**View**中写一个函数，将该函数指向**RenderEvent**，运行时，设置List数量，Item将会**通过该函数进行回调**，开发者在该函数中实现**Item的刷新**。
8. 可选设置**选择模式（SelectedMode）**，选择模式有**SIMPLE（单选）**、**MULT（多选）**两种模式，须与**ListItem组件**搭配使用，`ListItem`组件需要拖拽到**模板Item**上。在**View**中写一个函数，将该函数指向**SelectedEvent**，运行时，当**选择变更**，将会通过该函数**回调**。

  > 在**View**中，若是**单选**模式，用`list.selectedId=Number`来改变**当前选择**。若是**多选**模式，则调用`list.setMultSelected(args, boolean)`接口来设置多选数据。

9. 完成以上设置后，在**View**中调用`list.numItems=Number`设置列表数量，本组件就会通过**渲染器（即RenderEvent）**进行**回调**了！

> 在**View**中设置`list.customSize`以达到**每个Item宽度或高度不一样**的**虚拟列表**效果，简直美滋滋！但这个会**耗费更多性能！仅支持虚拟列表！仅支持单列或单行**！

--------------------------------------------------------------------------------

## 注意

1. 本组件所依赖的**ScrollView节点**以及`ScrollView`下的孙子节点**Content**，这两个节点的**锚点**需要**按方向**去设置。比如**从顶到底单列排列**，就需要设置锚点为（0.5, 1）。如果是**从左到右网格排列**，就需要设置锚点为（0, 1）。始终将锚点设置到**首个Item**那一边。
2. 各种**反方向**排列的布局（**BOTTOM_TO_TOP**、**RIGHT_TO_LEFT**）都会有问题（Item数量过少，就会导致`Content`错位），**这个是官方问题。而本组件是配合cc.ScrollView去写的，所以也不支持，待官方后续优化（Last test by Creator_v2.1.1）**。如果Item数量很多，那么可以使用。
3. 理论上**设为虚拟列表后不可再设回普通列表**（即`virtual`属性）。
4. SlideMode设为**ADHERING（粘附）**后，组件将**强行屏蔽惯性滚动**。

--------------------------------------------------------------------------------

## 更新日志

### 2019/5/28

- 优化效率。

### 2019/5/27

- 修复滑动模式为吸附（ADHERING）时，还未吸附到目标点时，点击滚动窗，再松开，会吸附失败的BUG。
- 独立吸附接口`list.adhere()`，可外部调用，不限滑动模式。

### 2019/5/23

- 支持BOTTOM_TO_TOP，RIGHT_TO_LEFT布局。需要注意：各种反方向排列的布局（BOTTOM_TO_TOP、RIGHT_TO_LEFT）都会有问题（item数量过少，就会导致Content错位），这个是官方问题。而本组件是配合cc.ScrollView去写的，所以也不支持，待官方后续优化（Last test by Creator_v2.1.1）。如果Item数量很多，那么可以使用。

### 2019/5/21

- 单列布局时支持自定义指定Item的height。单行布局时支持自定义指定Item的width。通过`list.customSize = {0:100, 1:130, 5:120, ...}`来设置，其中key为Item的ID，value为height/width，没有找到该Item的customSize将会与模板ItemSize一致。

### 2019/5/16

- 新增List的配套组件----ListItem。
- 新增选择模式。

### 2019/5/13

- 新增带动画删除Item接口`list.aniDelItem(id, callFunc, aniType)`，因本组件毕竟还是数据驱动，所以请务必在callFunc中删除数据数组中相应下标的数据，然后手动更新该list（即`list.numItems=Number`）。
