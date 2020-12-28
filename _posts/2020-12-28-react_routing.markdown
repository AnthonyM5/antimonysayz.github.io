---
layout: post
title:      "React Routing"
date:       2020-12-28 16:59:47 -0500
permalink:  react_routing
---

[React Router](https://reactrouter.com/web/guides/quick-start) allows developers to build single page applications, and still be able to utilize nagivatiable URLs, and routing.  React Router is an extension of the component principle of React, allowing you to import and deploy components to provide URLs, functional navigation, client-side routing, current app location, and much more.



For our single page application we will be using the web version of react router: 
https://npm.im/react-router-dom
```
npm install react-router-dom 
```

The router for our app consists of three parts: 
## Router
```
import { BrowserRouter as Router } from 'react-router-dom'
```
This allows us to use HTML5 history API that allows us to access the local session history on the cliient side.  This also gives us access to the app's current location.  

This component is ideally wrapped around your main App component:

```
<Router>
      <Provider store={store}>
        <App />
      </Provider>
    </Router>
```
## Switch/Route
```
import { Switch, Route } from 'react-router-dom';
```
Much like the switch case in Javascript the switch component returns the route that matches the location being passed in

```
<Switch>
          <Route path="/meals/:id" component={MealsPage} />
          <Route path="/meals" component={UserContainer} />
          <Route path="/foods/:id" component={FoodPage}/>
          <Route path="/foods" component={FoodCards} />
          <Route path="/uploads" component={FileReader}/>
        </Switch>
```

It is important to put your most specifc routes first, as the switch route will render the first route that matches the location being hit.

## Links
Lastly our app utilizes the link component, which as it's name suggests provides us a link component that can link to a plain string, or an object

```
<Link to={
          { pathname: `/foods/${id}`,
            state: {mealId}
          }
        }>{name}</Link></h5>
```

This allows us to access the locatation.state and pass over some information whenever the user clicks on this link.  The state can be accessed later via this.props.location.state.
