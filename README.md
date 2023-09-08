# [Sendbird](https://sendbird.com) Sendbird X ChatGPT Fintech AI Chatbot Demo

[![Platform](https://img.shields.io/badge/platform-iOS-orange.svg)](https://cocoapods.org/pods/SendBirdUIKit)
[![Languages](https://img.shields.io/badge/language-Swift-orange.svg)](https://github.com/sendbird/sendbird-uikit-ios)
[![Commercial License](https://img.shields.io/badge/license-Commercial-green.svg)](https://github.com/sendbird/sendbird-uikit-ios/blob/main/LICENSE.md)

This demo app showcases what AI Chatbots with Sendbird can do to enhance the customer experience of your service with more personalized and comprehensive customer support.
Utilizing OpenAI’s GPT3.5 and its Function Calling functionality, ***Sendbird helps you build a chatbot that can go extra miles: providing informative responses with the data source you feed to the bot, accommodating customer’s requests such as tracking and canceling their orders, and even recommending new products.*** Create your own next generation AI Chatbot by following the tutorial below.

![fintech](https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/b854bedf-b678-4ebd-9148-9dea2f75ac38)

## Prerequisites
1. [Sendbird Account](https://dashboard.sendbird.com/)
2. Application ID and ChatBot: Please refer to [Step1 ~ Step4](https://sendbird.com/developer/tutorials/create-an-ai-chatbot)
   - [Fintech FAQ Knowledge Base File](https://github.com/sendbird/chat-ai-green-vertical-contents/blob/github-pages/fintech_faq.txt)
     ```
      Q: What is Fintech Demo App?
      A: Fintech Demo App is an all-in-one app that allows users to pay, chat, and explore. It's a digital marketplace platform offering a simple, secure, and reliable experience. Users can chat, transfer money, pay bills, make mobile payments, and engage in other entertainment features.
      
      Q: What can I do with the app?
      A: Users can connect with friends and family via free video and voice calls, share videos, pictures, games, and text messages. You can also discover new friends, pay for services, and soon, listen to music and play games.
      
      Q: Is Fintech Demo App free?
      A: Yes, it is a free digital marketplace app offering data or Wifi-based video and voice calls, as well as other features like music and gaming.
      
      Q: Are there regional restrictions for voice messages?
      A: No, there are no limits. You can use the Fintech Demo App from any location.
      
      Q: How do I register?
     
        ...
     
     ```
## How to open the demo app
1. Open Xcode Demo project
```shell
open Sample/QuickStart.xcodeproj
```
2. Set the `applicationId` and `botId` in [`AppDelegate.swift`](https://github.com/sendbird/fintech-ai-chatbot/blob/main/Sample/QuickStart/AppDelegate.swift#L13-L23)
```swift
static let botId: String = <#botId: String#>  // TODO: set your sendbird bot id
let applicationId: String = <#applicationId: String#>  //  TODO: set your sendbird application id
```

## Table of Contents
1. [Use case: Fintech](#use-case-fintech)
2. [How it works](#how-it-works)
3. [Demo app settings](#demo-app-settings)
4. [System Message](#system-message)
5. [Function Calls](#function-calls)
6. [Welcome Message and Suggested Replies](#welcome-message-and-suggested-replies)
7. [Custom Response](#custom-response)
8. [UI Components](#ui-components)
9. [Limitations](#limitations)

## Use case: Fintech
This demo app demonstrates the implementation of the AI Chatbot tailored for fintech. It includes functionalities such as ***retrieving the failed transactions, retrying failed payments, and providing last transfers.*** By leveraging ChatGPT’s new feature [Function Calling](https://openai.com/blog/function-calling-and-other-api-updates), the Chatbot now can make an API request to the 3rd party with a predefined Function Calling based on the customer’s enquiry. Then it parses and presents the response in a conversational manner, enhancing overall customer experience.

## How it works
<img width="2000" alt="image" src="https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/d4ff5f4c-1df9-40d7-9901-9e3d342d94d8">

1. A customer sends a message containing a specific request on the client app to the Sendbird server.
2. The Sendbird server then delivers the message to Chat GPT.
3. Chat GPT then analyzes the message and determines that it needs to make a Function Calling request. In response, it delivers the relevant information to the Sendbird server.
4. The Sendbird server then sends back a Function Calling request to Chat GPT. The request contains both Function Calling data Chat GPT previously sent and the 3rd party API information provided by the client app. This configuration must be set on the client-side.
5. The 3rd party API returns a response in `data` to the Sendbird server.
6. The Sendbird server passes the `data` to Chat GPT.
7. Once received, Chat GPT analyzes the data and returns proper responses to the Sendbird server as `data`.
8. The Sendbird server passes Chat GPT’s answer to the client app.
9. The client app can process and display the answer with Sendbird Chat UIKit.

***Note***: Currently, calling a 3rd party function is an experimental feature, and some logics are handled on the client-side for convenience purposes. Due to this, the current version for iOS (3.7.0 beta) will see breaking changes in the future, especially for QuickReplyView and CardView. Also, the ad-hoc support from the server that goes into the demo may be discontinued at any time and will be replaced with a proper feature on Sendbird Dashboard in the future.

## Demo app settings
To run the demo app, you must specify `System prompt` and `Function Calls`.

### System Message
`System prompt` defines the Persona of the ChatBot, informing users of the role the ChatBot plays. For this Fintech AI ChatBot, it is designed to be an AI assistant that handles and manages customer orders.

You can find this setting under Chat > AI Chatbot > Manage bots > Edit > Bot settings > Parameter settings > System prompt.

<img width="1149" alt="image" src="https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/d502d03f-fe61-494b-9038-c2dbafb0500c">

- Input example
```
You are a Fintech bot that handles and manages customer transactions. You will be interacting with customers who have queries related to their financial transactions. If the user wants to retry the transaction, Before retrying payment, always ask the user if they would like to proceed with the transaction or not. 

1. Available 24/7 to assist customers with their transaction inquiries.
2. Customers may request to check the status of their failed transactions, refunds, or payment statuses.
3. You have access to the customer's transaction history and the details associated with each transaction.
4. When a customer requests information on a specific transaction, you need to confirm the specific transaction ID or relevant details before proceeding.

If a customer needs further assistance related to Fintech Bot functionalities or other related queries, be ready to provide it.
```

### Function Calls
`Function Calls` allows you to define situations where the ChatBot needs to interface with external APIs. Within `Function Calls`, you need to enter definitions for the Function and Parameters to pass to GPT, and you can define the specs of the 3rd party API to obtain the actual data of the specified Function.

You can find this setting under Chat > AI Chatbot > Function Calls. 

- Example list of Function Calls
  <img width="1231" alt="image" src="https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/6cf69e85-8e10-4eac-8d2f-f3df7472d700">

- Input example
  <img width="1235" alt="image" src="https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/4a0f758e-20ab-45b3-97bf-a55df896ea8b">
  
In addition, you can enhance the user experience by streamlining communication with a Welcome Message, Quick Replies, and Button. Using Quick Replies can clarify your customer’s intentions, as they are presented with a list of predefined options set by you.

Mock API Server Information: [Link](https://documenter.getpostman.com/view/21816899/2s9YBxZbgh)

## Welcome Message and Suggested Replies

The `Welcome Message` is the first message displayed to users by the chatbot. Along with the `Welcome Message`, you can also set up `Suggested Replies` from the Dashboard.

You can find this setting under Chat > AI Chatbot > Manage bots > Edit > Bot settings > Default messages > Welcome message / Suggested replies.

- Input example
  <img width="1148" alt="image" src="https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/c2cd887d-4f18-41b1-85e3-fb8301010e25">

## Custom Response

Through the `Custom Response` feature, you can execute tailored response messages or function calls based on the user's message intent. Additionally, you can also add `Suggested Replies`. 

To allow the chatbot to accurately understand the intent, it's recommended to input about 10 messages. By using the `AI suggestions` feature, this input process can be made much more convenient.

You can find this setting under Chat > AI Chatbot > Custom reponses. 

![custom_response](https://github.com/sendbird/fintech-ai-chatbot/assets/104121286/35a2f669-1027-4842-875c-03e462de314c)

## UI Components
### CardView
The `data` in the response are displayed in a Card view. In the demo, information such as order items and their delivery status can be displayed in a card with an image, title, and description. Customization of the view can be done through `cardViewParamsCollectionBuilder` and `SBUCardViewParams`. The following codes show how to set the Card view of order status.

[SBUUserMessageCell.swift](https://github.com/sendbird/fintech-ai-chatbot/blob/main/Sources/View/Channel/MessageCell/SBUUserMessageCell.swift#L159)
```swift
// MARK: Card List
if let cardListView = self.cardListView {
    self.contentVStackView.removeArrangedSubview(cardListView)
}

let functionResponse = json["function_calls"][0]

if functionResponse.type != .null {
    let statusCode = functionResponse["status_code"].intValue
    let functionName = functionResponse["name"].stringValue
    let response = functionResponse["response_text"]

    if statusCode == 200 {
        if functionName.contains("get_refund_recent") {
            customText = "Here are the refund details:"
            SBUGlobalCustomParams.cardViewParamsCollectionBuilder = { messageData in
                guard let json = try? JSON(parseJSON: messageData) else { return [] }
                
                return json.arrayValue.compactMap { transaction in
                    return SBUCardViewParams(
                        imageURL: nil,
                        title: "Date: \(transaction["timestamp"].stringValue)",
                        subtitle: nil,
                        description: "- Refund ID: \(transaction["refundId"].stringValue)\n- Payment ID: \(transaction["paymentId"].stringValue)\n- Amount: \(transaction["amount"].stringValue) \(transaction["currency"].stringValue)\n- Reason: \(transaction["amount"].stringValue)",
                        link: nil
                    )
                }
            }
            if let items = try?SBUGlobalCustomParams.cardViewParamsCollectionBuilder?(response.rawString()!){
                self.addCardListView(with: items)
            }
        } else if functionName.contains("get_transfer_recent") {
            customText = "Here are the details of your last transfers:"
            SBUGlobalCustomParams.cardViewParamsCollectionBuilder = { messageData in
                guard let json = try? JSON(parseJSON: messageData) else { return [] }
                
                return json.arrayValue.compactMap { transaction in
                    return SBUCardViewParams(
                        imageURL: nil,
                        title: "Date: \(transaction["timestamp"].stringValue)",
                        subtitle: nil,
                        description: "- Transfer ID: \(transaction["transferId"].stringValue)\n- From: \(transaction["fromUserId"].stringValue)\n- To: \(transaction["toUserId"].stringValue)\n- Amount: \(transaction["amount"].stringValue) \(transaction["currency"].stringValue)",
                        link: nil
                    )
                }
            }
            if let items = try?SBUGlobalCustomParams.cardViewParamsCollectionBuilder?(response.rawString()!){
                self.addCardListView(with: items)
            }
        } else if functionName.contains("get_payments_failed") {
            customText = "Here are the details of your recent failed transfers:"
            SBUGlobalCustomParams.cardViewParamsCollectionBuilder = { messageData in
                guard let json = try? JSON(parseJSON: messageData) else { return [] }
                
                return json.arrayValue.compactMap { transaction in
                    return SBUCardViewParams(
                        imageURL: nil,
                        title: "Date: \(transaction["timestamp"].stringValue)",
                        subtitle: nil,
                        description: "- Payment ID: \(transaction["paymentId"].stringValue)\n- User ID: \(transaction["userId"].stringValue)\n- Amount: \(transaction["amount"].stringValue) \(transaction["currency"].stringValue)\n- Payment method: \(transaction["paymentMethod"].stringValue)\n- Reason: \(transaction["amount"].stringValue)",
                        link: nil
                    )
                }
            }
            if let items = try?SBUGlobalCustomParams.cardViewParamsCollectionBuilder?(response.rawString()!){
                self.addCardListView(with: items)
            }
        } else if functionName.contains("retry_payment") {
            SBUGlobalCustomParams.cardViewParamsCollectionBuilder = { messageData in
                guard let json = try? JSON(parseJSON: messageData) else { return [] }

                // Convert the single order object into a SBUCardViewParams object
                let orderParams = SBUCardViewParams(
                    imageURL: nil,
                    title: "Retry payment result",
                    subtitle: nil,
                    description: "- Payment ID: \(json["paymentId"].stringValue)\n- Status: \(json["status"].stringValue)\n- Message: \(json["message"].stringValue)",
                    link: nil
                )

                // Return the SBUCardViewParams object inside an array
                return [orderParams]
            }
            if let items = try?SBUGlobalCustomParams.cardViewParamsCollectionBuilder?(response.rawString()!){
                self.addCardListView(with: items)
            }
        }
    }
} else {
    self.cardListView = nil
}
```

### QuickReplyView
The following codes demonstrate how to set the view for Quick Replies. The values in `quick_replies` of `message.data` are used as Quick Replies.

[SBUUserMessageCell.swift](https://github.com/sendbird/fintech-ai-chatbot/blob/main/Sources/View/Channel/MessageCell/SBUUserMessageCell.swift#L149)
```swift
// MARK: Quick Reply
if let quickReplyView = self.quickReplyView {
    quickReplyView.removeFromSuperview()
    self.quickReplyView = nil
}

if let quickReplies = json["quick_replies"].arrayObject as? [String], !quickReplies.isEmpty {
    self.updateQuickReplyView(with: quickReplies)
}
```

## Limitations
`Tokens`
The maximum number of tokens allowed for `data` is 4027. Make sure that the settings information including the system message does not exceed the limit.
