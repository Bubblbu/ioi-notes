---
layout: Post
title: By Date
permalink: /dates/
content-type: eg
---

<style>
.date-content {
    padding-bottom: 0.4em;
    list-style: none;
}
</style>

<main>
    {% assign postsByDay = site.notes | group_by_exp:"note", "note.created | date: '%d-%B-%Y'" %}

    {% for day in postsByDay %}
        <h3 id="{{ day.name }}">{{ day.name }}</h3>
        {% for note in day.items %}
            <li class="date-content">
                <a href="{{ note.url }}">{{ note.title }}</a>
            </li>
        {% endfor %}
    {% endfor %}
</main>
