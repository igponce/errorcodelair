# Google Cloud Platform

## Pubsub

### 400 This method is not supported for push subscription

You are trying to read data from a subscription that is configured as Push, rather than pull.

Push subscriptions _send_ the messages through a configured https endpoint: there is no need to *read* the data.

If you need to read data from a *pubsub topic* , you must use a *pull* subscription instead.

Remember: messages get published into a pubsub topic. You can have more than one subscription to read that messages.

If you have a push subscription, probably it is also a good idea to have a pull one configured (with low retention = because it cost money) just to be able to see what was sent by the push subscription.

### 503 Deadline Exceeded

When reading from pubsub with 

References: 