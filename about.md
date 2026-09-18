---
layout: page
title: "About Hintdesk"
---

<!-- markdownlint-disable MD033 MD010 -->
{% if site.homepage.intro-text.size > 0 or site.homepage.intro-image.size > 0 %}
<section class="text-center mb-70">
	{% if site.homepage.intro-text.size > 0 %}
	<p>{{ site.homepage.intro-text }}</p>
	{% endif %}

	{% if site.homepage.intro-image.size > 0 %}
	{%
		include image.html
		file=site.homepage.intro-image
		alt=site.title
	%}
	{% endif %}
</section>
{% endif %}
<!-- markdownlint-enable MD033 MD010 -->

## About

This website is here for me to save some notes. Please don't expect to find some cool things here.
If you need to contact me, please start a new [discussion](https://github.com/hintdesk/hintdesk.com/discussions).

If you would like to access the old post, you can read it here [Hintdesk Backup](https://hintdeskbackup1.wordpress.com/)

## Some apps made by me

[Network HAR to CSV](https://hintdesk.github.io/networkhartocsv/input)

[Internet Clipboard](https://clipboard.hintdesk.com/)

[Some Android Apps](https://play.google.com/store/apps/dev?id=6936148626631500832)
