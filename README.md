# go-caster
Go implementation of an internal event notifier for any modern applications

## Usage 

```golang
// Initialize caster
caster.Initialize(caster.Config{
  // Configure caster using the caster.ConfiguratorFunc
})


// Subcribe to an event topic
subscription, err := caster.Subscribe("info:component:method:start", subscriber)

select {
  case subcription.Received <- event:
    // Do something with the event
  case subcription.Cancelled <- event:
    // Do something when the subscription is cancelled 
}

// Build the detail object
details := map[string]string{
  "payload": 1,
}

// Raise an event on a topic with additionnal information
err := caster.Raise("info:component:method:start", details)

// Unsubscription
err := subscription.Cancel()

// Or 
err := caster.Unsubscribe(subcriber)

// You can cancel all subscription on a specific topic
err := caster.CancelAllSubscriptionOn("info:component:method:start")
```

## API 

### caster

#### .Initialize(config)

Configure caster with the given config.

###### ARGS

It takes the following parameters :

Name | Type | Required | Description
---------|----------|---|------
config  | caster.Config | Yes | Contains configuration items for caster

<br>

#### .Subscribe(topic, subj)

Register an object as a subcriber to a channel or a topic.
It adds the subscriber to its internal collections of subcribers
and return a subcription.

###### ARGS

It takes the following parameters :

Name | Type | Required | Description
---------|----------|---|------
topic | string | Yes | topic we want to subcribe to.
subj  | Subscriber | Yes | Object that need to subscribe to a topic

###### RETURNS

 Type         | Description
--------------|---------
 Subscription | Used to capture topic related events
 Error        | An error if something went wrong during subscription

<br>

#### .UnSubscribe(subj)

Unregister a subcriber from any topic it is has subscribed to.
It cancel all subscriptions made for this subscriber.

###### ARGS

It takes the following parameters :

Name | Type | Required | Description
---------|----------|---|------
subj  | Subscriber | Yes | Object has subscribe to topics

###### RETURNS

 Type         | Description
--------------|---------
 Error        | An error if something went wrong during unsubscription


<br>

#### .Raise(topic, extras)

Raises an event for the given topic and passes the extras information as a payload for the event.

###### ARGS

It takes the following parameters :

Name | Type | Required | Description
---------|----------|---|------
topic  | string | Yes | topic on which the event will be sent
extras  | interface{} | Yes | Extras info to consume as an event payload

###### RETURNS

 Type         | Description
--------------|---------
 Error        | If raising fails


<br>

#### .CancelAllSubscriptionOn(topic)

Cancels all subscription made for a specific topic.
Any subscriber that has subscribe to this topic, will be unsubscribed.

###### ARGS

It takes the following parameters :

Name | Type | Required | Description
---------|----------|---|------
topic  | string | Yes | topic on which subscriptions have been made

###### RETURNS

 Type         | Description
--------------|---------
 Error        | If the cancellation fails