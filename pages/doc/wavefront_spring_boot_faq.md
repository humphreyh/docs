---
title: Wavefront for Spring Boot FAQs
keywords:
tags: 
sidebar: doc_sidebar
permalink: wavefront_spring_boot_faq.html
summary: Get answers to your questions about Wavefront for Spring Boot
---

### What is the difference between the freemium cluster and a Wavefront trial?
* **Freemium cluster** <br/>The freemium cluster supports limited data ingestion throughput with 5-day retention and no SLA guarantees. It allows developers to try out Wavefront without having to sign up or provide an email address.

* **Wavefront trial** <br/>The Wavefront trial allows you to experience the full power of the Wavefront platform by bringing in data from your cloud environments (AWS/GCP/Azure/vSphere), Kubernetes, over 200 integrations, and large-scale service fleets into a single observability platform. You can also create smart alerts that dynamically filter noise and capture true anomalies. We ask that you tell us more about yourself when signing up for a trial.

  Once you've signed up, you can retrieve an API token and configure it in your `application.properties` file: 
  ```
  management.metrics.export.wavefront.api-token=44444-34this-45is-123a-sampletoken
  ```

### What is the retention and Service Level Agreement (SLA) on the freemium cluster?
While this is subject to changes at any time, we currently retain 5 days of data and offer no SLA on the free Wavefront cluster. Production Wavefront clusters currently offer 18 months of full-resolution (no downsampling) data retention for metrics, 6 months for histograms, and 30 days for spans. We also have a 99.95% uptime guarantee, as well as High Availability (HA) and Disaster Recovery (DR) options.

Reach out to us on [Slack](https://www.wavefront.com/slack-us) and join the #springboot public channel for more information.

### Why do I not see a link to access the Wavefront service on start-up?

* Currently, only our freemium cluster supports automatic account provisioning with Spring Boot. If you are using a different cluster, you won’t see a link.
* You have configured an API token in your application's `application.properties` file. If you want to see the link printed on the console, add `wavefront.freemium-account=true` to the `application.properties` file.
* If you have a web application, expose the Wavefront actuator endpoint to easily access your dashboard.

### How do I ensure I send data to the same account all the time (across multiple machines and deployments)?
* If you are just trying out Wavefront, see [Manage Service Accounts](accounts.html#service-accounts) to create a service account that has a static token for reporting. Once you have the token, add it to the `application.properties` file.
* If you want to use Wavefront in a larger deployment, sign-up for a Wavefront trial at [www.wavefront.com](www.wavefront.com) and see [Manage Service Accounts](accounts.html#service-accounts) to learn how to create a service account. Next, add the token and URL to the `application.properties` file. We can help you with sizing and designing large-scale collection architectures for metrics, histograms, and traces. Reach out to us on [Slack](https://www.wavefront.com/slack-us) and join the #springboot public channel for more information.

### How do I set up an email/password login to the account?

You can [Invite users and let them send data to the same cluster](wavefront_springboot.html#optional-custom-configurations). To invite yourself, just enter your email address. 
 
If you added `wavefront.freemium-account=true` to your `application.properties` file, make sure to remove it so that a single-use login URL is no longer requested on startup.

### What do I do if I sign out of the freemium cluster?

* If you have invited yourself and created an account, log in using your username and password.
* Save the link that you used to access the Wavefront Service dashboard and restart your Spring Boot application. Next, paste the link you saved into a browser to access the dashboard.
* If you deleted the `~/.wavefront_freemium` file that was saved in the home directory, a new account is created and you will not be able to access the old link you saved to view your existing data.

### How do I get help?
Reach out to us on [Slack](https://www.wavefront.com/slack-us) and join the #springboot public channel.