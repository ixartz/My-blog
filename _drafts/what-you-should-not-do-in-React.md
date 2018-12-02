---
title: '5 bad practices that you should not do in React'
date: 2018-12-02
description:
categories:
  - React
tags:
  - React
  - Tips
---
Lately, I have used intensively React in my work but also in my personal project. Here, I will share what mistakes I have done in my React code. And, what you should also avoid doing in your project.

### Arrow function in render

First thing you should avoid is to inline arrow function in render. Here is an example:

What is wrong with the previous code? Well, the function is re-created at each rendering of parent component. As you can guess, it will hurt application performance by two ways. First one, it will create unnecessary anonymous function.

The second one, because at each rendering of parent component it creates a new function, React will also trigger a re-render of child component.

#### Solution

### Object in state

### Copy props to state

### target="_blank" security

style component??
webpack??
