# Urgent Help Needed: Order Processing System Not Working

Hi Support team,

We've been trying to get our order processing system working with the Dapr pub/sub API, but we're running into issues. The messages aren't getting through to our processing service, and we can't figure out why. We've spent the last two days trying to debug this. Things have become worse after we moved the Redis host configuration to a local secret store. Now the apps won't even start :(

## Our Setup

We have two microservices built with .NET:

1. An Order Publisher service that accepts orders through a REST API
2. An Order Processor service that should receive and process these orders

The code is in our repository with this structure:

```text
📁 src/
├── Publisher/      # REST API to submit orders
└── Subscriber/     # Order processing service
```

We're using Dapr's pub/sub building block with Redis (the default one that comes with Dapr). Our component configurations are in the `components` directory.

## How to Run Our Services

First service (Publisher):

```bash
dapr run --app-id publisher --app-port 6500 --dapr-http-port 3500 --resources-path ../../components/ -- dotnet run
```

The second (subscriber) service runs on port 6501 and should be run with app-id subscriber.

## What was Happening when the apps were running

1. When we submit an order through the API (http://localhost:6500/order), the Publisher service accepts it and returns a success message. We've been testing this by running the following:

```bash
curl -X POST http://localhost:6500/order -H "Content-Type: application/json" -d '{
    "items": [
        {
            "productId": "PROD-1",
            "quantity": 2
        }
    ]
}'
```

2. The order details look correct in the Publisher's logs
3. But the Subscriber service never receives or processes the order

## What We've Tried

1. The services fail to initialize with Dapr but we don't know why!
2. Before, we could see in the logs that the Publisher service was receiving the orders
3. We've checked that Redis is running (using the default Dapr Redis)
4. We're now using the local secret store for Redis configuration

Could you please help us figure out what's wrong? This is blocking our development team from moving forward.

## Additional Context

- We're using .NET 8.0
- Dapr version: 1.14.4
- All components are deployed locally for development

Thank you for your help!
