# About

Brief study about Design Pattern Strategy with Java.

# How does it work?

Basically, we can define a common interface for a group of "behaviors" that are related.

Each implementation from this interface will represent a "strategy", and the client can choose which strategy to use.

In this study we are going to see the implementation of this strategy in an if-else code, but that's not the unique
use of this pattern, check it out:

## Where strategy is useful beyond if-else?

Here are some situations:

### Configurable systems

When we need to offer different ways to execute an action, like: 

1. Calculate discounts;
2. Process payment;
3. Determine some validation policy.

Put some code with example.

### Polymorphism behavior

When different objects of a hierarchy needs to implement similar behaviors with variations (WITHOUT USING HIERARCHY).

(Write more).

Put some code with example.

### Modular processing

When different steps of the processing can be altered or combined dynamically.

(Write more).

Put some code with example.

### Complex conditional logic

Replace if-else, making it easier to add new conditions without the need to change the existent code.

(Write more).

Put some code with example.

### Testability

Making it easier for unitary tests, isolating and testing each behavior individually

(Write more).

Put some code with example.


# If-else (service) - example

![img.png](img.png)

This, is going to become this:

![img_1.png](img_1.png)

[Refactoring Guru Strategy Java](https://refactoring.guru/pt-br/design-patterns/strategy/java/example)

## Identifying the problem (Patterns)

The first thing we need to see to know if we need to use this pattern, is if we have a lot of conditionals and depending
on the outcomes, the result are the same.

For example: we need to make a payment, right? If we are going to use a credit card is one system, a debit card is another 
one, pix other service as well.

The implementation of each payment WILL BE different but the purpose is the same, MAKE A PAYMENT.

In our example is a similar concept, we are going to send a notification to different social networks.

So, lets begin!

## Creating the interface

We are going to create an interface. The names as always is what we intend to do + Strategy. Since we want to send
notifications, we are going to use ``NotificationStrategy``.

Each implementation of this interface will be a **DIFFERENT STRATEGY**, but every strategy will need common attributes.

```java
public interface NotificationStrategy {
    
    void sendNotification(String destination, String message);
}
```

Ok, we created the strategy interface, now we need to create **each strategy**! We can create inside the service package,
another one called ``strategy``, and inside this package we can create each one of them.

For example, if we have a notification type that's going to be sended by an email, we create a java class ``Email
NotificationStrategy`` that's going to implement our interface. We can see in the image above that we have: Discord,
Twitter and Instagram notifications as well, so we do the same thing.

We can remove the "if" conditions inside the service and insert inside the method on each Java class.

Now, to use everything inside the service (and method), we need to create a "context", indicating from witch parameter
each strategy will be used, we are going to use ``Map<Key, Value>``. The key will be the channel (instagram, twitter or
discord, for example) and the Value will be the implementation, check it out:

```java
import java.util.Map;

public class NotificationService {


    private final Map<String, NotificationStrategy> mapStrategy = Map.of(
            "discord", new DiscordNotificationStrategy(),
            "instagram", new InstagramNotificationStrategy(),
            "twitter", new TwitterNotificationStrategy(),
            "email", new EmailNotificationStrategy(),
            "whatsapp", new WhatsappNotificationStrategy()
    );
    
    public void notify(String channel, String destination, String message) {
        
        //.get gets the key and by that we can use the method sendNotification, passing the
        //destination and the message
        mapStrategy.get(channel).sendNotification(destination, message);
    }
}
```

Now, if we go to postman, and make a POST passing the channel "instagram/whatsapp/email/discord" it'll work perfectly.

This was a simple method, but it could be a complex one as well, like calling the instagram/whatsapp API.







# Advantages

With this implementation, we could insert new strategies inside the Map, without the need to create a huge code inside
the service.

Furthermore, this method makes it easier to make unitary tests inside the application.