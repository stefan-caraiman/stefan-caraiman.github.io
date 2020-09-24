---
title: "Lessons learned from production outages - Part 1 Embracing the outage 📉"
date: 2020-01-25T10:23:30-04:00
categories:
  - devops
  - outage
tags:
  - devops
---

You never know when it will happen to you, but you can be sure that one day it will, it is inevitable.

No I'm not refering to a deep existential topic, I'm talking about production outages, or a blessing in disguise, as you will shortly see after going through this blogpost, and why outages should be embraced as something which is normal.

Because I feel like this topic can become quite expandable, I will be separating it in multiple parts, in order to make it easier to understand and also learn(or amuse yourself) gradually, from my experiences with outages.

## Context

Although the "DevOps" mindset is all about planning ahead and reducing these kind of impactful events, it is inevitable, it happened before, it happens, and it will still happen. For example, here are a few in-depth outage reports from bigger companies about how the outages happened and how they eventually got fixed:

* [Gitlab][gitlab] 
* [Grafana][grafana]
* [Cloudflare][cloudflare]
* [Facebook][facebook]
* [Discord][discord]

It is afterall in the nature of the DevOps mindset that there will always be a continous growth and subsequent changes to the systems and tools that we are using, it is the normal course of a projects life cycle.

The get go is that, no matter the scale of the project and that of the team, time and time again there will always be an edge case or point of failure hiding around, or something well known which simply being treated as "non-critical" (big mistake).


## How to tackle outages

Just as the only real recipe for true success is failure, the same thing applies to outages. You can't grow unless you embrace the fact that you will fail, the difference between a failure and a future success being in the way you take it in, learn from it, and continue on growing, you can't expect to continously fail and grow, you need to have a plan for it.

Having said that, here are a few points which I identified as being critical in order to be prepared for an outage.

## Automate all the manual proccesses

Humans make mistakes, especially in stressful situations, such as an outage. And at the same time the whole "DevOps" mindset is about automation, there is no excuse for a manual procedure when you decide to go down the "DevOps" path.

## Backup your sensitve data

Although this sounds like the most obvious thing for any business which works with sensitive data, time and time again this proves to be simply forgotten or never properly checked or implemented. There will always be that case of someone running a query on a production database and suddenly deleting everything, and all hell breaking loose because there are no actual backups of the database.

## Communication is key

As soon as the outage is acknowledged, all the main parties should be informed and involved in order to make the process as transparent as possible for everyone. And by that I mean, the IT/DevOps/Infra teams, higher management and most importantly, the client or any stakeholder. Either if the client is an end user, or another party to which you offer DevOps services, these parties are most of the times the ones being forgotten and the most important ones in reality, since they are the "bread and butter", of the project.

Make sure to also remember to give updates at any checkpoint, progress or after a certain amount of time has passed, since everyone will be wondering what is happening and if anyone is actually still working on it.

## Monitoring, logging, alerting to the rescue

Having a general overview of everything that is happening in your infrastructure, is invaluable for debugging. Many outages can be derailed from causing any kind of a business impact, by using automated alerts, rather than learning about it from the end users. Having a centralized logging solution will also drastically reduce the time required to come to a resolution.

## Conclusion

Most of the steps described above require time and effort in order to plan, implement and maintain. But at the end of the day we should be asking ourselves if the risk associacted with it is worth the major impact an outage can have on the business.

In the next parts of this blog series, I will be going through past personal experiences with outages and how it all played out, and how to conduct a post mortem outage report.

Feel free to drop any questions or comments down below, happy reading!


[gitlab]: https://about.gitlab.com/blog/2017/02/10/postmortem-of-database-outage-of-january-31/
[grafana]: https://grafana.com/blog/2020/01/23/how-a-gcp-persistent-disk-incident-snowballed-into-a-23-hour-outage-and-taught-us-some-important-lessons/
[cloudflare]: https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019/
[facebook]: https://blog.thousandeyes.com/facebook-outage-deep-dive/
[discord]: https://status.discordapp.com/incidents/dj3l6lw926kl