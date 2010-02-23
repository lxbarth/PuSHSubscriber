
#PuSH Subscriber

A PHP library for [PubSubHubbub](http://code.google.com/p/pubsubhubbub/)
subscribers.

Requires PHP 5.2 or higher with curl support.

##Introduction

The PuSHSubscriber library is a framework independent PHP library that
implements the PubSubHubbub spec for subscribers.

###Integration with host applications

1) Implement PuSHSubscriberSubscriptionInterface and
PuSHSubscriberEnvironmentInterface.

2) Create a new path in the host application that is unique for every
subscription. For example:

    (http://mysite.com/)pubsub/[subscription_id]

3) In the callback for the new path invoke the subscriber's request handler:

    function my_pubsub_page($subscription_id) {
      $sub = PuSHSubscriber::instance('my_subs', $subscription_id, 'MySubscriptions', new Environment());
      $sub->handleRequest('my_pubsub_notification');
    }

4) Note the 'my_pubsub_notification' parameter in the previous point? This is
the callback that will be invoked if a notification has been received:

    function my_pubsub_notification($raw) {
      // Parse and store the changed items.
    }


###Usage

1) Create a PuSHSubscriber:

    $sub = PuSHSubscriber::instance('my_subs', 12, 'MySubscriptions', new Environment());

Note: The domain id 'my_subs' is merely for allowing multiple tiers in an
application to use the PuSHSubscriber library.

2) Subscribe to a hub for notifications:

    $sub->subscribe('http://example.com/blog/feed', 'http://mysite.com/notifications/12');

3) Unsubscribe from a hub.

    $sub->unsubscribe('http://example.com/blog/feed', 'http://mysite.com/notifications/12');
