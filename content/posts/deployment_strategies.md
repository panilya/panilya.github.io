+++ 
author = "Illia Pantsyr" 
title = "Deployment Strategies" 
date = "2024-05-09" 
tags = [ "devops" ] 
+++

Deployment strategies provide a systematic approach to releasing software changes, minimizing risks, and maintaining consistency across projects and teams.
Without a well-defined strategy and systematic approach, deployments can lead to downtime, data loss, or system failures, resulting in frustrated users and revenue loss.
Before we start exploring different deployment strategies in more detail, let’s take a look at the short overview of each deployment strategy mentioned in this article:

- All-at-once deployment: This strategy involves updating all the target environments at once, making it the fastest but riskiest approach.
- In-place deployment: This involves stopping the current application and replacing it with a new version, directly affecting availability.
- Blue/Green deployment: A zero-downtime approach that involves running two identical environments and switching from old to new.
- Canary deployment: Introduces new changes incrementally to a small subset of users before a full rollout.
- Shadow deployment: Mirrors real traffic to a shadow environment where the new deployment is tested without affecting the live environment.

## All-at-once deployment

All-at-once deployment strategy, also known as the "Big Bang" deployment strategy, involves simultaneously releasing your application's new version to all servers or environments. This method is straightforward and can be implemented quickly, as it does not require complex orchestration or additional infrastructure. The primary benefit of this approach is its simplicity and the ability to immediately transition all users to the new version of the application.

However, the all-at-once method carries significant risks. Since all instances are updated together, any issues with the new release immediately impact all users. There is no opportunity to mitigate risks by gradually rolling out the change or testing it with a subset of the user base first. Additionally, if something goes wrong, the rollback process can be just as disruptive as the initial deployment.

Despite these risks, all-at-once deployment can be suitable for small applications or environments where downtime is more acceptable and the impact of potential issues is minimal and is used pretty often. It is also useful in scenarios where applications are inherently simple or have been thoroughly tested to ensure compatibility and stability before release.

## In-place (Recreate) deployment

In-place or recreate deployment strategy is another strategy that is used pretty often when developing projects. It is the simplest and does not require additional infrastructure.
Its essence lies in the fact that when we deploy a new version, we stop the application and start it with new changes. The disadvantage of this approach is that the service we are updating will experience downtime that will affect its users.
Also, in case of problems with new software changes, we might need to rollback the latest changes which will lead to service downtime.
To avoid downtime and be able to rollback changes without it during the deployment process, there are deployment strategies that are created for this purpose and used in the industry.

## Blue/Green deployment

The first zero downtime deployment strategy we’re going to talk about is the Blue/Green deployment strategy. Its main goal is to minimize downtime and risks while deploying new software versions. This is done by having 2 identical environments of our service. One environment contains the original application (Blue environment) that serves users' requests and the other environment (Green environment) is where new software changes are deployed. This allows us to verify and test new changes with near zero downtime for users and the service, with the ability to safely rollback in case of any problems, except for some cases that we will discuss a bit later.
Typically, the process is the following: after verifying and testing the new changes in the Green environment, we reroute traffic from the Blue environment to our identical Green environment with the new changes.
Sounds easy, isn’t it?

![umm...actually...emoji](/deployment_strategies/umm_actually_emoji.png)

it depends.
The problem is that we can easily reroute traffic between environments only when our services are stateless. If they interact with any data sources, things get more complicated, and here's why:
Our identical Green and Blue environments share common data source(s), while sharing data sources such as NoSQL databases or object stores (AWS S3, for example) between our identical environment is easier to accomplish, this is completely not true for relational databases because it requires additional efforts (NoSQL also might require some effort) to support Blue/Green deployments. Since approaches to handle schema updates without downtime are out of the scope of this article you can check out the article by Yuriy Ivon: [Upgrading database schema without downtime](https://blog.devgenius.io/upgrading-database-schema-without-downtime-9961070b9016) to learn more about updating schemas without downtime.
A general recommendation is that If your services are not stateless and use data sources with schemas, implementing a Blue/Green deployment strategy is not always recommended because of the additional risk and failure points this can introduce minimizing the benefits of the Blue/Green deployment strategy. But if you’ve decided that you need to integrate a Blue/Green deployment strategy and your infrastructure is running on Amazon Web Services, you might find [this document by AWS on how to implement Blue/Green deployments](https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf) and its infrastructure useful.

## Canary deployment

The idea of the Canary deployment strategy is to reduce the risks of deploying new software versions in production by rolling out new changes to users slowly.
In the same manner, as we do in the Blue/Green deployment strategy we roll out new software versions to our identical environment, but instead of completely rerouting traffic from one environment to another - we, for example, route a portion of users to our environment with new software version using a load balancer. The size of the portion of users getting new software versions and/or the criteria used to determine them - may be specific for every company/project. Some roll out new changes only to their internal stuff first, some determine users randomly and some may use algorithms to match users based on some criteria. Pick anything that best suits your needs.

## Shadow deployment

Shadow deployment strategy is the next strategy I find interesting, personally. This strategy also uses the concept of identical environments, just as the Blue/Green and Canary deployment strategies do. The main difference is that instead of completely rerouting or rerouting only a portion of real users we duplicate entire traffic to our second environment where we deployed our new changes. This way, we can test and verify our changes without negatively affecting our users, thus mitigating risks of broken software updates or performance bottlenecks.

## Conclusion

In this article, we walked through 5 different deployment strategies each with its own set of advantages and challenges. The All-at-once and In-Place deployment strategies stand out for their speed and minimal effort required to deploy new versions of software. While these 2 strategies will be your go-to deployment strategies in most cases, it’s still important to understand and know about more complex and resource-intensive strategies.
Ultimately, implementing any deployment strategy requires careful consideration of the potential impact on both the system and its users. The choice of deployment strategy should align with your project’s needs, risk tolerance, and operational capabilities.
