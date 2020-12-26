---
layout: post
title:      "Jingle Bell Rails: Associations and Nested Forms All the Way"
date:       2020-12-24 16:09:19 -0500
permalink:  jingle_bell_rails_associations_and_nested_forms_all_the_way
---


What a rewarding project to complete by Christmas! I had so much fun creating this Ruby on Rails application. There were many challenges but I learned so much in the process, including the the wonderful convenience of the Active Record method, build, and nested forms.

I developed an application for musicians, where active contractors and musicians could connect for the same concern; finding work and finding employees . Users have access to a listing of gigs, authorization to submit applications upon interest of gigs, and views to fellow musician and contractor's profiles. Users have the option to sign up as a contractor, given authorization to post gigs and receive requests for gigs from applicatns and a direct link to the applicants page. These features aid the contractor in the hiring process.

Developing the associations with the desired features was the first challenge. Thanks to many draw.io drafts and my friends in the Flatiron cohort,  I concluded on 5 models with **belongs_to, has_many, has_many: :through, and many to many** relationships to establish the desired functions.

<a href="https://imgur.com/5enANuI"><img src="https://i.imgur.com/5enANuI.png" title="source: imgur.com" /></a>

I initially struggled to figure out how to make my model associations work based on my set user attributes. 
The User model has a contractor attribute with a boolean datatype that defaults to false, providing the user the option to sign up as contractor. 

In order to allow different functions for the type of user, I set my associations so that a  User could **belong_to :gig** AND **has_many :gigs, through: :requests**

```

class Gig < ApplicationRecord
    
    belongs_to :user
    
    has_many :gig_instruments
    has_many :instruments, through: :gig_instruments

    has_many :requests
    has_many :users, through: :requests
    
end
```

```
class User < ApplicationRecord
   
    has_many :requests
    has_many :posted_gigs, through: :requests, source: :gigs
    
    has_many :gigs

end
```

I had issues when calling **has_many :gigs, through: :requests** because it conflicted with my **has_many: gigs** association. That made sense because there was no way for Rails to know which association I was referring to when querying the database. With **source:**, I was able to distinguish these associations by renaming to **:posted_gigs** while providing the source, **:gigs** to distinguish the associations on the User model. 

Developing functioning nested forms was another trip. The Form Builder,  **fields_for** creates a scope around another object outside of its class which it is associated with. So in reference to my models, a gig is associated to instruments by **has_many :instruments**. With **fields_for** , we can nest a form inside the Gig form and scope in on the instrument class and its attributes to create instruments with a gig object. That’s it, right? WELL now in your controllers, you need ensure that you have instantiated this newly associated object in the new action in the Gigs Controller.

What if we want to make sure that this new instrument form that is nested inside the gig form is associated to a user? No problem! In the controllers, we can query IF the nested user is in fact the current user that I would like to create this new gig object on, then we can instantiate that object and instatiate the object to build an instrument on the gig form.

```   
def new
    if params[:user_id] && @user = User.find_by_id(params[:user_id])
        @gig = @user.gigs.build
      else
        @gig = Gig.new
    end
    @gig.instruments.build
end
	```
	
	```
	<%= form_for @gig do |f| %>

    <p><%= f.label :title %>
    <%= f.text_field :title %></p>

    <p><%= f.label :datetime %>
    <%= f.datetime_local_field :datetime %></p>

    <p><%= f.label :description %>
    <%= f.text_area :description %></p>

    <p><%= f.label :payment %>
    $<%= f.text_field :payment %></p>

    <p>Instruments required: </p>
    <p><%= f.label "Select Existing Instrument"%>
      
    <%= f.collection_check_boxes :instrument_ids, Instrument.all, :id, :name %><br>
		
		  <p> Or create new Instrument: </p>
  <div>
    <%=f.fields_for :instruments, @gig.instruments.build do |i| %>
      <p><%= i.label :name %>
      <%= i.text_field :name %>
    <% end %>
    <%=f.fields_for :instruments, @gig.instruments.build do |i| %>
      <p><%= i.label :name %>
      <%= i.text_field :name %>
    <% end %>

    <p><%= f.submit %>

<% end %>
```

Building this project helped me explore expand my knowledge on Rails. I had a lot of fun exploring different avenues to make my ideas possible while following convention.  I look forward to ringing into the new year with Javascript!
