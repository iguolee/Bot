
## Custom Bot Webhook Event
  - Visitor Question Asked Event

When visitor sent a message through live chat, we will pass this message and other information we defined to this webhook. 
You need process this message and information within this webhook using your own bot engine and give us a formatted 
response so that we can give visitor an answer base on your response through live chat.  

  - Intent Link Clicked Event

If the answer we give to visitor contains link/button/quickreply which point to an intent, when visitor click this link/button/quickreply, we will pass this action to this webhook with intent id and other information we defined. You need process this action within this webhook and give us a formatted response so than we can give an answer to visitor base on your response through live chat.

  - Bot Answer Rated Event

Visitor can click helpful or not-helpful button to rate our answers. When visitor clicked these buttons, we will pass this action to this webhook, you can use this webhook to collect information about your bot’s correctness and improve your bot’s experience. Also, we need a formatted response from this webhook to give visitor a message for his/her rating.

  - Visitor Location Shared Event

When we received a response whose event type is visitor.location.shared, we will display an webview for visitor to collect his/her location, when visitor shared his/her location to us, we will pass these information to this webhook and you can give us a response base on nformation we provided through this webhook.

  - Form Information Collected Event

When we received a response whose event type is form.collected, we will display an webview for visitor to collect more information about him/her, when visitor filled out webview, we will pass these information to this webhook, and you can give us a response based on information we provided through this webhook.
.;[=
  - Bot Chat Started Event

When we received a response whose event type is bot.greetingMessage.requested, e will pass this action to this webhook, You need process this action within this webhook and give us a formatted response so than we can give an answer to visitor base on your response through live chat.

### Request and Response Data of Custom Bot webhook 

#### Request data

  - `event ` -  visitor.question.asked / intent.link.clicked / bot.answer.rated / visitor.location.shared / form.collected / bot.chat.started
  - `unique_id ` -  it is the unique id of the event. 
  - `time ` -  event happens time
  - [Webhook Request Data](#webhook-request-data)
  - [visitorInfo](#VisitorInfo)

#### Response data
  
  - `answer` - an array of [Response](#response)
  
    [Sample Json](#response-sample-json)


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
  | `items`| an array of [QuickReplyItem](#quickreplyitem)  | no | no | link information of the text|  

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
  | `items`| an array of [ButtonItem](#buttonItem)  | no | no | link information of the text|  

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
  | `ifNeedConfirm` | bool  | no | yes | whether need to confirm after webview submit|
  | `fields` | an array of [Field](#field)  | no | yes | fields displayed on webview|

#### Field

  Field is represented as simple flat JSON objects with the following keys:  
  
  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `name` | string  | no | yes | name of the field|
  | `value` | string  | no | yes | value of the field|
  | `type` | string  | no | yes | field type, contains text, textArea, radio, checkBox, dropDownList, checkBoxList |
  | `ifRequired` | bool  | no | yes | when it is true, visitor have to input a value in the field before submit |
  | `ifMasked` | bool  | no | yes | when it is true, information collected will replaced by * in chat log for security |
  | `options` | an array of string  | no | yes when type is dropDownList, checkBoxList | values displayed in the field when type is dropDownList, checkBoxList for visitor to choose|

#### Webhook Request Data

  Webhook Request is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `chatId` | string | yes | yes | | id of the session |
  | `campaignId` | int | yes | yes | id of the campaign in comm100 live chat |
  | `question` | string | yes | yes | the last question that Bot receives from visitor |
  | `intentId` | int | no | yes | id of intent which visitor clicked,it is originally from the response of the webhook [Visitor Message Sent](#visitor-message-sent), another [Intent link clicked](#intent-link-clicked), [Location Collected](#location-collected), [Information Collection](#information-collected) |
  | `isHelpful` | bool | yes | no | true or false |
  | `formCollectedValues` | array of [Form Collected Values](#form-collected-values) | yes | no | true or false |
  
#### Form Collected Values

  Form Value is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | :-: | :-: | - | 
  | `lable` | string  | no | yes | lable of the formValue |
  | `value` | string  | no | yes | value of the formValue |
  
#### VisitorInfo

  Visitor info is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `id` | integer | yes | yes | id of the visitor |
  | `name` | string | yes | yes | name of the visitor |
  | `language` | string | yes | yes | language |
  | `email` | string | yes | yes | email of the visitor |
  | `phone` | string | yes | yes | phone of the visitor |
  | `longitude` | float | yes | no | longitude of the visitor location |
  | `latitude` | float | yes | no | latitude of the visitor location |
  | `country` | string | yes | yes | the country of the visitor |
  | `state` | string | yes | yes | state of the visitor |
  | `city` | string | yes | yes | the city of the visitor |
  | `company` | string | yes | yes | the company of the visitor |
  | `department` | int | yes | yes | department of the visitor |
  | `browser` | string | yes | yes | visitor use browser type |
  | `current_browsing` | string | yes | yes | page of the current browsing |
  | `referrer_url` | string | yes | yes | referrer url |
  | `landing_page` | string | yes | yes | the page of login |
  | `search_engine` | string | yes | yes | search engine |
  | `keywords` | string | yes | yes | search engine key |
  | `operating_system` | string | yes | yes | operating system of the visitor |
  | `ip` | string | yes | yes | ip of the visitor |
  | `flash_version` | string | yes | yes | version of the flash |
  | `product_service` | string | yes | yes | product service |
  | `screen_resolution` | string | yes | yes | screen resolution |
  | `time_zone` | string | yes | yes | time zone of the visitor |
  | `first_visit_time` | string | yes | yes | the time of first visit |
  | `visit_time` | string | yes | yes | time of the visitor |
  | `visits` | integer | yes | yes | count of the visited |
  | `chats` | integer | yes | yes | count of chat |
  | `page_views` | integer | yes | yes | count of the visited |
  | `status` | string | yes | yes | status of the visitor |
  | `custom_fields` | [CustomFields](#customfields) | yes | yes | an array of custom fields |
  | `custom_variables` | [CustomVariables](#customvariables) | yes | yes | an array of custom variables |

#### CustomFields

  Custom fields is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `id` | integer  | yes | yes | id of the field |
  | `name` | string  | yes | yes | name of the field |
  | `value` | string  | yes | yes | value of the field |

#### CustomVariables

  Custom variables is represented as simple flat JSON objects with the following keys:  

  | Name | Type | Read-only | Mandatory | Description |    
  | - | - | - | - | - | 
  | `name` | string  | yes | yes | name of the variable |
  | `value` | string  | yes | yes | value of the variable |

  #### Response Sample Json
  ```json
   [
        {
            "type": "text",
            "content": {
                "message": "this is a plain message"
            }
        },
        {
            "type": "text",
            "content": {
                "message": "this is a web link message",
                "link": {
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
            "type": "text",
            "content": {
                "message": "this is a go to intent message",
                "link": {
                    "type": "goToIntent",
                    "startPos": 10,
                    "endPos": 17,
                    "intentId": "test-intent-id",
                    "intentName": "test-intent-name"
                }
            }
        },
        {
            "type": "image",
            "content": {
                "name": "test-image.jpg",
                "url": "www.test.com/test-image.jpg"
            }
        },
        {
            "type": "video",
            "content": {
                "url": "www.test.com/test-video.jpg"
            }
        },
        {
            "type": "quickreply",
            "content": {
                "message": "this is a quick reply response",
                "items": [
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
            "type": "button",
            "content": {
                "message": "this is a button response",
                "items": [
                    {
                        "type": "goToIntent",// goToIntent, hyperlink or webview.
                        "text": "click to trigger test-intent-name",
                        "intentId": "test-intent-id",
                        "intentName": "test-intent-name"
                    },
                    {
                        "type": "hyperlink",
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
            "type": "location",
            "content": null
        },
        {
            "type": "form",
            "content": {
                "text": "book ticket",
                "message": "please fill the form below",
                "ifNeedConfirm": "true",
                "fields": [
                    {
                        "type": "text",// text, textArea, radio, checkBox, dropDownList or checkBoxList
                        "name": "field-1",
                        "value": "field-value-1",
                        "ifRequired": true,
                        "ifMasked": true
                    },
                    {
                        "type": "dropDownList",
                        "name": "field-2",
                        "value": "field-value-2",
                        "ifRequired": true,
                        "ifMasked": true,
                        "options": [
                            "value": -1,
                            "value": -2,
                            "value": -3
                        ]
                    }
                ]
            }
        }
    ]
```
