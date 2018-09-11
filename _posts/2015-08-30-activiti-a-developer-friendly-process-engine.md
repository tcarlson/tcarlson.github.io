---
title: 'Activiti: a developer-friendly process engine'
layout: post
image: wp-content/uploads/2015/08/activiti-825x510.png
categories:
  - Activiti
  - BPM
tags:
  - activiti
  - automation
  - bpm
  - workflow
---
<a href="http://www.slideshare.net/TravisCarlson/activiti-a-developerfriendly-process-engine" target="_blank">Slides</a> and <a href="https://github.com/tcarlson/demos" target="_blank">code</a> from my presentation and demo of Activiti at the [Tampa Java Users Group](http://www.meetup.com/Tampa-JUG/events/222813410/).

[<img src="wp-content/uploads/2015/08/highres_441359061.jpeg" alt="highres_441359061" width="3264" height="1836" class="alignnone size-full wp-image-106" srcset="wp-content/uploads/2015/08/highres_441359061.jpeg 3264w, wp-content/uploads/2015/08/highres_441359061-300x169.jpeg 300w, wp-content/uploads/2015/08/highres_441359061-1024x576.jpeg 1024w" sizes="(max-width: 3264px) 100vw, 3264px" />](wp-content/uploads/2015/08/highres_441359061.jpeg)

Evolved from jBPM, Activiti is an open-source workflow and process engine for Java. Unlike big-vendor BPM platforms that are traditionally bloated and targeted at business analysts, Activiti is developer-oriented, light-weight, easy to learn and manage. 

Modeling your processes in Activiti makes them self-documenting, organized, and manageable. Process state is persisted to a database which creates a convenient audit log to know what happened when and can even be the basis for a Business Intelligence (BI) tool. 

Activiti processes are modeled using an Eclipse plugin and managed using a simple web application. The runtime engine can either be deployed as a WAR or embedded into your Java application. An optional REST interface allows for language-agnostic and remote interaction with the process management API. 

This presentation will be an overview of Activiti and the concepts involved, followed by a demo of how to model, deploy, execute, and monitor a process.