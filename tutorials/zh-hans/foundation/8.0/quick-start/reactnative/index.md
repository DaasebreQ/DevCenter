---
layout: tutorial
title: React Native 端到端演示
breadcrumb_title: React Native
relevantTo: [reactnative]
weight: 1
---
<!-- NLS_CHARSET=UTF-8 -->
## 概述
{: #overview }
本演示的目的是解释端到端流程：

1. 在 {{ site.data.keys.mf_console }} 中注册并下载与 {{ site.data.keys.product_adj }} 客户机 SDK 预捆绑的样本应用程序。
2. 将新的或提供的适配器部署到 {{ site.data.keys.mf_console }}。  
3. 应用程序逻辑更改为发出资源请求。

**最终结果**：

* 成功地 ping {{ site.data.keys.mf_server }}。
* 成功地使用适配器检索数据。

### 先决条件：
{: #prerequisites }
* Xcode for iOS 或 Android Studio for Android
* React Native CLI
* *可选*。{{ site.data.keys.mf_cli }}（[下载]({{site.baseurl}}/downloads)）
* *可选*。单机{{ site.data.keys.mf_server }}（[下载]({{site.baseurl}}/downloads)）

### 步骤 1. 启动 {{ site.data.keys.mf_server }}
{: #1-starting-the-mobilefirst-server }
请确保您[已创建 Mobile Foundation 实例](../../bluemix/using-mobile-foundation)，或者如果使用 [{{ site.data.keys.mf_dev_kit }}](../../installation-configuration/development/mobilefirst)，请导航至服务器的文件夹并运行命令 `./run.sh`（在 Mac 和 Linux 中）或 `run.cmd`（在 Windows 中）。

### 步骤 2. 创建并注册应用程序
{: #2-creating-and-registering-an-application }
在浏览器中，通过加载以下 URL 打开 {{ site.data.keys.mf_console }}：`http://your-server-host:server-port/mfpconsole`。如果服务器在本地运行，请使用 `http://localhost:9080/mfpconsole`。*用户名/密码*为 **admin/admin**。

1. 单击**应用程序**旁的**新建**按钮
    * 选择平台：**Android 或 iOS**
    * 输入 **com.ibm.mfpstarter.reactnative** 作为**应用程序标识**
    * 将 **1.0.0** 输入为**版本**
    * 单击**注册应用程序**

    <img class="gifplayer" alt="注册应用程序" src="register-an-application-reactnative.png"/>

2. 从 [Github](https://github.ibm.com/MFPSamples/MFPStarterReactNative) 下载 React Native 样本应用程序。

### 步骤 3. 编辑应用程序逻辑
{: #3-editing-application-logic }
1. 在您所选的代码编辑器中打开 React Native 项目。

2. 选择位于项目根文件夹中的 **app.js** 文件，并粘贴以下代码片段，以替换现有的 `WLAuthorizationManager.obtainAccessToken()` 函数：

```javascript
  WLAuthorizationManager.obtainAccessToken("").then(
      (token) => {
        console.log('-->  pingMFP(): Success ', token);
        var resourceRequest = new WLResourceRequest("/adapters/javaAdapter/resource/greet/",
          WLResourceRequest.GET
        );
        resourceRequest.setQueryParameters({ name: "world" });
        resourceRequest.send().then(
          (response) => {
            // Will display "Hello world" in an alert dialog.
            alert("Success: " + response.responseText);
          },
          (error) => {
            alert("Failure: " + JSON.stringify(error));
          }
        );
      }, (error) => {
        console.log('-->  pingMFP(): failure ', error.responseText);
        alert("Failed to connect to MobileFirst Server");
      });
```

### 步骤 4. 部署适配器
{: #4-deploy-an-adapter }
下载 [.adapter 工件](../javaAdapter.adapter)，并通过**操作 → 部署适配器**操作从 {{ site.data.keys.mf_console }} 进行部署。

或者，单击**适配器**旁边的**新建**按钮。  

1. 选择**操作 → 下载样本**选项。下载 *Hello World* **Java** 适配器样本。

    > 如果未安装 Maven 和 {{ site.data.keys.mf_cli }}，请遵循屏幕上的**设置开发环境**指示信息。

2. 从**命令行**窗口中，导航至适配器的 Maven 项目根文件夹并运行以下命令：

    ```bash
    mfpdev adapter build
    ```

3. 构建完成时，通过**操作 → 部署适配器**操作从 {{ site.data.keys.mf_console }} 进行部署。适配器可在 **[adapter]/target** 文件夹中找到。

    <img class="gifplayer" alt="部署适配器" src="create-an-adapter.png"/>   


<img src="reactnativeQuickStart.png" alt="样本应用程序" style="float:right"/>

### 步骤 5. 测试应用程序
{: #5-testing-the-application }
1.  确保您已安装 {{ site.data.keys.mf_cli }}，然后浏览至特定平台（iOS 或 Android）根文件夹并运行命令 `mfpdev app register`。如果使用远程 {{ site.data.keys.mf_server }}，那么[运行命令 ](../../application-development/using-mobilefirst-cli-to-manage-mobilefirst-artifacts/#add-a-new-server-instance)
```bash
mfpdev server add
```
来添加服务器，后跟用于注册应用程序的命令，例如：
```bash
mfpdev app register myIBMCloudServer
```
2. 运行以下命令来运行应用程序：
```bash
react-native run-ios|run-android
```

如果已连接某设备，将在该设备上安装并启动此应用程序。否则将使用模拟器或仿真器。

<br clear="all"/>
### 结果
{: #results }
* 单击 **Ping {{ site.data.keys.mf_server }}** 按钮将显示**已连接到 {{ site.data.keys.mf_server }}**。
* 如果应用程序能够连接到 {{ site.data.keys.mf_server }}，那么将使用部署的 Java 适配器进行资源请求调用。然后，适配器响应将显示在警报中。

## 后续步骤
{: #next-steps }
要了解有关在应用程序中使用适配器，如何集成附加服务（如推送通知），使用 {{ site.data.keys.product_adj }} 安全框架及其他内容的更多信息，请：

- 查看[应用程序开发](../../application-development/)教程
- 查看[适配器开发](../../adapters/)教程
- 查看[认证和安全教程](../../authentication-and-security/)
- 查看[通知教程](../../notifications/)
- 查看[所有教程](../../all-tutorials)
