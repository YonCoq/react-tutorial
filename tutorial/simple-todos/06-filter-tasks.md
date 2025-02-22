---
title: "6: Filter tasks"
---

In this step, you will filter your tasks by status and show the number of pending tasks.

## 6.1: useState

First, you are going to add a button to show or hide the completed tasks from the list.

The `useState` function from React is the best way to keep the state of this button. It returns an array with two items, where the first element is the value of the state, and the second is a setter function that is how you are going to update your state. You can use _array destructuring_ to get these two back and already declare a variable for them.

Bear in mind that the names used for the constants do not belong to the React API, you can name them whatever you like.

Also, add a `button` below the task form that will display a different text based on the current state.

`imports/ui/App.jsx`
```js
import React, { useState } from 'react';
..
export const App = () => {
  const [hideCompleted, setHideCompleted] = useState(false);

  ..
    <div className="main">
      <TaskForm />
       <div className="filter">
         <button onClick={() => setHideCompleted(!hideCompleted)}>
           {hideCompleted ? 'Show All' : 'Hide Completed'}
         </button>
       </div>
  ..
```

You can read more about the `useState` hook [here](https://reactjs.org/docs/hooks-state.html).

We recommend that you add your hooks always in the top of your components, so it will be easier to avoid some problems, like always running them in the same order.

## 6.2: Button style

You should add some style to your button so it does not look gray and without a good contrast. You can use the styles below as a reference:

`client/main.css`

```css
.filter {
  display: flex;
  justify-content: center;
}

.filter > button {
  background-color: #62807e;
}
```

## 6.3: Filter Tasks

Now, if the user wants to see only pending tasks you can add a filter to your selector in the Mini Mongo query, you want to get all the tasks that are not `isChecked` true.

`imports/ui/App.jsx`
```js
..
  const hideCompletedFilter = { isChecked: { $ne: true } };

  const tasks = useTracker(() =>
    TasksCollection.find(hideCompleted ? hideCompletedFilter : {}, {
      sort: { createdAt: -1 },
    }).fetch()
  );
..
```

## 6.4: Meteor Dev Tools Extension

You can install an extension to visualize the data in your Mini Mongo.

[Meteor DevTools Evolved](https://chrome.google.com/webstore/detail/meteor-devtools-evolved/ibniinmoafhgbifjojidlagmggecmpgf) will help you to debug your app as you can see what data is on Mini Mongo. 

<img width="800px" src="/simple-todos/assets/step06-extension.png"/>

You can also see all the messages that Meteor is sending and receiving from the server, this is useful for you to learn more about how Meteor works.

<img width="800px" src="/simple-todos/assets/step06-ddp-messages.png"/>

Install it in your Google Chrome browser using this [link](https://chrome.google.com/webstore/detail/meteor-devtools-evolved/ibniinmoafhgbifjojidlagmggecmpgf).

## 6.5: Pending tasks

Update the App component in order to show the number of pending tasks in the app bar.

You should avoid adding zero to your app bar when there are no pending tasks.

`imports/ui/App.jsx`
```js
..
  const pendingTasksCount = useTracker(() =>
    TasksCollection.find(hideCompletedFilter).count()
  );

  const pendingTasksTitle = `${
    pendingTasksCount ? ` (${pendingTasksCount})` : ''
  }`;
..

    <h1>
      📝️ To Do List
      {pendingTasksTitle}
    </h1>
..
```

You could do both finds in the same `useTracker` and then return an object with both properties but to have a code that is easier to understand, we created two different trackers here.

Your app should look like this:

<img width="200px" src="/simple-todos/assets/step06-all.png"/>
<img width="200px" src="/simple-todos/assets/step06-filtered.png"/>

> Review: you can check how your code should be in the end of this step [here](https://github.com/meteor/react-tutorial/tree/master/src/simple-todos/step06) 

In the next step we are going to include user access in your app.
