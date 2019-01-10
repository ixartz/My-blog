---
title: '4 practices you should avoid in React'
date: 2019-01-05
description: Mistakes I have done in React
categories:
  - React
tags:
  - React
  - Tips
---
Lately, I've used intensively React in my work but also in my personal project. Here, I'll share the mistakes I've done in my React code. And, what you should also avoid doing in your project.

You can access one of my personal project using React [at this location](https://github.com/ixartz/handwritten-digit-recognition-tensorflowjs). The 4 mistakes I list here was done in this project where I implement a digit recognizer. This project helps me learn Redux, Tensorflow.js, styled-components, Ant Design, etc. I'm very happy to share what I learn in this small deep learning project with React.

### Arrow function in the render function

The first thing you should avoid is to inline an arrow function in React's render function. The ESLint rule is *react/jsx-no-bind*. Here is an example:

{% highlight jsx %}
class Button extends React.Component {
  render() {
    return (
      <button onClick={() => { console.log("Hello world!"); }}>
        Click me!
      </button>
    );
  }
}
{% endhighlight %}

What is wrong with the previous code? Well, the function is re-created at each rendering of the parent component. As you can guess, it'll hurt the application performance in two ways. First, it'll create an unnecessary anonymous function at each rendering of the parent component.

Then, it creates a new anonymous function, React will also trigger a re-render of the child component. It'll break *React.PureComponent* or *shouldComponentUpdate* optimization.

#### Solution

It's very easy to solve, you shouldn't declare your arrow function inside of the render. You should move the arrow function as a class field. Then, child component props should refer to this class field. Here is a solution:

{% highlight jsx %}
class Button extends React.Component {
  handleClick = () => {
    console.log("Hello world!");
  };

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me!
      </button>
    );
  }
}
{% endhighlight %}

#### Going deeper

Before changing all your inline function, you should also read these two articles:

* [React, Inline Functions, and Performance](https://cdb.reacttraining.com/react-inline-functions-and-performance-bdff784f5578)
* [Is It Necessary to Apply ESLint jsx-no-bind Rule?](http://shzhangji.com/blog/2018/09/13/is-it-necessary-to-apply-eslint-jsx-no-bind-rule/)

They consider *react/jsx-no-bind* is a premature optimization. I'll let you make your own thought about this topic.

### Nested state

I made a mistake to try working with the nested state in React. A nested state is putting an object into React's state. For example, this following code is a nested state:

{% highlight jsx %}
let coord = {
  x: 0,
  y: 0,
  width: 200,
  height: 200
};

this.state = {
  coord
};
{% endhighlight %}

The issue with nested state happens when you try to update the *coord* object:

{% highlight jsx %}
coord.x = 10;

this.setState({
  coord
});
{% endhighlight %}

You are expecting the component being rendered again. Unfortunately, it is not the case for *PureComponent*. React makes a shallow compare on component state and it won't see there is a change in the state.

Another thing you need to be careful when you use nested state is that stateState performs a shallow merge.

{% highlight jsx %}
constructor() {
  this.state = {
    x: 10,
    y: 10
  };
}

otherfunction() {
  this.setState({
    y: 100
  });
}
{% endhighlight %}

You are expecting **this.state.x = 10** and **this.state.y = 100**. But, when you have a nested state like:

{% highlight jsx %}
constructor() {
  this.state = {
    coord: {
      x: 10,
      y: 10
    }
  };
}

otherfunction() {
  this.setState({
    coord: {
      y: 100
    }
  });
}
{% endhighlight %}

**this.state.coord.x** will become *undefined*.

#### Solution

Here are the solutions you can follow based on your context:

1. Just change your design and avoid using a nested state
2. Use destructuring, it will un-nested your object into the state
3. You can also create a new object yourself when you make a change. But, what I suggest is to use an *immutable* library. Facebook provides *Immutable.js*, it will do the job.

Each solution has his own advantages and disadvantage. You should choose a solution based on your context.

### Show/Hide component with conditional rendering

As you may know, React allows you to render a component based on conditions. I thought I could benefit this conditional rendering to show/hide components. Actually, you should use conditional rendering for toggling small components.

But, for complex ones, you should avoid. Especially, when you have a complex *constructor* or a complex mounting process. Even if it works well but behind the scene, the component was unnecessarily re-created each time we show/hide the element.

{% highlight jsx %}
class Button extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      show: true
    };
  }

  handleClick = () => {
    this.setState({
      show: !this.state.show
    });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>
          Click me!
        </button>
        {/* Here is the conditional rendering */}
        {this.state.show && <ComplexComponent />}
      </div>
    );
  }
}
{% endhighlight %}

The above code will toggle *ComplexComponent* component each time when you click on the button. It works very well to hide/show the *ComplexComponent* component for each click. But, there is a major drawback: each time we display back the *ComplexComponent* component, it'll instantiate a new instance and it will re-create a new one from scratch.

You should avoid using conditional rendering. Especially, when the *ComplexComponent* component has a resource-consuming constructor and/or mounting process. Indeed, the *constructor* and *componentDidMount* method will be called each time we show the component.

#### Solution

The other way in React to show or hide a component is to use CSS. A simple *display* CSS property can be used to show/hide a component without re-creating it.

Below, you can find an example where *display* CSS property can be applied:

{% highlight css %}
.hidden {
  display: none;
}
{% endhighlight %}

{% highlight jsx %}
render() {
  const classname = this.state.show ? null : 'hidden';

  return (
    <div>
      <button onClick={this.handleClick}>
        Click me!
      </button>
      {/* Here is the conditional rendering */}
      <ComplexComponent className={classname} />
    </div>
  );
}
{% endhighlight %}

#### Warning

![Use carefully display none in React]({{ site.baseurl }}assets/images/posts/error.png){: .alignleft} Do not abuse the *display* rule in your React application. With *display: none*, React will still render the element and add to the DOM. Please use the two solutions to toggle a component based on your context.

### target="_blank" security

It's not only related to a React application. But, I learn it when I was working in a React project. Thanks to ESLint, it raises *react/jsx-no-bind* warning and I discover there is a security issue with this simple code:

{% highlight jsx %}
<a href="http://malicious-website.com" target="_blank">Click here!</a>
{% endhighlight %}

I couldn't imagine with this one line of code on your website, it can bring a vulnerability to your application.

The attacker can put this following code on his malicious website:

{% highlight jsx %}
window.opener.location = "http://fake-facebook.com";
{% endhighlight %}

It can redirect the tab where your website was displayed to any website.

#### Solution

In your link, you just need to add *rel="noopener noreferrer"* and you should get the following code:

{% highlight jsx %}
<a href="http://malicious-website.com" target="_blank" rel="noopener noreferrer">Click here!</a>
{% endhighlight %}

Now, you are safe with this security issue.

## Conclusion

Here was my 4 mistakes I've done when I was working in React. I'm continuing to learn but I hope you can avoid doing the same mistake as me. If you have also some other anti-pattern, do not hesitate to leave below a comment. If you enjoy this article, I'll share more bad practices in React.
