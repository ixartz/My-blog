---
title: '4 bad practices that you should not do in React'
date: 2018-12-31
description: Mistakes I have done in React
categories:
  - React
tags:
  - React
  - Tips
---
Lately, I have used intensively React in my work but also in my personal project. Here, I will share the mistakes I have done in my React code. And, what you should also avoid doing in your project.

### Arrow function in the render function

The first thing you should avoid is to inline arrow function in React's render function. Here is an example:

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

What is wrong with the previous code? Well, the function is re-created at each rendering of the parent component. As you can guess, it will hurt application performance in two ways. First, it will create an unnecessary anonymous function at each rendering of parent component.

Then, because it creates a new anonymous function, React will also trigger a re-render of child component.

#### Solution

It is very easy to solve, you should not declare your arrow function inside of render. You should move the arrow function as class field. Then, child component props should refer to this class field. Here is the solution:

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

### Nested state

I made a mistake to try working with nested state in React. A nested state is putting object into react State. For example this following code is a nested state:

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

The issue with nested state happens when you try to update *coord* object:

{% highlight jsx %}
coord.x = 10;

this.setState({
  coord
});
{% endhighlight %}

You are expecting the component being rendered again. Unfortunately, it is not the case. React makes a shallow compare on component state and it will no see there is a change in the state.

#### Solution

Here is the solutions you can follow based on your context:

1. Just change your design and avoid using nested state
2. Use destructuring, it will un-nested your object into the state
3. You can also create a new object yourself when you make a change. But, what I suggest is to use immutable library. Facebook provides Immutable.js, it will do the job.

Each solution has his own advantages and disadvantage. You should choose a solution based on the context.

### Show/Hide component with conditional rendering

As you may know, React allows you to render a component based on conditions. I thought I could benefit this conditional rendering to show/hide components. Actually, it was a wrong idea. Indeed, even if it works well but behind the scene, the component was unnecessarily re-created each time we show/hide the element.

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
        // Here is the conditional rendering
        {this.state.show && <Label />}
      </div>
    );
  }
}
{% endhighlight %}

The above code will toggle **Label** component each time when we click on the button. It works very well to hide/show the **Label** component for each click. But, there is a major drawback: each time the show back the **Label** component, it will instantiate a new instance and it will re-create a new one from scratch.

#### Solution

The best way in React to show or hide a component is to use CSS. A simple **display** CSS property can be used to show/hide a component without re-creating it.

### target="_blank" security

Thanks to ESLint, I discover there is a security issue with this simple code:

I could not imagine with this one line of code, it can bring a vulnerability to your application.

Indeed, in the targeted link with another simple code: . It can redirect to malicious website.

#### Solution

In your link, you just need to add

and you should get the following code:

Now, you are safe to redirection with opener.

## Conclusion

I have just shared 4 mistakes I have done when I was working in React. I am continuing to learn but I hope you can avoid doing the same mistake as me. If you have also some other anti-pattern, do not hesitate to leave below a comment.
