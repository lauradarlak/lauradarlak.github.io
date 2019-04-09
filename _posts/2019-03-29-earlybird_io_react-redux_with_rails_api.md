---
title: "earlybird.io: React-Redux with Rails API"
categories: [portfolio projects]
classes: wide
---

## Project Details

earlybird.io is an ecommerce platform for farmers markets built with a React-Redux front-end and connected to a Rails API backend. Have you ever gone to a farmer’s market only to find that every vendor is sold out of eggs, organic bacon, or fresh whole chickens? earlybird.io is a solution to ensuring that Locovores, like you, always get the fresh food you're looking, without having to arrive at the farmers market by dawn. Also, by shopping with earlybird.io farmers know how much product to bring to market and are able to sell more product.

### React-Router

This application is made up of four routes provided by react-router’s dynamic routing. The route matching components, which determine the correct component to render based on the current location, are found with App.js.

```
<Switch>
  <Route exact path='/' component={Home} />
  <Route exact path='/categories/:category_slug' component={ProductsContainer} />
  <Route exact path='/order' component={Order} />
  <Route exact path='/cart' render={() => <FinalCart cart={this.props.cart} />} />
</Switch>
```

The '/cart' path that renders the FinalCart component, a stateless presentational component. This component renders details from the cart state, managed in the redux store. In order to pass the cart state to the FinalCart component, the cart state must be available as a prop, via mapStateToProps. Instead of passing the component prop to the Route component, the render prop is used and passed an inline function with the FinalCart component as an expression passed the cart props.

### Asynchronous Actions

Asynchronous calls to the app's Rails API are made with Redux Thunk middleware. Async actions are required to receive data for the products available at the farmers market. Additionally, when a order is completed, an async action sends a PATCH request to the server, updating the product inventory for the products purchased in the order.

```
// ** Async Actions **

export const updateInventory = (item) => {
  fetch(`http://localhost:3001/api/products/${item.id}`, {
    method: 'PATCH',
    body: JSON.stringify(item),
    headers: {
      'Content-Type': 'application/json'
    }
  })
  .then(res => {
    if(res.ok) {
      redirect: window.location.replace("http://localhost:3000/order")
    } else {
      throw Error(`Request rejected with status ${res.status}`);
    }
    return res.json()
  })
  .catch(console.error)
}

```

It was necessary to check ok on the fetch call response object. If res.ok is true, the response was successful and the current window location is redirected to the order route.
