---
title: " "
layout: splash
lang: "en"
permalink: en/about/
author_profile: true
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/about/about-heading.jpg
excerpt: "An IT professional in a continuous journey of learning, studying, sharing, and contributing ."
intro: 
  - excerpt: >-
      I graduated in Computer Science from the UCB (Universade Católica de Brasília), and I was post graduated in Service Oriented Architecture 
      by UNIEURO. I work with IT since 2003, being working most of the time for the Brazilian Government and Brazilian banks. My career objective is to 
      be involved in solutions development which make difference in people's life. In addition to working with innovative things. More details about 
      my career can be found on my LinkedIn.
feature_row:
  - title: "Computer Science in UCB"
    excerpt: >-
      I joined the university having 16 years old. When I got accepted into the university course, it had class topics related to software engineering, 
      computer networks, and artificial intelligence. Besides having class on those topics, I participated in some University research activities and 
      projectes, which helped me in my course conclusion project in the topic of intelligent tutoring systems. This project resulted in a publishing  
      at the World Congress on Medical Physics and Biomedical Engineering in 2006. 
    url: "https://ucb.catolica.edu.br/portal/"
    btn_label: "Read Mores"
    btn_class: "btn--primary"
  - title: "MBA in Service Oriented Architecture"
    url: "https://unieuro.grupoceuma.com.br/"
    btn_label: "Read Mores"
    btn_class: "btn--primary"
    excerpt: >-
      This post graduation course was done in 2009 when I was already working with solutions architecture topics for the Brazilian Government projects. At that 
      it was a modern approach for handling solution scenarios and it brought a background that I can use even nowadays. The conclusion project for this course 
      was based on a real case scenario about the usage of service oriented approach to map the business process for evolving the architecture from the Bank BRDS.
feature_row1:
  - title: "Government Projects Experience"
    excerpt: >-
      My career started at SERPRO (Serviço Federal de Processamento de Dados) in projects related to the Government federal revenue agency. After that I worked at CONFEA  
      (Federal Council for Engineering and Architecture) in the engineers and architects national register. I had the opportunity to work in projects with social importance
      at FNDE (Fundo Nacional da Educação), MDS (Social Development Ministry) and TSE (Electoral Superior Tribunal). In this journey I can tell that it was possible to bring
      several contributions to the Brazilian society with as much dedication as possible.
  - title: "Bank Projects Experience"
    excerpt: >-
      My first experience with banking projects was in 2007 when I worked in the payments module from Banco do Brasil Internet Banking. In this project I could work 
      at the layout unification for payment documents in different browsers. I worked later with projects for service channels at Banco da Amazônia e Caixa Econômica Federal. 
      After this period I worked in credit cooperative project at SICOOB. It was a pleasure to work with solutions for a so demanding segment.
  - title: "Europe Projects Experience"
    excerpt: >-
      I moved to Europe with the main goal to aggregate into my professional experience the reality of international projects, besides the fact of improving also the experience 
      in projects related to other business contexts. This journey allowed me to get in touch with professionals from different countries. I could work in projects related to 
      corporate baking systems, supply chain, telecom, travel and tourism.
---

{% assign author = page.author | default: page.authors[0] | default: site.author %}
{% assign author = site.data.authors[author] | default: author %}

{% if author.avatar %}
  <div class="author__avatar" align="center">
    <a href="{{ author.home | default: '/' | absolute_url }}">
      <img src="{{ author.avatar | relative_url }}" alt="{{ author.name }}" itemprop="image" class="u-photo">
    </a>
  </div>
{% endif %}

<div class="author__urls-wrapper" align="center">
  <ul class="author__urls social-icons">
    <li>
      <a href="https://github.com/{{ author.github }}" itemprop="sameAs" rel="nofollow noopener noreferrer me">
        <i class="fab fa-fw fa-github" aria-hidden="true"></i><span class="label">GitHub</span>
      </a>
    </li>
    <li>
      <a href="https://www.linkedin.com/in/{{ author.linkedin }}" itemprop="sameAs" rel="nofollow noopener noreferrer me">
        <i class="fab fa-fw fa-linkedin" aria-hidden="true"></i><span class="label">LinkedIn</span>
      </a>
    </li>
  </ul>
</div>

{% include feature_row id="intro" type="center" %}

<h1 id="page-title" class="page__title" style="text-align: center;">University Experience</h1>

{% include feature_row type="center" %}

<h1 id="page-title" class="page__title" style="text-align: center;">Career</h1>

{% include feature_row id="feature_row1" type="center" %}