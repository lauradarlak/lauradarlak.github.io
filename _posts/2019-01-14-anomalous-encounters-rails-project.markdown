---
title: "Rails Portfolio Project: Anomalous Encounters"
categories: [portfolio projects]
classes: wide
excerpt: "Anomalous Encounters (AE) is a public repository of extraterrestrial, supernatural, and unexplainable experiences. Built with Rails, AE is a database-backed web application that allows users to record individual encounters that are visible throughout the platform."
---

## Project Details

Anomalous Encounters (AE) is a public repository of extraterrestrial, supernatural, and unexplainable experiences. Built with Rails, AE is a database-backed web application that allows users to record individual encounters that are visible throughout the platform. Users must be logged in to create, edit or delete an encounter. User authentication is handled through the Devise gem with Google Oauth integration, which authenticates users' accounts with their Google account.

![](https://i.imgur.com/exZITYhl.png?1)

The AE root URL ( ‘/‘ ) does not require authentication and renders the same primary content independent of a user’s logged in status, with the exception of a navigation content. The 25 most recently persisted encounters are displayed in the main column of the  root URL. This view is achieved through the use of a custom Encounter model scope method and corresponding controller action.  If an encounter record belongs to the current user,  a link to edit the encounter will display in the header of the encounter card.  

![](https://i.imgur.com/f3Kni3zl.png?1)

Encounter records support user submitted tagging. This feature requires a has_many through relationship between the apps User and Tag models with a Tagging join model.  

## Project Process

One topic that created a snowballing effect was my decision to nest the RESTful encounter resources within a named parameter scope:

~~~
​  scope ':display_name', as: 'user' do    
      resources :encounters  
   end
~~~

​​Creating similar routes to:

~~~
​/janedoe/encounters/new
​/janedoe/encounters/1
​/janedoe/encounters/1/edit
~~~

The named parameter scope, which is an attribute of the `User` model, functions as a nested route because the scope captures the `has_many`​and `belongs_to`​model relationship between `Users` and `Encounters`.  ​For example, from the route `/janedoe/encounters/new` the created encounter will be associated with the named parameter prefix of the URL.

![](https://i.imgur.com/pOND8X6m.png)

Prefixing the nested encounters with the named parameter scope presented an issue when users sign up for AE via Google Oauth. Google Oauth created users will not be assigned a display name, which is a necessary attribute to access the nested encounters routes. When a user signs up via Google, the user is logged in, redirected to the root URL, and if the user does not have a display name, presented with a flash message to follow a link to finish the sign up process and create a display name. Logged in user’s who attempt to create an encounter without first having a display name will be redirected to the finish signup url to create a display name.

An alternate approach to ensuring that all user’s have a display name is to validate the presence of a display name in the `User` model, then assign the display name with the user’s first and last name supplied by Google. Disclosing anomalous encounters is still considered stigmatizing, and maintaining user anonymity is important to AE, therefore I opted against the later approach.

## Final Thoughts

Overall, I was enthralled by the process of creating Anomalous Encounters, which made it difficult to stick to the MVP (Minimally Viable Product) submission point. It was important to me to create an attractive user experience for this app. I look forward to continuing to refactor this project and implementing new features.
