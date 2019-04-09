---
title: "Dream Catcher: Sinatra App"
categories: [portfolio projects]

---

## Overview
Dream Catcher is a CRUD, Model-View-Controller application using Sinatra. This app allows users to keep track of their dreams. The user can assign a dream with a name, date and description, as well as user-created themes. Users' dream data is persisted in a SQLite3 database and accessed by Active Record Object Relational Mapping (ORM). User's are able to create new dreams, view individual dreams and a dream index, edit and delete dreams. A theme index is generated for each user, which allows user’s to explore connections between dream themes.

Dreams are gateways into ones unconscious mind, and therefore often regarded as sensitive, personal information. The Dream Catcher app ensures that users can't view or modify content created by other users, this is achieved by setting the sessions hash user_id key equal to the user's ID upon login. A current_user helper method finds the user whose ID is set to the session hash user_id key. Throughout the DreamController, `@dream.user == current_user` is used to ensure that the `session[:user_id]` matches the user ID associated to a particular dream. If a match is not found, the logged in user will not be able to access the requested dream or any information related to that dream.

## Process
Before writing a single line of code, I outlined my models and associations for this project. After building my models, I created a series of Active Record migrations that created tables for each of my models to map onto. I utilized the tux gem to ensure that my associations were working as intended. This planning and development process provided a comfortable foundation to build out the other project requirements.

The creation of a Sinatra CRUD app is formulaic by virtue of using a DSL (Domain Specific Language). To add uniqueness to the project, my original vision for Dream Catcher included scraping an online dream dictionary for information related to the dream themes assigned to each user created dream; this feature went beyond the project requirements. An entire work day spent on the scraping feature, resulted in a broken feature and several project requirements that remained unfulfilled. I decided to leave the scraping feature to be developed after the project assessment. I built out the theme feature in a different direction by creating a show route for each dream theme and displaying other themes that a user dreamed about when dreaming about a particular theme instance.

<a href="https://imgur.com/QJch6EC"><img src="https://i.imgur.com/QJch6ECh.png" title="source: imgur.com" /></a>
