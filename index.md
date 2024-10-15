---
layout: base
title: Student Home 
description: Home Page
hide: true
---

<ul>
  <li><a href="https://zachpeltz.github.io/zach_2025/blogs/">Blogs</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/about/">About</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/devops/hacks">Hacks</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/snake/">Snake</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/games/">Games</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/planningdocument/">Planning Doc</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/cookieclicker/">Cookie Clicker</a></li>
  <li class="dropdown">
    <a href="#">Dropdown</a>
    <ul class="dropdown-content">
      <li><a href="3_1_link">3.1</a></li>
      <li><a href="3_2_link">3.2</a></li>
      <li><a href="3_3_link">3.3</a></li>
      <li><a href="3_4_link">3.4</a></li>
      <li><a href="3_5_link">3.5</a></li>
      <li><a href="3_6_link">3.6</a></li>
      <li><a href="3_7_link">3.7</a></li>
      <li><a href="3_8_link">3.8</a></li>
      <li><a href="3_10_link">3.10</a></li>
    </ul>
  </li>
</ul>

<img src="https://media.tenor.com/xKJ0blGgIlQAAAAM/dance-happy.gif" alt="mario gif">
This is my website: some links above

<style>
/* General styling */
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}
li {
  float: left;
}
li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
li a:hover {
  background-color: #111;
}

/* Dropdown styling */
.dropdown {
  position: relative;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #333;
  min-width: 160px;
  z-index: 1;
}

.dropdown-content li {
  float: none;
}

.dropdown-content li a {
  padding: 12px 16px;
  text-align: left;
}

.dropdown-content li a:hover {
  background-color: #555;
}

.dropdown:hover .dropdown-content {
  display: block;
}

.dropdown:hover a {
  background-color: #111;
}
</style>