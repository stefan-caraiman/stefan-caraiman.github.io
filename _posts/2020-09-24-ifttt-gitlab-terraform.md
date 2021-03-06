---
title: "Smart devices + Gitlab + Terraform = 💘"
date: 2020-09-24T17:24:31-04:00
tags:
  - docker
  - linux
  - terraform
  - endava
---

This blog post serves as an insight into the implementation details that went into my recent demo presentation at **Creative Minds** hosted by Endava about IFTTT, Google Assistant, Gitlab and Terraform and to also offer additional side details and views on these topics.

A link to the Gitlab project can be found **[here][gitlab]** 🍀


## Why 🤔?


The first thing that comes to mind would be, **why not?** we always hear about the future and the shiniest tools and how smart devices can help us in many different ways, so why not try to implement something like that in the **DevOps life cycle**.

The other immediate answer would be, becase ever since I saw [Kelsey][Kelsey] Hightower do his now *classical* demos with the **Google Assisstant**, I've wondered just **how hard can it be** to integrate and implement something like that?

Well after some initial *google searches* and a couple of hours of clicking around the web, I had already managed to create a somehow functional **voice assisted CI/CD pipeline**.


## How 📝?

After a few minutes of scribbling around I came to the following **ideal workflow** 🔎

  1. Tell the **Google Assisstant** to run a **terraform** command
  2. It will pass the given input to the **Gitlab pipeline** as a *parameter*
  3. The **Gitlab pipeline** will run the **terraform** command inside a **Docker** container
  4. Following that we trigger a **message** on a **Slack channel**
  5. Finally we **light up a bulb** inside the **development room** (my livingroom in this case since it's the Working from Home year 🏡)


But what kind of service could serve as the **glue** for triggering all of these diverse actions?

In my case I've decided to go with **IFTTT** (*If This Then That*), a web service which can **connect** and **create chains of actions** of various other **services**.

Services such as(*but not limited to*): Github, Webhooks, Google, Alexa, HueLights, Office365 suite, Trello, Slack and a lot of smart devices.

I started off by connecting the following services to my **IFTTT** account:
  
  * Google Assisstant 🎙
  * TP-Link Kasa(for the lightbulbs) 💡
  * Slack 📓


After which I created and setup the **[gitlab][gitlab]** project 🛠

  * setup **Gitlab** repository
  * setup CI/CD
    * have a look at `.gitlab-ci.yml` and `scripts/` folder
  * setup Docker image to run in the CI/CD stages
    * have a look at `Dockerfile`
  * setup a **webhook** that will be triggered by **IFTTT** through **Google Assisstant**
    * Settings -> CI/CD -> Pipeline triggers
  * write **Terraform** code to manage **AWS resources** 👨‍💻
    * have a look at `*.tf` files


From then on out, it was a matter of clicking and writting the necessary parameters and details in [IFTTT][IFTTT]

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/ifttt.png)


For example I've configured the **Google assisstant service** to respond like so (feel free to customize it as sassy as you like 💁‍♀️)

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/google-ifttt.png)


As you can see, the **IFTTT** platform is pretty straight forward and easy to use, making **smart device automation and integration** less intimidating than most of us might think of it.


## Conclusion 🙌

Although smart device integrations is seen as something still in the maturity phase, the integrations and services available at the moment all work almost flawlessly and cover up a large portion of what any business looks to adopt in their life cycles.

My view is that we should try and drive this new **revolution** as it proves that it can bring a lot of initial value to any kind of project, especially in making the working environment feel more **interactive** (*and a lot more fun as well*)


### Special thanks to 🙏: 

  * Dana Panica 💃
  * Gabriel Ichim 🤵

For all the help and support for and during this presentation!


[Kelsey]: https://twitter.com/kelseyhightower
[gitlab]: https://gitlab.com/devops146/terraform-smart-demo
[IFTTT]: https://ifttt.com/create/
