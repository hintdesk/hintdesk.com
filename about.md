---
layout: page
title: "About"
---

<!-- markdownlint-disable MD033 -->
{% if site.homepage.intro-text.size > 0 or site.homepage.intro-image.size > 0 %}
<div class="hero-section" style="padding-top: 0; margin-bottom: 2rem;">
  {% if site.homepage.intro-text.size > 0 %}
  <p class="hero-bio">{{ site.homepage.intro-text }}</p>
  {% endif %}

  {% if site.homepage.intro-image.size > 0 %}
  {%
    include image.html
    file=site.homepage.intro-image
    alt=site.title
  %}
  {% endif %}
</div>
{% endif %}
<!-- markdownlint-enable MD033 -->

## Overview

This website serves as my personal technical notebook and engineering knowledge base.

For archived historical posts from earlier versions of this blog, you can visit the [Hintdesk Backup](https://hintdeskbackup1.wordpress.com/).

## Projects & Tools

<!-- markdownlint-disable MD033 -->
<div class="showcase-grid">
  <div class="showcase-card">
    <h4><a href="https://github.com/hintdesk/elasmon" target="_blank" rel="noopener noreferrer">Elasmon</a></h4>
    <p>ElasMon is a standalone desktop application for real-time Elasticsearch monitoring.</p>
  </div>
  <div class="showcase-card">
    <h4><a href="https://hintdesk.github.io/networkhartocsv/input" target="_blank" rel="noopener noreferrer">Network HAR to CSV</a></h4>
    <p>A web utility to convert HTTP Archive (HAR) network logs into structured CSV spreadsheets for performance analysis.</p>
  </div>
  <div class="showcase-card">
    <h4><a href="https://clipboard.hintdesk.com/" target="_blank" rel="noopener noreferrer">Internet Clipboard</a></h4>
    <p>A lightweight online tool to quickly sync text snippets and notes across multiple devices.</p>
  </div>
  <div class="showcase-card">
    <h4><a href="https://play.google.com/store/apps/dev?id=6936148626631500832" target="_blank" rel="noopener noreferrer">Android Apps</a></h4>
    <p>A collection of mobile applications published on the Google Play Store.</p>
  </div>
</div>
<!-- markdownlint-enable MD033 -->
