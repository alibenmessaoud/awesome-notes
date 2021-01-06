# Choosing the right pattern

It is always difficult to choose an architecture pattern these days when we start a new project. We need to study and take in consideration many factors as we can get confused easily with all the hype around different technologies.

The following text provides some advises when to choose a microservice architecture over a monolithic web application architecture and vice versa.

## When to choose a monolithic architecture

- Market: time to market is critical

- Application context: small scope, well defined features

  For example, a blog, a simple online shopping website, a simple CRUD application.

- Users: small and stable users base

  For example, an enterprise app targeting a specific set of users.

- Team: small team of 7/8 people, average skill set is either novice or intermediate

- Budget: small budget for development and infrastructure

In most practical use cases, a monolithic architecture would suffice. 

## When to choose a microservice architecture

the decision here is more complex unlike choosing a monolithic architecture:

- Market: time to market is NOT critical

- Application context: large scope, well defined features, 100% will grow

  For example, an e-commerce store, a social media service, a video streaming service

- Users: large users base

  For example, public users.

- Team: large team, average skill set is good and they are confident about microservice

- Budget: ready to spend money for development and infrastructure

Though a monolithic architecture would suffice in most cases, investing up front in a microservice architecture will reap long-term benefits when the application grows huge.