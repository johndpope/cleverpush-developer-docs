---
id: methods
title: Methods
---

#### <code>triggerOptIn</code>

Triggers the opt-in dialog.
The appearance of the dialog and timing settings can be changed in your CleverPush account under `Channels / Select channel / Settings / Opt-In`.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['triggerOptIn', function(err, subscriptionId) {
    if (err) {
        console.error(err);
    } else {
        console.log('successfully subscribed with id', subscriptionId);
    }
}]);

// set the second parameter to 'true' to skip the opt-in timeout:

CleverPush.push(['triggerOptIn', true, ...]);
```

#### <code>subscribe</code>

Forces a subscription and shows the opt-in dialog immediately, if the user is not subscribed, yet. Callback is required.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['subscribe', function(err, subscriptionId) {
    if (err) {
        console.error(err);
    } else {
        console.log('successfully subscribed with id', subscriptionId);
    }
}]);
```


#### <code>tagSubscription</code>

Tags an existing subscription.
You can also find your personal tagging codes in your CleverPush account under `Channels / Select channel / Settings / Tags`.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['tagSubscription', 'INSERT_TAG_ID']);
```


#### <code>untagSubscription</code>

Untags an existing subscription.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['untagSubscription', 'INSERT_TAG_ID']);
```


#### <code>setTopics</code>

Programmatically sets the user's topics. This is not recommended, because tags should be used for automatic segmetation. Topics should only be managed by the user itself (via the notification bell panel, opt-in or content widget).
This method automatically waits for an active subscription to be available before being executed.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['setTopics', ['TOPIC_ID_1', 'TOPIC_ID_2', '...']]);
```


#### <code>getTopics</code>

Get a user's subscribed topics.
This method automatically waits for an active subscription to be available before being executed.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['getTopics', function (err, topics) {
  console.log(topics);
}]);
```


#### <code>setAttribute</code>

Set's a custom subscription attribute. The attribute needs to be created in the channel settings first. Call this method with the attribute's ID and the desired value afterwards.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['setAttribute', 'ATTRIBUTE_ID', 'VALUE']);
```


#### <code>getAttribute</code>

Gets custom subscription attribute.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['getAttribute', 'ATTRIBUTE_ID', function(value) {
    console.log(value);
}]);
```


#### <code>isSubscribed</code>

Checks if the user is subscribed to push notifications.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['isSubscribed', function(result) {
  console.log('isSubscribed result', result); // true or false
}]);
```


#### <code>unsubscribe</code>

Unsubscribes a subscribed user.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['unsubscribe']);
```


#### <code>triggerFollowUpEvent</code>

Triggers a follow-up campaign event.
You can also find the full event code in your CleverPush account under `Channels / Select channel / Follow-up campaigns`.
Insert the desired follow-up campaign ID and the event name.
The last argument are the event's parameters, which are completely optional. You can specify any parameters and use them in your follow-up notification templates.

```js
CleverPush = window.CleverPush || [];
CleverPush.push(['triggerFollowUpEvent', 'INSERT_FOLLOW_UP_CAMPAIGN_ID', 'EVENT_NAME', { productAmount: 2500.00, productName: 'iMac' }]);
```


<br/>


### Initialization event

You can listen for the global initialization event.
This is very useful if you want to check if the current browser support Web Push notifications and then show a button to manually trigger the opt-in dialog.

Will fail if:
* Browser does not support push notifications
* There is no connection to our API available
* SDK was already initialized

Always make sure to insert this BEFORE your personal CleverPush loader code (`<script src="https://static.cleverpush.com/channel/loader/XXXXXX.js" async></script>`). If this is not possible, you need to check, if CleverPush was already initialized (see the code below).

Example:

```js
var showPushOptIn = function() {
    document.getElementById('#cleverpush-button').style.display = 'block';
}

if (!window.CleverPush || !window.CleverPush.initialized) {
    window.cleverPushInitCallback = function(err) {
        if (err) {
            console.error('Init callback error:', err);
        } else {
            showPushOptIn();
        }
    };
} else {
    showPushOptIn();
}
```


### Subscription Events

Several subscription events can be listened for with one of the following methods (can only be called after successful initialization, see above):

```js
CleverPush.on(event, callback)
CleverPush.once(event, callback)
```

`event` can be:

* 'SUBSCRIBED' (after successful opt-in)
* 'UNSUBSCRIBED' (after successful opt-out)


### Chat

1. Additionally to the general CleverPush script, include the following script:

```html
<script src="https://static.cleverpush.com/sdk/cleverpush-chat.js"></script>
```

2. Place this empty div where you want the chat to appear:

```html
<div class="cleverpush-chat-target"></div>
```