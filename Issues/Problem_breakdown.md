 [Morgan] Okay, so we've just heard from our customer, and there are a few key points I want to discuss before we begin designing this architecture. The customer stated that they're moving from an application hosted on premises to a service-oriented application hosted on AWS. Some services have already been migrated to AWS, and they're looking for advice on their order service, which currently includes a couple of different functions, and is acting as one monolithic code base. Based on my notes, this is what their on-premises solution looks like right now. It's good to keep this architecture in mind as we go along, so that we don't forget anything. Now, a big takeaway I heard from the customer, is that they want this service to be more reliable and resilient, so that if one part of the application fails, it doesn't impact the other processes running downstream. The current problem is that the orders acceptance and downstream calls are all handled by one code package as a monolith, and we need to break that into multiple components, and have them loosely coupled to avoid having a single point of failure, like they have now.
Play video starting at :1:7 and follow transcript1:07
Now, for the requirements that we gathered. First off, managed scaling for compute and database components, when possible, is going to be important. This one makes me immediately think that we will be using serverless services as much as possible. Serverless services scale in and out with demand on their own, without the customer needing to step in. For some services, you can set parameters around how scaling happens, or set limits to scaling, but it's totally managed behind the scenes. So, I'm going to keep serverless top of mind as we go through the different potential components, which, spoiler alert, means I probably won't be looking at EC2 a lot for this solution. Now, for the next requirement, we have decoupling components to maximize resilience. This one is really about eliminating single points of failure for the app. I'm thinking of breaking the order service down into multiple components, and following an event-driven architecture, so we can more loosely couple each piece. What I mean by that, is that the order acceptance, order processing, and then the downstream calls, should all be independent of each other, so that if one has a hiccup or fails, it won't take down everything else. We'll keep talking about this throughout the week. Another requirement from the customer is that they wanted centralized monitoring and logging. They want to be able to see the state of their application all in one place, and have a centralized point to come to for debugging or troubleshooting. This won't be a problem, especially with the serverless services, which all tend to be integrated with Amazon CloudWatch and Amazon CloudWatch Logs. Then finally, there's our last requirement, which is all about optimizing for cost, performance efficiency, and operational overhead. Cost is going to be one of the main drivers for what services I choose, and then, secondarily, performance efficiency and operational overhead. It can be difficult to balance these things. But again, serverless really tends to optimize in the direction of these three things anyways, so I'm not too concerned here. We will build out the architecture first, piece by piece, put it all together, and then circle back and talk about the different choices that we can make to optimize for cost and performance.