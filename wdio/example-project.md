---
title: Webdriver.io 
description: An attempt to recreate an end to end testing suite used by development teams at work. 
published: true
date: 2023-07-12T16:38:26.657Z
tags: e2e, webdriver.io, devops
editor: markdown
dateCreated: 2023-07-12T16:25:44.874Z
---

# WDIO

- [Docs](https://webdriver.io/)

A development team within my company uses webdriver.io to perform automating e2e testing of an application deployed within various environments. The testing suites are isolated from the application code in its own repository. This allows testers to plug in env variables associated with the environment they wish to test against. Webdriver.io is configured to produce a report using the [Allure framework](https://docs.qameta.io/allure/). These reports are then published to a UI where stakeholders of the project can view the results. 

The goal of this project is to create a "watered-down" replication of what this development team is doing in their test suites. 


