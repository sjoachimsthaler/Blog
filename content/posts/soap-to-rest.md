+++
title = "Soap to Rest"
draft = false
date = "2022-03-22T22:50:01+01:00"
author = "Stefan Joachimsthaler"
authorTwitter = "" #do not include @
cover = ""
tags = ["Dotnet", "SOAP", "Rest"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = true
+++

# How to convert an old SOAP Service to a (more) modern REST-Service

So you inherited a legacy app that still runs on asmx or WCF services on DotNet Framework? This is what just hapenned to me. 

For many the first intention is to just rewrite the whole application. Especially when there are a lot of problems in the codebase. But please, do not give in to that urge. Often there are so many hidden features in the codebase and nobody remembers the features that are needed and implemented. So usually the best way to update the old codebase is to transfer it to a new technology.

## What are the target options?
Here are some possible options, but as the title indicates: I would prefer to move to a REST-based Web API

### GraphQL
This would be a good choice if you have complex queries that are run through the service. But it would be a lot of effort to convert an old service to a GraphQL Service and many methods won't be able to be called like they used to be.

### gRPC
gRPC sounds like a great solution. Especially if you are trying to update an existing WCF service, as gRPC has a lot of features WCF services have. And it is also quite efficient, as the data is not transfered in a humand readable, but in a binary format.

But there is one big downside for my case: I cannot run a gRPC server in an old Dotnet Framework app. This means that I still have a huge problem if I want to convert a big codebase step by step.

### REST
Let me clarify it right away: I am not suggesting you should convert your old services to a truly RESTful service, but to just use a small part of it and build upon that.

The big benefit of using REST: There are a ton of tools that help you and the migration path is quite simple.

## How to move to a REST service

1. Move your logic further down the road. Your services should be clean of logic. All that belongs in a service (no matter the type) is the wiring that is needed to call the methods in the layers down below. Maybe some inupt validation and error handling. But not more!
2. If you have some elements that cannot be moved to e.g. the business layer then I suggest you create a new project that contains this logic, and move those elements there. This allows you to use those elements in both, your old service and the new project we are about to create.
3. Create a new Web API project that refernces the same projects that the old service project does.
4. You can not create new controller methods that are equal to your old service methods. As the logic is down below the controller methods are really lean and should basically write themselves. In my opinion you should just move every service to a POST request.

Then you have the chance to serve both services in parallel to ensure a smooth transition. You can already migrate a specific usergroup to the new service and check if everything works as expected.

When you are sure that your new service works you can abandon the old service and delete the old project.

The new REST service can now be modified to be more RESTful or you can now start to modify your new service.