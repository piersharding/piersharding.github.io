---
layout: post
title:  "Moodle bulk management of users, courses, and course categories"
date:   2012-04-14 12:00:00
categories: general
---


<p>
One of the (many) new features of <a href="http://moodle.org/">Moodle 2.2</a> is the ability to create administration Tools <a href="http://docs.moodle.org/dev/Admin_tools">plugins</a>.  This enables us developers to create and package (hopefully) useful tools that make the management of Moodle easier.  One of the things that I've seen wished for is the ability to bulk upload courses and related material, and over recent months, this is something that I've been working on.
</p>
<p>The key things that people want (from an administration point of view) are to manage people and courses.  Often these activities are a tiresome bulk process at set times of the year with a relatively minor tweaking type of activity in between.  For managing users - create/update/delete, and enrolments - we already have the built in functionality to do <a href="http://docs.moodle.org/22/en/Upload_users">bulk user upload</a>.  I have added to this for <a href="https://gitorious.org/moodle-tool_uploadcourse">Courses</a> and for <a href="https://gitorious.org/moodle-tool_uploadcoursecategory">Course categories</a>.<br/>
The course upload admin tool can be used to create and manage course outlines, but it can also populate courses using either a nominated course as a template (copies the course contents using the Moodle backup/restore facility), or populate the course from a Moodle backup file.
</p>

<div id="a000093more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at April 14, 2012  8:41 PM</p>





