---
title: Project Tech Stack
description: This document outlines the technology stack will be used in the project.
author: jay
date: 2024-09-06 12:00:00 +0800
categories: [Blogging]
tags: [templj]
pin: false
math: true
mermaid: true
image:
  path: /commons/devices-mockup.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Responsive rendering of Chirpy theme on multiple devices.
---

# Project Tech Stack

This document outlines the technology stack will be used in the project.

## Infrastructure
- **Cloud Provider**: AWS
- **Container Orchestration**: EKS (Elastic Kubernetes Service)
- **Container Registry**: ECR (Elastic Container Registry)
- **Load Balancing**: ALB (Application Load Balancer)
- **VPC Configuration**: Public Subnet, Private Subnet
- **Database**: RDS (Relational Database Service)

## Backend
- **Framework**: Spring Boot (Java)
- **API Communication**: REST API
- **Database**: Aurora MySQL (RDS)
- **ORM**: Hibernate (JPA)

## DevOps
- **CI/CD**: GitHub Actions
- **Monitoring**: AWS CloudWatch
- **Log Management**: AWS CloudWatch Logs
- **Infrastructure as Code**: Terraform
- **Container Management**: Docker

## Other Tools
- **Version Control**: Git (GitHub)
- **Notification**: Slack
