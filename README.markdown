
#PuSH Subscriber

A PHP library that implements the
[PubSubHubbub](http://code.google.com/p/pubsubhubbub/) spec for subscribers.
Framework independent.


##Requirements

PHP 5.2 or higher with curl support.


##Integration with host applications

Before you can start using the PuSHSubscriber library you need to prepare your
host application following these four steps:

1) Implement PuSHSubscriberSubscriptionInterface and
PuSHSubscriberEnvironmentInterface (see PuSHSubscriber.inc).

2) Create a new path in the host application that is unique for every
subscription. For example:

    (http://mysite.com/)pubsub/[subscriber_id]

3) In the callback for the new path invoke the subscriber's request handler:

    // MySubscription and MyEnvironment are the interfaces implemented in 1)
    function my_pubsub_page($subscription_id) {
      $domain = 'my_subs';
      $sub = PuSHSubscriber::instance($domain, $subscriber_id, 'MySubscription', new MyEnvironment());
      $sub->handleRequest('my_pubsub_notification');
    }

4) Note the 'my_pubsub_notification' passed to the request handler? This is
the callback that will be invoked if a notification has been received:

    function my_pubsub_notification($raw, $domain, $subscriber_id) {
      // Parse $raw and store the changed items for the subscription identified
      // by $domain and $subscriber_id
    }


##Usage

1) Create a subscription:

    $sub = PuSHSubscriber::instance('my_subs', 12, 'MySubscription', new MyEnvironment());

Note: The domain id 'my_subs' is merely for allowing multiple tiers in an
application to use the PuSHSubscriber library simultaneously. MySubscription
is an implementation of PuSHSubscriberSubscriptionInterface and MyEnvironment is
an implementation of PuSHSubscriberEnvironmentInterface.

2) Subscribe to a hub for notifications:

    $sub->subscribe('http://example.com/blog/feed', 'http://mysite.com/notifications/12');

3) Unsubscribe from a hub:

    $sub->unsubscribe('http://example.com/blog/feed', 'http://mysite.com/notifications/12');
