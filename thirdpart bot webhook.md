
## Bot webhook
  - [Bot Answer Requested](#bot-answer-requested)

When visitor sent a question/intent link clicked event/visitor location shared event/helpful or not-helpful clicked event/form information collected event through live chat, we will pass this question/intent link clicked event/visitor location shared event/helpful or not-helpful clicked event/form information collected event and other information we defined to this webhook. You need process this question/intent link clicked event/visitor location shared event/helpful or not-helpful clicked event/form information collected event and information we provided within this webhook using your own Bot engine and give us a formatted response so that we can give visitor an answer base on your response through live chat. 

  - [Bot Greeting Message Requested](#bot-greeting-message-requested)

When a chat allocated to bot, live chat will request for the greeting message from bot, then bot pass the request and other pre- defined information to the webhook. The webhook need process this request and other pre- defined information within the webhook using your own Bot engine and send back a formatted greeting message response so that we can give visitor a greeting message base on your response through live chat.


### Bot Answer Requested

#### Request data

  - `event ` -  visitor.question.asked / intent.link.clicked / bot.answer.rated / visitor.location.shared / form.collected
  - `unique_id ` -  it is the unique id of the event. 
  - `time ` -  event happens time
  - [Webhook Request](#webhook-request)
  - [visitorInfo](#VisitorInfo)

#### Response data
  - `type` - string , contains  highConfidenceAnswer, possibleAnswer, noAnswer
  - `answer` - an array of [Response](#response)
  
    [Sample Json](#response-sample-json)


### Bot Greeting Message Requested

#### Request data

  - `event ` -  visitor.question.asked / intent.link.clicked / bot.answer.rated / visitor.location.shared / form.collected
  - `unique_id ` -  it is the unique id of the event. 
  - `time ` -  event happens time
  - `sessionId ` -  id of the session
  - `campaignId` - id of the campaign in comm100 live chat
  - `questionId` - id of originall question
  - `intentId` - id of intent which visitor clicked,it is originally from the response of the webhook [Visitor question Sent](#visitor-question-sent), another [Intent link clicked](#intent-link-clicked), [Location Collected](#location-collected), [Information Collection](#information-collected)
  - [visitorInfo](#VisitorInfo)

#### Response data
  - `type` - string , contains  highConfidenceAnswer, possibleAnswer, noAnswer
  - `answer` - an array of [Response](#response)

    [Sample Json](#sample-json)


### Custom Bot webhook Related Object Json Format

#### Response
Response is represented as simple flat json objects with the following keys:

|Name| Type| Read-only    |Mandatory | Description     | 
| - | - | - | - | - | 
|`type` | string | no | yes |enums contain text,image,video, quickreply, button, collectLocation, collectInformation.  | 
|`id` | string | no | yes |id of current response.  | 
|`content` | object | no | yes |response's content. when type is text, it represents [TextResponse](#textresponse);when type is image ,it represents [ImageResponse](#imageresponse);when type is video, it represents [VideoResponse](#videoresponse); when type is quickreply, it represents [QuickReplyResponse](#quickreplyresponse); when type is button, it represents [ButtonResponse](#buttonresponse); when type is collectLocation, it should be null; when type is collectInformation, it represents [CollectInformationResponse](#collectinformationresponse)| 

#### TextResponse
  TextResponse is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `message` | string  | no | yes | text of the response |
  | [linkInfo](#linkinfo) | object  | no | no | link information of the text |  

#### LinkInfo
  TextResponse is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `type` | enums | no | yes | enums contain weblink and goToIntent |
  | `startPos` | int | no | yes | start index of text which contains link info |
  | `endPos` | int | no | yes | end index of text which contains link info |
  | `url` | string | no | yes when type is weblink | url of the web resource that you want user to open,including web forms,articles,images,video,etc. |
  | `intentId` | string| no | yes when type is goToIntent | id of intent that you want user to click. |
  | `intentName` | string| no | yes when type is goToIntent | name of intent that you want user to click. |
  | `openIn` | enums | no | yes when type is weblink | enums contain currentWindow,sideWindow,newWindow. This field defined the way that webpage will be opened. |

#### ImageResponse

  ImageResponse is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `name` | string  | no | yes | name of the image |
  | `url` | string  | no | yes | url of the image |  

#### VideoResponse
  VideoResponse is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `url` | string  | no | yes | url of the video |

#### QuickReplyResponse
  QuickReplyResponse is represented as simple flat JSON objects with the following keys:

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `message` | string  | no | yes | text of the response|
  | `quickReplyItems`| an array of [QuickReplyItem](#quickreplyitem)  | no | no | link information of the text|  

#### QuickReplyItem
  QuickReplyItem is represented as simple flat JSON objects with the following keys: 

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `type` | string  | no | yes | enums contain  goToIntent, contactAgent, text|
  | `name`| string  | no | yes | text on quick reply |
  | `intentId`| string  | no | yes when type is goToIntent  | id of the intent which current quickreply point to |
  | `intentName`| string  | no | yes when type is goToIntent  | name of the intent which current quickreply point to |  

#### ButtonResponse
  ButtonResponse is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `message` | string  | no | yes | text of the response|
  | `buttonItems`| an array of [ButtonItem](#buttonItem)  | no | no | link information of the text|  

#### ButtonItem
  QuickReplyResponse is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `type` | string  | no | yes | enums contain  enums contain weblink,webview and goToIntent|
  | `text`| string  | no | no | text on button |
  | `url` | string | no | yes when type is weblink or webview | url of the web resource that you want user to open,including web forms,articles,images,video,etc.|
  | `intentId`| string  | no | yes when type is goToIntent | id of the intent which current quickreply point to |
  | `intentName`| string  | no | yes when type is goToIntent | name of the intent which current quickreply point to |
  | `openIn` | enums | no | yes when type is weblink | enums contain currentWindow,sideWindow,newWindow. This field defined the way that webpage will be opened.|
  | `openStyle` | enums | no | yes when type is webview | enums contain compact, tall, full. This field defined the way that webview will be opened.|


#### CollectInformationResponse
  CollectInformationResponse is represented as simple flat JSON objects with the following keys:  
  
  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `text` | string  | no | yes | text on the button which can be clicked to open a webview to collection information|
  | `message` | string  | no | yes | message of the response which will be displayed upon the button|
  | `isNeedConfirm` | bool  | no | yes | whether need to confirm after webview submit|
  | `fields` | an array of [Field](#field)  | no | yes | fields displayed on webview|

#### Field

  Field is represented as simple flat JSON objects with the following keys:  
  
  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `Id` | int  | no | yes | id of the field |
  | `name` | string  | no | yes | name of the field|
  | `value` | string  | no | yes | value of the field|
  | `type` | string  | no | yes | field type, contains text, textArea, radio, checkBox, dropDownList, checkBoxList |
  | `isRequired` | bool  | no | yes | when it is true, visitor have to input a value in the field before submit |
  | `isMasked` | bool  | no | yes | when it is true, information collected will replaced by * in chat log for security |
  | `option` | an array of string  | no | yes when type is dropDownList, checkBoxList | values displayed in the field when type is dropDownList, checkBoxList for visitor to choose|

#### Webhook Request

  Webhook Request is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `sessionId` | string | yes | no | id of the session |
  | `campaignId` | int | no | yes | id of the campaign in comm100 live chat |
  | `question` | string | no | yes | the last question that Bot receives from visitor |
  | `questionId` | string | no | yes | id of current question |
  | `intentId` | int | no | yes | id of intent which visitor clicked,it is originally from the response of the webhook [Visitor Message Sent](#visitor-message-sent), another [Intent link clicked](#intent-link-clicked), [Location Collected](#location-collected), [Information Collection](#information-collected) |
  | `isHelpful` | bool | no | no | true or false |

#### VisitorInfo

  Visitor info is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `id` | integer | yes | no | id of the visitor |
  | `name` | string | no | yes | name of the visitor |
  | `language` | string | no | yes | language |
  | `email` | string | no | yes | email of the visitor |
  | `phone` | string | no | yes | phone of the visitor |
  | `longitude` | float | no | no | longitude of the visitor location |
  | `latitude` | float | no | no | latitude of the visitor location |
  | `country` | string | no | yes | the country of the visitor |
  | `state` | string | no | yes | state of the visitor |
  | `city` | string | no | yes | the city of the visitor |
  | `company` | string | no | yes | the company of the visitor |
  | `department` | int | no | yes | department of the visitor |
  | `browser` | string | no | yes | visitor use browser type |
  | `current_browsing` | string | no | yes | page of the current browsing |
  | `referrer_url` | string | no | yes | referrer url |
  | `landing_page` | string | no | yes | the page of login |
  | `search_engine` | string | no | yes | search engine |
  | `keywords` | string | no | yes | search engine key |
  | `operating_system` | string | no | yes | operating system of the visitor |
  | `ip` | string | no | yes | ip of the visitor |
  | `flash_version` | string | no | yes | version of the flash |
  | `product_service` | string | no | yes | product service |
  | `screen_resolution` | string | no | yes | screen resolution |
  | `time_zone` | string | no | yes | time zone of the visitor |
  | `first_visit_time` | string | no | yes | the time of first visit |
  | `visit_time` | string | no | yes | time of the visitor |
  | `visits` | integer | no | yes | count of the visited |
  | `chats` | integer | no | yes | count of chat |
  | `page_views` | integer | no | yes | count of the visited |
  | `status` | string | no | yes | status of the visitor |
  | `custom_fields` | [CustomFields](#customfields) | no | yes | an array of custom fields |
  | `custom_variables` | [CustomVariables](#customvariables) | no | yes | an array of custom variables |

#### CustomFields

  Custom fields is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `id` | integer  | yes | no | id of the field |
  | `name` | string  | no | yes | name of the field |
  | `value` | string  | no | yes | value of the field |

#### CustomVariables

  Custom variables is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `name` | string  | no | yes | name of the variable |
  | `value` | string  | no | yes | value of the variable |

  #### Response Sample Json
  ```json
  {
    "type": "highConfidenceAnswer",// highConfidenceAnswer, possibleAnswer or noAnswer
    "answer": [
        {
            "id": "1",
            "type": "text",
            "content": {
                "message": "this is a plain message"
            }
        },
        {
            "id": "2",
            "type": "text",
            "content": {
                "message": "this is a web link message",
                "linkInfo": {
                    "type": "hypelink",// hypelink or goToIntent.
                    "startPos": 10,
                    "endPos": 17,
                    "ifPushPage": true,
                    "url": "www.test.com",
                    "openIn": "currentWindow"// currentWindow, sideWindow or newWindow.
                }
            }
        },
        {
            "id": "3",
            "type": "text",
            "content": {
                "message": "this is a go to intent message",
                "linkInfo": {
                    "type": "goToIntent",
                    "startPos": 10,
                    "endPos": 17,
                    "intentId": "test-intent-id",
                    "intentName": "test-intent-name"
                }
            }
        },
        {
            "id": "4",
            "type": "image",
            "content": {
                "name": "test-image.jpg",
                "url": "www.test.com/test-image.jpg"
            }
        },
        {
            "id": "5",
            "type": "video",
            "content": {
                "url": "www.test.com/test-video.jpg"
            }
        },
        {
            "id": "6",
            "type": "quickreply",
            "content": {
                "message": "this is a quick reply response",
                "quickReplyItems": [
                    {
                        "type": "goToIntent",// goToIntent, contactAgent or text.
                        "name": "click to trigger test-intent-name",
                        "intentId": "test-intent-id",
                        "intentName": "test-intent-name"
                    },
                    {
                        "type": "contactAgent",
                        "name": "click to contact agent"
                    },
                    {
                        "type": "text",
                        "name": "click to send this text"
                    }
                ]
            }
        },
        {
            "id": "7",
            "type": "button",
            "content": {
                "message": "this is a button response",
                "buttonItems": [
                    {
                        "type": "goToIntent",// goToIntent, weblink or webview.
                        "text": "click to trigger test-intent-name",
                        "intentId": "test-intent-id",
                        "intentName": "test-intent-name"
                    },
                    {
                        "type": "weblink",
                        "text": "click to open this url in web page",
                        "url": "www.test.com",
                        "openIn": "currentWindow"
                    },
                    {
                        "type": "webview",
                        "text": "click to open this url in web view",
                        "url": "www.test.com",
                        "openStyle": "full"
                    }
                ]
            }
        },
        {
            "id": "8",
            "type": "collectLocation",
            "content": null
        },
        {
            "id": "9",
            "type": "collectInformation",
            "content": {
                "text": "book ticket",
                "message": "please fill the form below",
                "isNeedConfirm": "true",
                "fields": [
                    {
                        "Id": 1,
                        "type": "text",// text, textArea, radio, checkBox, dropDownList or checkBoxList
                        "name": "field-1",
                        "value": "",
                        "isRequired": true,
                        "isMasked": true
                    },
                    {
                        "Id": 2,
                        "type": "dropDownList",
                        "name": "field-2",
                        "value": "",
                        "isRequired": true,
                        "isMasked": true,
                        "option": [
                            "value": -1,
                            "value": -2,
                            "value": -3
                        ]
                    }
                ]
            }
        }
    ]
}
```
