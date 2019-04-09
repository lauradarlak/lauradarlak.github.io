---
title: "Nested Forms in Rails"
categories: [rails]
---

Nested Forms in Rails allow you to handle multiple models, including creating and saving attributes of different models, from a single form. A functional nested form requires modification to the form view, relevant form models, as well as the controller for the primary form object.  This walkthrough will provide code snippets for each element of our example MVC (Model, View, Controller) that is necessary to create a working nested form.

In order to take advantage of *magical* Rails Form Builder Helpers, the intended associations between your application models must be set up by defining the associations in your models and in respective database table migrations.

For our example, we will create a dream journal with a form to record the content of a dream and tag relevant dream themes. The nested form will focus on the `dream` and `theme` models.

## Models

Our model relationships are specified as each `dream` `has_many` `themes` and a `theme` `belongs_to` a `dream`.

~~~ruby
# app/models/dream.rb

class Dream
  has_many :themes
  accepts_nested_attributes_for :themes

end

# app/models/theme.rb

class Theme
  belongs_to :dream

end
~~~

**`accepts_nested_attributes_for :themes`**

This class method is essential to our nested form and required for mass assignment.  The `accepts_nested_attributes_for` method will allow us to save theme attributes through the dream model within our ​`form_for(@dream)`​. As a result, an attribute writer for `themes` is created. The attribute writer in our example will be: `themes_attributes=(attributes)`

## Controller

Within the `new` action we are instantiating a new instance of a `dream`; this will allow us to use the `form_for` macro, essentially telling the form what model we are building the form for.  Next we will use the `collection.build` method to build a new collection of themes linked to the dream instance. We gain the `collection.build` method when we use the `has_many` macro in the `dream` model.

If we were working with the `belongs_to` macro, the `association_build` method would replace the `collection.build` method.

Lastly, we must update the strong params to include the `themes_attributes` hash. This addition will permit the `themes_attributes` hash to be used in mass assignment within the `create` action.

~~~ruby
# app/controllers/dreams_controller.rb

class DreamsController < ApplicationController
  def new
   @dream = Dream.new
   # This creates an empty theme collection
   @dream.themes.build
  end

 def create
  @dream = Dream.create(dream_params)
   if @dream.save
    redirect_to dream_path(@dream)
   else
    render ‘new’
   end
 end

 private

  def dream_params
    params.require(:dream).permit(:title, :content, theme_id, themes_attributes: [:name])
  end

end
~~~

### Params

The form we intend to build should store the user submitted data in the following params structure:

~~~ruby
params = {
  dream => {
    :title => 'Space Journey',
    :content => 'Fusce fermentum. Curabitur suscipit suscipit tellus. Suspendisse potenti. Vestibulum ullamcorper mauris   
		at ligula.',
    :themes_attributes => [
      {:name => 'space'},
      {:name => 'aliens'}
    ]
  }
}
~~~

## View

Instantiating a new `dream` instance in the `DreamController` `new` action allows us to take advantage of the `form_form` helper.  The dream instance (​`@dream`​) passed to the form builder informs the form builder object of the model we are creating the form for.

~~~
# app/views/dreams/new.html.erb

# this form is for a dream object
<%= form_form(@dream) do |f| %>

  <%= f.label :title %>
  <%= f.text_field :title %>
  <br>
  <%= f.label :content %>
  <%= f.text_field :content %>
  <br>
  Select related themes:
  <%= f.collection_check_boxes :theme_id, Theme.all, :id, :name %>
  <br>
  Or, add a new theme:
  <%= f.fields_for :themes do |theme|
    <%= theme.label :name %>
    <%= theme.text_field :name %>
  <% end %>
  <%= f.submit %>
<%= end %>
~~~

The `fields_for` will nest the value of the newly added `theme` to the `dream_params`. The HTML form input tag will resemble our desired params structure and render as: `<input type=“text” name=“dream[themes_attributes][0][name]” id=“dream_themes_attributes_0_name”>`.

Lastly, we must update the `dream` model to prevent the creation of blank `themes` in the nested attribute.

~~~ruby
# /models/dream.rb

class Dream
  has_many :themes
  accepts_nested_attributes_for :themes, reject_if: :all_blank

end
~~~
