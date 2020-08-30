---
title: "如何在 Xcode 11 中不使用 Storyboard 和低於 iOS 13 進行開發?"
description: "從 iOS 13 和 Xcode 11 ，引入了一個新的概念，叫做 Scene，Scene 是可以在 iPadOS 上同時打開多個窗口，也就是可以同時打開切割畫面"
date: 2020-07-01T21:57:40+08:00
author: "TingKun"
tags: ["Xcode", "Swift"]
categories: ["iOS開發"]
---

<!--more-->

從 iOS 13 和 Xcode 11 ，引入了一個新的概念，叫做 Scene，Scene 是可以在 iPadOS 上同時打開多個窗口，也就是可以同時打開切割畫面。



![img](https://miro.medium.com/max/2273/1*9DVSzbrI4ZvsdDf3PwocOQ.png)

[官方文件來源：Apple Developer Documentation](https://developer.apple.com/documentation/uikit/app_and_environment/scenes)

## 如何在 SceneDelegate 中不使用 Storyboard 創建一個 window，以及如何處理 iOS 12 及以下版本的問題？

先開啟 Xcode 11 ，當你創建一個新項目時， user interface 選擇 Storyboard 才能運行 iOS 13 以下的版本，也會發現 SceneDelegate 自動生成。
這個檔案管理著許多場景中的一個場景的生命週期。
另一方面，AppDelegate 還為 Scene 增加了一個方法。
這是整個應用程序相關的 func ，例如添加和刪除場景。

總而言之，AppDelegate 管理應用級的生命週期，SceneDelegate 管理場景級的生命週期。

------

## SceneDelegate without Storyboard

不使用 Storyboard 情況下，大部分都會去刪除 Main.storyboard，用 viewController 或 xib 進行開發，但引入 SceneDelegate 後，生成的方法基本相同，只是略有不同。


1. Targets Main Interface General清空界面

    ![img](https://miro.medium.com/max/1462/1*vJ2DTPNDIapD2WFpLq3LUQ.png)

    

2. 從 Info.plist 中更改 Application Scene Manifest 的值。 
   
    **Application Scene Manifest > Scene Configuration > Application Session Role > Item 0 (Default Configuration)**

    ![img](https://miro.medium.com/max/1995/1*SypC8lxwODq-uZrB7hsCwQ.png)

    Enable Multiple 設置成 YES, iPadOS上能夠並行多個畫面

   

3. 來到 window 的 rootViewController。 
   

​	**在 SceneDelegate.swift 中的 scene(_:willConnectTo:options:) 方法中，指定以下内容**
​    
![img](https://miro.medium.com/max/2311/1*fmNYcMFnPDIikikDR0Bobw.png)
​    
> 現在可以在不使用 Storyboard 的情況下生成一個 window 👏👏👏

------

## 如何運行 iOS 12 及以下版本？

如果你使用的是 iOS 12 或更低的版本，會發現無法正常運行。
首先，讓我們先設置使用 available 和 SceneDelegate 相關的 code。

![img](https://miro.medium.com/max/2727/1*HCaWEPkOIF_VMvhJ-U7haw.png)


![img](https://miro.medium.com/max/1735/1*vfzJ5eN9P3HyeKdvw3vzhw.png)

在 AppDelegate 中宣告 window、並在`application(_: didFinishLaunchingWithOptions:)`中設置 window，這樣的話就適用於 iOS 13 和小於其他版本 。

![img](https://miro.medium.com/max/2451/1*aZpYiCqW9QO5lyD4yhgpfw.png)

> 有了以上 code，iOS 12 及以下的系統就可以順利運行了👏
>



------



## 參考資料

[Scene Delegate In Xcode 11 & iOS 13 - LearnAppMakingWritten by Reinder de Vries on October 13 2019 in App Development What does the new SceneDelegate class in your iOS 13…learnappmaking.com](https://learnappmaking.com/scene-delegate-app-delegate-xcode-11-ios-13/)

[Understanding the iOS 13 Scene Delegate - Donny WalsBy the end of this week's blog post you will know: What the scene delegate is used for. How you can effectively…www.donnywals.com](https://www.donnywals.com/understanding-the-ios-13-scene-delegate/)
