# go-caster

Go implementation of an internal event notifier for any modern applications

See [Our Moviations doc](docs/caster/motivations.md) for more information about why we build this tool and understand how things work under the hood.

## Contents

* [Install]
* [Usage]
* [API]
  * [caster]
  * [subcription]
* [Examples]
* [Tests]
* [Guidelines]
* [Contribute]
* [FAQ]
* [References]
* [Maintainers]
* [License]

## Install 

To get the library runs : 

```bash
go get -u github.com/lktslionel/go-caster
```


## Usage 

```golang
// Initialize caster
caster.Initialize(caster.Config{
  // Configure caster using the caster.ConfiguratorFunc
})


// Subcribe to an event topic
subscription, err := caster.Subscribe("info:component:method:start", subscriber)

type Subscriber interface {
  func EventReceived(e *Event, s *Subscription) 
  func SubscriptionCancelled(s *Subscription)
}

subscription.WhenReceiveEvent(func(event *Event, s Subscriber){
  select {
    case event := <- this.Received:
      // Do something with the event
    case event := <- this.Cancelled:
      // Do something when the subscription is cancelled 
  }
})

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

---

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

---

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


---

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


---

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

---
<br>

### Subscription

#### .Cancel()

Cancels the current subscription.
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


## Examples


## Tests


## Guidelines

See the [guidelines doc].

## Contribute

See the [contributing doc].

## FAQ

See the [faq doc].

## References


## Maintainers

* Lionel T. [@lktslionel](https://twitter.com/lktslionel)

## License
 
[MIT license]


[Install]: #Install
[Usage]: #Usage
[API]: #API
[caster]: #caster
[subcription]: #Subcription
[Examples]: #Examples
[Tests]: #Tests
[Guidelines]: #Guidelines
[Contribute]: #Contribute
[FAQ]: #FAQ
[References]: #References
[Maintainers]: #Maintainers
[License]: #License
[Changelog]: docs/CHANGELOG.md
[contributing doc]: docs/CONTRIBUTE.md
[guidelines doc]: docs/GUIDELINES.md
[faq doc]: docs/FAQ.md
[MIT license]: LICENSE