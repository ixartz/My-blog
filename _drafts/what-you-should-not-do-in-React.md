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

What is wrong with the previous code? Well, the function is re-created at each rendering of parent component. As you can guess, it will hurt application performance by two ways. First, it will create unnecessary anonymous function at each rendering of parent component.

Then, because it creates a new anonymous function, React will also trigger a re-render of child component.

#### Solution

It is very easy to solve, you should not declare your arrow function inside of render. You should move the arrow function as class field. Then, child component props should refer to this class field.

### Nested state

I made a mistake to try working with nested state in React. A nested state is putting object into react State. For example this following code is a nested state:

#### Solution

Here is the solutions you can follow based on your context:

1. Just change your design and avoid using nested state
2. Use destructuring, it will un-nested your object into the state
3. You can also create a new object yourself when you make a change. But, what I suggest is to use immutable library. Facebook provides Immutable.js, it will do the job.

### Copy props to state

This React anti-pattern needs a post itself to cover fully this subject. I will not go deeper but you can find all the information at this location.

### target="_blank" security

Thanks to ESLint, I discover there is a security issue with this simple code:

I could not imagine with this one line of code, it can bring a vulnerability to your application.

Indeed, in the targeted link with another simple code: . It can redirect to malicious website.

#### Solution

In your link, you just need to add

and you should get the following code:

Now, you are safe to redirection with opener.

## Conclusion

I have just shared 5 mistakes I have done when I was working in React. I am continuing to learn but I hope you can avoid doing the same mistake as me. If you have also some other anti-pattern, do not hesitate to leave below a comment.
