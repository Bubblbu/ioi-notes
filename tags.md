---
layout: Post
title: By Tags
permalink: /tags/
content-type: eg
---

<style>
.category-content {
    padding-bottom: 0.4em;
    list-style: none;
}
</style>

<main>
    {% assign tags =  site.notes | map: 'tags' | join: ' '  | split: ' ' | uniq %}
    {% for tag in tags %}
        <a id="{{ tag }}" href="/tags/#{{ tag }}">
            <h3>{{ tag | capitalize }}</h3>
        </a>
        {% for note in site.notes %}
            {%- if note.tags contains tag -%}
                <li class="category-content">
                    <a href="{{note.url}}">{{ note.title }}</a>
                </li>
            {%- endif -%}
        {% endfor %}
    {% endfor %}
</main>
