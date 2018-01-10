**State1 Initial:** before app sends a request to Subscribe to Vehicle Data
app is not subscribed to any VD
SDL has no stored VD params	

**State2:** after app_1 sends a request to Subscribe to Vehicle Data
app_1 is subscribed to some VD parameters
SDL subscribes app_1 and responds with successful result code to app_1
SDL has stored VD parameters
SDL notifies HMI only about first subscribed application and keep the list of all other subscribed applications internally

**State3:** before app_2 sends a request to Subscribe to Vehicle Data
app_2 is not subscribed to any VD
app_1 is subscribed to some VD parameters
SDL has stored VD parameters

**State4:** after app_2 sends a request  to Subscribe to Vehicle Data
SDL doesn’t transfer this request to HMI but subscribes this application internally with successful result code to app_2
State5: Both app_1 and app_2 are subscribed and receive VD change notification
SDL has 2 (or more than 1) subscribed apps in its list

UNSUBSCRIBE  
State1(initial): SDL has 2 (or more than 1) subscribed apps in its list  

Sate2: app_1 requests to unsubscribe from waypoints change notification 
SDL more than one subscribed applications in SDL list  

State3: SDL must transfer the unsubscription request for app_1 to HMI 
and send subscription request to HMI for app_2 (meaning for any other application which is the next one in the list of subscribed apps)
