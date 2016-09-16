---
layout: post
title: '[iOS] 크롬 페이지를 사파리로 열기'
author: 이재연
date: 2016-09-13
tags: [iOS]
---

## 서문

안녕하세요 이재연입니다.

저는 출퇴근 시간에 주로 **아이패드 미니4** 를 통해 슬라이드를 보거나, 개발 블로그를 읽습니다. 이때에는 아이패드용 크롬앱을 사용합니다. 사파리보다는 무겁지만, 윈도우를 포함한 데스크탑끼리와의 동기화를 위함입니다. UI가 깔끔하기도 하구요. 슬라이드는 [SlideShare](http://www.slideshare.net/) 에서 보는 편입니다. 아이패드용 앱도 있어서 좋습니다. 하지만, 크롬 브라우저에서 슬라이드쉐어 페이지를 들어가게되면 앱을 깔아뒀어도 웹으로만 볼수가있고, 앱으로는 열수가 없습니다. 제 경우에는 그랬으며 혹시 방법이 있다면 저에게 가르침을 주십시오. 기본 내장앱인 사파리를 통해서는 앱으로 열것인지 제안이 잘 뜨지만, 아무래도 윈도우와의 동기화를 생각해보면 크롬을 계속 쓸수밖에 없었습니다.

이 불편함을 해결하기 위한 도구로 App extension 을 사용해봤고, 이 경험을 간략하게 적어보려 합니다.

**참고 :**

1. xcode 8 기준입니다.
2. swift 3.0 기준입니다.



## iOS App Extension 이용하기

우선 새로운 Single View Application 프로젝트를 하나 생성합시다.

![Step1](/static/images/2016-09-16-app-extension/Step1.png)

정보는 대충 적어주시구요.

![Step2](/static/images/2016-09-16-app-extension/Step2.png)

생성후에 프로젝트 기본 화면에서 빨간 네모안의 Targets 부분을 잘 봐주세요

![Step3](/static/images/2016-09-16-app-extension/Step3.png)

`Add Target` 을 눌러줍시다.

![Step4](/static/images/2016-09-16-app-extension/Step4.png)

Action  Extension  을 선택하고 Next!

![Step5](/static/images/2016-09-16-app-extension/Step5.png)

정보도 대충적고 만들어주시면 아래와 같은 그룹이 하나 생성되어있을꺼에요.

![Step6](/static/images/2016-09-16-app-extension/Step6.png)

Info.plist 를 열고 `App Transport Securuty Setting` , `Bundle display name` 이 두가지를 다음과 같이 설정해주세요.

![Step7](/static/images/2016-09-16-app-extension/Step7.png)

UIViewController.swift 파일을 하나 만들고 다음과 같이 작성합니다.

```swift
import UIKit

extension UIViewController {

    func openURL(url: NSURL) {
        do {
            let application = try self.sharedApplication()
            application.performSelector(inBackground: "openURL:", with: url)
        }
        catch {

        }
    }

    func sharedApplication() throws -> UIApplication {
        var responder: UIResponder? = self
        while responder != nil {
            if let application = responder as? UIApplication {
                return application
            }

            responder = responder?.next
        }

        throw NSError(domain: "UIInputViewController+sharedApplication.swift", code: 1, userInfo: nil)
    }

}
```

왜 그냥 UIApplication.openURL 함수를 사용하지 않고 우회적으로 실행하는 코드를 작성하는가? 라는 물음에 답해보자면,

1. App Extension 의 경우 self.extensionContext?.openURL 을 통해 실행해야합니다.

2. App Extension 에서는 일반적으로 openURL 을 사용할 수 없습니다. 해당 앱의 Extension 으로 작동해야만 합니다.


이제는 ActionViewController.swift 를 수정해보겠습니다. 이후 스토리보드에서 done action 을 연결해줍니다.

```swift
import UIKit
import MobileCoreServices

class ActionViewController: UIViewController {

    @IBOutlet weak var imageView: UIImageView!

    override func viewDidLoad() {
        super.viewDidLoad()

        for item in self.extensionContext!.inputItems {
            let inputItem = item as! NSExtensionItem
            for provider in inputItem.attachments! {
                let itemProvider = provider as! NSItemProvider
                if itemProvider.hasItemConformingToTypeIdentifier("public.url") {
                    itemProvider.loadItem(forTypeIdentifier: "public.item", options: nil, completionHandler: {
                        result, _ in
                        if let url = result as? NSURL {
                            self.openURL(url: url)
                            self.extensionContext!.completeRequest(returningItems: self.extensionContext!.inputItems, completionHandler: nil)
                        }
                    })
                }
            }
        }
    }

    @IBAction func done() {
        self.extensionContext!.completeRequest(returningItems: self.extensionContext!.inputItems, completionHandler: nil)
    }

}

```

## 실행 화면

![](/static/images/2016-09-16-app-extension/ScreenShot.png)

## 마치며

1. Action Extension 을 만들었을때 기본적으로 생성되는 MainInterface.storyboard 에는 UIImageView 가 추가되어있는데. 이 부분은 사용하지 않으니, MainInterface.storyboard, ActionViewController.swift 두곳에서 imageView 를 삭제해도 무방합니다.
2. `사파리로 열기` 아이콘이 비어있는데, 이 부분은 앱의 아이콘이 들어가게됩니다. 앱 아이콘 이미지를 넣어주세요.
3. [https://github.com/leejaedus/on-safari](https://github.com/leejaedus/on-safari){:target="_blank"}
4. leejaeduss@gmail.com
