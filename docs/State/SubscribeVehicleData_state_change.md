### SubscribeVehicleData state change

#### State_1

||app_1|app_2|SDL|
|---|---|---|---|
|**Initial state**|not subscribed|not subscribed|has no subscribed app in internal list<br> has no stored VD params|
|**Input**|sends a request to subscribe|-|-|
|**Changed state**|subscribed<br>receives VD change notifications|not subscribed|has app_1 in the list of subscribed applications<br> has stored VD params|

#### State_2
||app_1|app_2|SDL|
|---|---|---|---|
|**Initial state**|subscribed<br>receives VD change notifications|not subscribed|has app_1 in the list of subscribed applications<br> has stored VD params|
|**Input**|-|sends a request to subscribe|doesn’t transfer this request to HMI but subscribes app_2 internally|
|**Changed state**|subscribed<br>receives VD change notifications|subscribed<br>receives VD change notifications|has app_1, app_2 in the list of subscribed applications<br> has stored VD params|

### UnsubscribeVehicleData state change
#### State_1
||app_1|app_2|SDL|
|---|---|---|---|
|**Initial state**|subscribed<br>receives VD change notifications|not subscribed|has app_1 in the list of subscribed applications<br> has stored VD params|
|**Input**|sends a request to unsubscribe|-|-|
|**Changed state**|not subscribed<br> does not receive VD change notifications|not subscribed|has no subscribed app in internal list<br> has no stored VD params|

#### State_2
||app_1|app_2|SDL|
|---|---|---|---|
|**Initial state**|subscribed<br>receives VD change notifications|subscribed<br>receives VD change notifications|has app_1, app_2 in the list of subscribed applications<br> has stored VD params|
|**Input**|-|sends a request to unsubscribe|doesn’t transfer this request to HMI but unsubscribes app_2 internally|
|**Changed state**|subscribed<br>receives VD change notifications|not subscribed|has app_1 in the list of subscribed applications<br> has stored VD params|
