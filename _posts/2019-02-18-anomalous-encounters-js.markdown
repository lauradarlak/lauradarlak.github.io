---
title: "Rails with JavaScript Portfolio Project"
categories: [portfolio projects]
classes: wide
---

## Rails with JavaScript Portfolio Project

Anomalous Encounters JS is an extension of an existing [Rails project](http://memyselfandruby.com/anomalous_encounters_rails_project). The goal of this project was to add dynamic features to the existing Rails application through the use of JavaScript and a JSON API.


### Render Index Resource via AJAX GET

The feature updates include rendering an index resource via Javascript and Active Model Serialization JSON backend. The root path for the application displays a summary card for all persisted encounters.

![](https://i.imgur.com/Fq7WQWll.png?1)

The encounter summary card details are fetched using a AJAX GET request. A successful request will render the encounter details in JSON format from the backend. A JavaScript `forEach() `method initializes an Encounter JavaScript Model Object on each encounter node represented in the JSON data response.

~~~ javascript
// Get all encounters via AJAX request

function indexEncounters() {
  $.ajax({
     url: 'http://localhost:3000/encounters',
     method: 'get',
     dataType: 'json',
     success: function(json) {
       json.forEach(encounter => {
         var newEncounter = new Encounter(encounter)
         var shortEncounterRender = newEncounter.renderShortCard();

         $("#encounter-details-" + newEncounter.id).prepend(shortEncounterRender)
         console.log("rendered!")
       })
     }
   })
}
~~~

Next, the Encounter object is rendered to the page by calling a prototype method on the object, which renders a HandlebarsJS  template:

~~~ javascript
// Encounter prototypes to render handlebars templates

Encounter.prototype.renderShortCard = function(){
  return Encounter.shortTemplate(this)
}
~~~

then appending the Encounter attributes onto the page within a styled card:

~~~ ruby

<div class="card encounter-card" id="encounter-<%=encounter.id%>">
  <div class="card-body">
    <h5 class="card-title"><%= link_to "Case ID: #{encounter.id}", user_encounter_path(encounter.user.display_name, encounter.id), class: "show-link" %>
      <% if user_signed_in? && encounter.user_id == current_user.id %>
        <span class="pull-right"><%= link_to 'Delete Encounter', user_encounter_path(encounter.user.display_name, encounter), method: :delete %></span>|
        <%= link_to "Edit Encounter", edit_user_encounter_path(encounter.user.display_name, encounter) %>
      <% end %>
    </h5>
    <div class="encounter-details" id="encounter-details-<%=encounter.id%>">
    </div>
  </div>
</div><!-- /.card -->

<script id="brief-encounter-template" type="text/x-handlebars-template">
  <h6 class="card-title">Reported by: <a href="/{{user_display_name}}/encounters">{{user_display_name}}</a></h6>
  <p class="card-text">Date of Encounter: {{date}}</p>
  <p class="card-text">State: {{state}}</p>
  <p class="card-text">Description of Encounter: {{short_description}}</p>
  <p class="card-text">Category: <a href="/categories/{{category_slug}}">{{category}}</a></p>
  <div class="card-footer">
    Tags:
    <span><a class="badge badge-info" href="/tags/{{this.slug}}">{{this.name}}</a></span>
  </div>
</script>

~~~


### Render Show Resource via AJAX GET

A similar pattern is used to dynamically render an individual encounter. By hijacking the event when clicking on the Case ID of an encounter card, an AJAX GET request is made to the show resource for that individual encounter. The JSON response is rendered in a detailed Handlebars template.

~~~ javascript
function showEncounter() {
  console.log('listening..')
  $('a.show-link').on('click', function(e) {
    e.preventDefault()
      $.ajax({
      url: this.href,
      method: 'get',
      dataType: 'json',
      success: function(json) {
        var newEncounter = new Encounter(json)
        var fullEncounterRender = newEncounter.renderFullCard();
        $("#encounter-details-" + newEncounter.id).empty();
        $("#bannerHeading").text("Encounter Details");
        $("a.show-link").removeAttr("href");

        $("section#encounters").append("<a class=\"btn btn-primary btn-lg d-block text-uppercase\" href=\"/\">View All Encounters</a>")

        $("div.encounter-card").not("#encounter-" + newEncounter.id).remove()
        $("#encounter-details-" + newEncounter.id).prepend(fullEncounterRender)
        console.log("rendered!")
      }
    })
  })
}

~~~


### Render Form via AJAX POST and Submit Dynamically

The form to submit a new encounter is rendered to the page using another AJAX GET request. When the form is submitted, the form is serialized and a AJAX POST request is initiated. The dataType for the AJAX POST request is set to `script`; this tells the Rails backend to return a **Server-generated JavaScript response**. Once the form is successfully submitted a JavaScript response will render a JavaScript template.  Rails will use implicit rendering to render the `create.js.erb` template.

~~~ javascript
function submitEncounterForm() {
  $("#new_encounter").on("submit", function(e) {
    e.preventDefault()

    console.log('submit listening..')
    $.ajax({
      url: this.action,
      type: 'POST',
      dataType: 'script',
      data: $(this).serialize(),
      success: function(data) {
        console.log("submitted encounter!")
        $("section#encounters").append("<a class=\"btn btn-primary btn-lg d-block text-uppercase\" href=\"/\">View All Encounters</a>")
        $("a.show-link").removeAttr("href");
      }
    })
  })
}
~~~

From within the `create.js.erb` template, a rails partial is rendered with a detailed encounter card for the newly persisted encounter.

~~~
$("section#encounters").html("<%= escape_javascript(render('encounters/encounter_template', :encounter => @encounter)) %>");

console.log("create js")
~~~

I opted to handle the submitted form data with a server-generated JavaScript response because the encounter cards contain backend logic that determines whether the card displays edit and delete links depending on whether the encounter belongs to the currently logged in user. By using a partial, I was able to access the instance of the newly submitted encounter and utilize the Ruby logic within the partial.

![](https://i.imgur.com/cg4SnVcl.png)
