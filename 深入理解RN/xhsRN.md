小红书的RN是在开源RN基础上做了一层封装，实现了引擎预热、接口预取等能力，并内置了部分业务组件

## 目录结构

![](https://xhs-doc.xhscdn.com/1040025031bqkdkbk7u0ff6o7dk?imageView2/2/w/1600)

## 引擎预热

提前执行一遍「启动流程」，但是不会调用最终runApplication进行渲染

```java
fun loadBusinessBundle(
    catalystInstance: CatalystInstance,
    businessBundleFilePath: String,
    bundleType: String,
    loadSynchronously: Boolean = false,
    tracker: Any? = null
): Boolean {
    val file = File(businessBundleFilePath)
    return try {
        if (file.exists() && file.isFile) {
            loadingBundleType = bundleType
            val bundleVersion = XYReactNativeExtension.getReactNativeBundleManager()?.getLocalBundleVersion(bundleType)
            catalystInstance.imageCallerContext = FrescoParams(FrescoBizParameter.Group.RN, "${bundleType}|${bundleVersion}|${curRoutePath}")
            val startTime = System.currentTimeMillis()
            val fileSize = LogFileUtil.getFileSize(File(businessBundleFilePath))
            if (fileSize <= 0) {
                trackRnFileSizeException(businessBundleFilePath, "loadBusinessBundle", "file exist: ${file.exists()}; size: $fileSize")
                throw DefaultCustomException("loadBusinessBundle: $businessBundleFilePath; size: $fileSize; failed")
            }
            catalystInstance.loadScriptFromFile(businessBundleFilePath, businessBundleFilePath, loadSynchronously)
            trackLoadScriptTime(bundleType, System.currentTimeMillis() - startTime)
            loadingBundleType = ""
            true
        } else {
            ReactActivityTrack.trackRNRouteError(
                action = "load_bundle",
                errorMessage = "bundle not exists",
                bundleType = bundleType,
                rnRoute = curRoutePath,
                extra = businessBundleFilePath
            )
            (tracker as? HybridContainerInitTrack)?.apply {
                val msg = "loadBusinessBundle businessBundleFilePath=${businessBundleFilePath} bundle not exists"
                XHSLog.d(BusinessType.RN_LOG, "${PAGE_LINK_TAG}_${TAG}", "errMsg= $msg")
                trackExtraInfo(HybridContainerInitTrack.KEY_EXTRA_ERROR_MSG, msg)
            }
            false
        }
    } catch (e: Exception) {
        ReactActivityTrack.trackRNRouteError(
            action = "load_bundle",
            errorMessage = "bundle load error",
            bundleType = bundleType,
            rnRoute = curRoutePath,
            extra = e.getStackTraceString()
        )
        (tracker as? HybridContainerInitTrack)?.apply {
            val msg = "loadBusinessBundle businessBundleFilePath=${businessBundleFilePath} bundle load error=${e.message}"
            XHSLog.d(BusinessType.RN_LOG, "${PAGE_LINK_TAG}_${TAG}", "errMsg= $msg")
            trackExtraInfo(HybridContainerInitTrack.KEY_EXTRA_ERROR_MSG, msg)
            trackExtraInfo("trace", e.getStackTraceString())
        }
        false
    }
}

```

## 接口预取

在打开路由过程中，通过XhsRequestPreload进行接口预取

```js
//是否支持新旧预取互斥开关
if(ReactAbHelper.getPreloadDataMutex()) {
    val isUseNewPreRequest = XhsRequestPreload.triggerRnPreRequest(bundle.getString(HostConts.KEY_RAW_URL) ?: "")
    if (isUseNewPreRequest) {
        bundle.putBoolean("isUseNewPreRequest", true)
    } else {
        if (ReactAbHelper.rnPerformanceOptimize and RN_PRE_FETCH_PRELOAD > 0) {
            XHSLog.i(BusinessType.RN_LOG, "${PAGE_LINK_TAG}_${TAG}", "数据预取路由阶段")
            preload(bundle.getString(HostConts.KEY_RAW_URL))
        }
        bundle.putBoolean("isUseNewPreRequest", false)
    }
} else {
    //不支持则保持原有逻辑，新旧同时发起预取
    if (ReactAbHelper.rnPerformanceOptimize and RN_PRE_FETCH_PRELOAD > 0) {
        XHSLog.i(BusinessType.RN_LOG, "${PAGE_LINK_TAG}_${TAG}", "数据预取路由阶段")
        preload(bundle.getString(HostConts.KEY_RAW_URL))
    }
    XhsRequestPreload.triggerRnPreRequest(bundle.getString(HostConts.KEY_RAW_URL) ?: "")
}

```

## 图片预取

```js
ICommercialProxy commercialProxy = ServiceLoader.with(ICommercialProxy.class).getService();
if (commercialProxy != null && commercialProxy.interceptByGoodsDetailRedirect(context, url == null ? "" : url, uri)) {
    return;
}
String preFetchImageUrl = bundle.getString("prefetch-image", "");
if (preFetchImageUrl.isEmpty() && (ReactAbHelper.INSTANCE.getRnPerformanceOptimize() & ReactAbHelper.INSTANCE.getRN_PRE_FETCH_OPT()) > 0) {
    preFetchImageUrl = bundle.getString("sku_image", "");
}
HybridUtil.INSTANCE.prefetchImage(preFetchImageUrl);
 
Routers.build(RnCons.Page.PAGE_PATH_FINAL, bundle).open(context, requestCode);
```