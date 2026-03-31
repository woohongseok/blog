---
layout: page
title: About
permalink: /about/
---

<img class="about-avatar" src="{{ '/assets/img/frieren-books-stack.jpg' | relative_url }}" alt="profile">

Hi, I'm **Woo Hong Seok**, a full-stack developer working with the Node.js ecosystem.

{% assign now_year = 'now' | date: '%Y' | plus: 0 %}{% assign now_month = 'now' | date: '%m' | plus: 0 %}{% assign year_diff = now_year | minus: 2024 %}{% assign month_diff = now_month | minus: 6 %}{% assign total_months = year_diff | times: 12 | plus: month_diff %}{% assign years = total_months | divided_by: 12 %}{{ years }}+ years of building web applications with TypeScript and Java — React, Next.js on the frontend, NestJS, Express, and Spring on the backend. Currently diving into Spring Boot.

Daily setup: Warp + Neovim (NvChad).

<h3>Skills</h3>

<div class="skill-grid">
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" alt="TypeScript">
    <span>TypeScript</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg" alt="Java">
    <span>Java</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" alt="React">
    <span>React</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nextjs/nextjs-original.svg" alt="Next.js">
    <span>Next.js</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nestjs/nestjs-original.svg" alt="NestJS">
    <span>NestJS</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/express/express-original.svg" alt="Express">
    <span>Express</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original.svg" alt="Spring">
    <span>Spring</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/prisma/prisma-original.svg" alt="Prisma">
    <span>Prisma</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/npm/simple-icons/icons/typeorm.svg" alt="TypeORM">
    <span>TypeORM</span>
  </div>
  <div class="skill-card">
    <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/hibernate/hibernate-original.svg" alt="JPA">
    <span>JPA</span>
  </div>
</div>
