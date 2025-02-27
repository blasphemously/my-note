### 新老渲染概述

![](https://xhs-doc.xhscdn.com/1040025031bpu84g0nu07pm52so?imageView2/2/w/1600)

#### 老渲染

![](https://xhs-doc.xhscdn.com/1040025031bpu8b9vnu06v9ttlc?imageView2/2/w/1600)

#### Fabric 新渲染器

![](https://xhs-doc.xhscdn.com/1040025031bpu8m8b7u081e1lqc?imageView2/2/w/1600)

我们从上述新老渲染架构可以看出，Yoga布局提前了，从UI Thread放到了C++层。也就是说，shadowNode的维护被放到了C++层。

Shadow Tree 是在 C++ 层创建的树，它由若干个 Shadow 节点组成。这些 Shadow 节点是在创建对应的拥有 stateNode 值的 Fiber 节点时，同步创建的。

在 Fabric 渲染器的 C++ 层，通过 Diff 算法对比更新前后的两棵 Shadow Tree，计算出更新视图的操作指令，完成最终的渲染。这就是，Fabric 渲染器实现将 JSX 渲染成原生视图的整体流程。

![](https://xhs-doc.xhscdn.com/1040025031bpub5a77u0dh975k4?imageView2/2/w/1600)

#### 性能比对

官方通过在 ScrollView 组件中大量渲染 View、Text 或 Image 组件（1500 或 5000 个），以测试新架构的性能。根据官方给出的性能测试报告，Android 的渲染性能提升了 0%～8%，而 iOS 的提升更为明显，达到了 13%～39%。

Google Pixel 4

![](https://xhs-doc.xhscdn.com/1040025031bpunsc6nu0d85ar58?imageView2/2/w/1600)

iPhone 12 Pro

![](https://xhs-doc.xhscdn.com/1040025031bpuo3m2nu05nr3uc0?imageView2/2/w/1600)

### JSI实现

大家回想一下「通信机制」中的Java module方法在C++中被挂载到JS上下文中的逻辑，其实JSI就是把nativeFabricUIManager直接挂到了上下文中，这样就省去了之前提到了执行队列的等待耗时。

```java
void UIManagerBinding::createAndInstallIfNeeded(

jsi::Runtime& runtime,

const std::shared_ptr<UIManager>& uiManager) {

auto uiManagerModuleName = "nativeFabricUIManager";

auto uiManagerValue =

runtime.global().getProperty(runtime, uiManagerModuleName);

if (uiManagerValue.isUndefined()) {

// The global namespace does not have an instance of the binding;

// we need to create, install and return it.

auto uiManagerBinding = std::make_shared<UIManagerBinding>(uiManager);

auto object = jsi::Object::createFromHostObject(runtime, uiManagerBinding);

runtime.global().setProperty(

runtime, uiManagerModuleName, std::move(object));

}

}

```