React

I have using React for a while, since 2014 when we created Refly. React brought a lot of simplicity at that time, other frameworks, like Angular, tried to solve too much, imitating desktop app frameworks. However, with React, there was always something missing in the whole picture, some kind of guidance, architecture, organization. We didn't know, but solutions as flux, flex, reflux, Redux, etc. seemed to fill the gap, but adding complexity and new artifacts. In Creditlinks we mix React component with views rendered in the Python backend, and we didn't use any library to organize the complexity around React components. Fortunately, they didn't grow enough to create a problem for us. 

From the very beginning I tried to use functional components and avoid class component, I guess that's why hooks looked so good to me when they were released. However, after working with them in a big base of code, I quickly came to the conclusion that far from reducing the complexity, they make my code much more coupled. That was the time when I started thinking more on what React is and how to use it properly. The following are my conclusions so far.

* React is a reactive components' library. 
* A component describe a composable behavior. The React's team decided to include component's rendering, lifecycle and state inside the library to handle these concerns in a common way. 
* React's components can listen to events to exercise some view logic and update their internal state or children properties. 
* React's components react to changes in their properties or internal state, updating the DOM. 

That's all. So, why we started using it in a different way? Put too much logic in our UI/Components/whatever is a common mistake. Maybe that was the reason because the React's team came out with hooks, but it didn't make it better. The way for getting data from the outside world into the components was through the effect hook, and with it, all the problem came. The documentation proposed to make a fetch inside an effect with an empty dependency array. Sooner or later, we take advantage of it to add some transformations or even some domain logic and adding new dependencies on the dependencies' array and some logic to decide what was the reason because the effect was executed. Actually, what we were doing was to use effects to orchestrate our app, something React wasn't designed for.

But, what do I mean by logic and why is so bad to put too much into the components? I am going to distinguish between three types of logic:

* View logic: The checks that drive changes in our UI, i.e., when a value is higher than 5, change the color to red
* Orchestration logic: The logic that controls the different paths the execution can take.
* Business logic: The solution to the issue we want to solve.

According to react documentation, components created by different people should work well together. If you put orchestration logic or business logic inside your component, chances are that you break that rule. But even if this is not important to you, you must be aware that you are including unnecessary complexity inside your components and this complexity will push you to do things for what React components are not designed. Business logic lead you to move application state into component state, when you add state for what the component should not react your only option is to start writing orchestration logic inside the components, and then synchronization issues start appearing.

What if we stop using effects at all. Is it even possible? I think so. The [new React's documentation](https://beta-reactjs-org-git-you-might-not-fbopensource.vercel.app/learn/you-might-not-need-an-effect) explores some of these misused. But, what about those things you have to do when a component is displayed? We don't really need an effect for that, we only need an event to subscribe a handler. IMHO this approach is clearer and more consistent and in the end easier to explain and understand. React doesn't support it, but I think it isn't difficult to create a component that trigger an event when the component has been loaded.

React covers a small space in web application development, if we keep it in charge of the things it was created for we would have much fewer problems:

* Better performance
* Simpler code
* Fast and simple tests

