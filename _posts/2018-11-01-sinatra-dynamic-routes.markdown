---
title: "Sinatra Dynamic Routes"
categories: [ruby]
---

Sinatra is a DSL (Domain Specific Language) that is used to create web applications by providing Ruby methods for handling HTTP requests. For example, Sinatra methods include get, post, put, and a delete, each relating a HTTP method. HTTP methods that have a URL path and a block that handles the request is a route.

~~~ruby
get '/dreams' do
  'Here is a list of dreams.'
end
~~~

The above route handles a get request for the /dreams URL and responds with the method block.  Web application can contain scores, if not hundreds, of routes, for example:

~~~ruby
get '/dreams/1' do
  'Details on dream 1.'
end

get '/dreams/2' do
  'Details on dream 2.'
end

get '/dreams/3' do
  'Details on dream 3.'
end

get '/dreams/4' do
  'Details on dream 4.'
end
~~~

Dynamic routes are implemented to avoid hard-coding each new route, like we see above, and to DRYout our routes.
When Sinatra receives a HTTP request it searches through all the available routes, typically coded in the application’s controller, and looks for a HTTP and url path that matches the request. The matching route can be a url pattern, rather than an explicit url.

~~~ruby
get '/dreams/:id' do
  @dream = Dream.find(params[:id])
  erb :'dreams/show'
end
~~~

The above route pattern represents a dynamic route for a get request with a url that contains /dreams and ends with :id. The symbol :id is a dynamic attribute that changes depending on which dream the client is attempting to access. This dynamic attribute found in the url becomes accessible through a params ​hash. The ​params​ hash includes data contained in the HTTP request, and in our example :id​ becomes a key nested in the params hash. Now that the :id key value pair is stored in the params hash, we are able to access the data in the route body:

~~~ruby
​@dream = Dream.find(params[:id])
~~~

The above line uses the ActiveRecord .find method, which retrieves the database record with the corresponding id/primary key. For the url /dreams/3, the Sinatra route would look for the instance of a Dream object mapped to a row in the active record dream table with an id = 3.
