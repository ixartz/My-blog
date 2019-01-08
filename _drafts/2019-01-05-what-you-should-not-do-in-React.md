---
title: '4 bad practices that you should not do in React'
date: 2019-01-05
description: Mistakes I have done in React
categories:
  - React
tags:
  - React
  - Tips
---
Lately, I have used intensively React in my work but also in my personal project. Here, I will share the mistakes I have done in my React code. And, what you should also avoid doing in your project.

You can access one of my personal project using React [at this location](https://github.com/ixartz/handwritten-digit-recognition-tensorflowjs). The 4 mistakes I list here was done in this project where I implement a digit recognizer. This project helps me learn Redux, Tensorflow.js, styled-components, Ant Design, etc. I am very happy to share what I learn in this deep learning and React project.

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

What is wrong with the previous code? Well, the function is re-created at each rendering of the parent component. As you can guess, it will hurt application performance in two ways. First, it will create an unnecessary anonymous function at each rendering of the parent component.

Then, it creates a new anonymous function, React will also trigger a re-render of child component.

#### Solution

It is very easy to solve, you should not declare your arrow function inside of render. You should move the arrow function as a class field. Then, child component props should refer to this class field. Here is a solution:

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

The issue with nested state happens when you try to update *coord* object:

{% highlight jsx %}
coord.x = 10;

this.setState({
  coord
});
{% endhighlight %}

You are expecting the component being rendered again. Unfortunately, it is not the case. React makes a shallow compare on component state and it will no see there is a change in the state.

#### Solution

Here are the solutions you can follow based on your context:

1. Just change your design and avoid using nested state
2. Use destructuring, it will un-nested your object into the state
3. You can also create a new object yourself when you make a change. But, what I suggest is to use *immutable* library. Facebook provides *Immutable.js*, it will do the job.

Each solution has his own advantages and disadvantage. You should choose a solution based on your context.

### Show/Hide component with conditional rendering

As you may know, React allows you to render a component based on conditions. I thought I could benefit this conditional rendering to show/hide components. Actually, you should use conditional rendering for toggling small components. But, for complex ones, you should avoid. Indeed, even if it works well but behind the scene, the component was unnecessarily re-created each time we show/hide the element.

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

The above code will toggle *ComplexComponent* component each time when you click on the button. It works very well to hide/show the *ComplexComponent* component for each click. But, there is a major drawback: each time we display back the *ComplexComponent* component, it will instantiate a new instance and it will re-create a new one from scratch.

You should avoid using conditional rendering. Especially, when the *ComplexComponent* component has a resource-consuming constructor and/or mounting process.

#### Solution

The best way in React to show or hide a component is to use CSS. A simple *display* CSS property can be used to show/hide a component without re-creating it.

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

### target="_blank" security

Thanks to ESLint, I discover there is a security issue with this simple code:

{% highlight jsx %}
<a href="http://malicious-website.com" target="_blank">Click here!</a>
{% endhighlight %}

I could not imagine with this one line of code on your website, it can bring a vulnerability to your application.

On his malicious website, the attacker can put this following code:

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

I have just shared 4 mistakes I have done when I was working in React. I am continuing to learn but I hope you can avoid doing the same mistake as me. If you have also some other anti-pattern, do not hesitate to leave below a comment.
