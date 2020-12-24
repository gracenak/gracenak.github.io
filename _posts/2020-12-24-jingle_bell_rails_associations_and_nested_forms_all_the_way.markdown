---
layout: post
title:      "Jingle Bell Rails: Associations & Nested Forms All the Way"
date:       2020-12-24 21:09:18 +0000
permalink:  jingle_bell_rails_associations_and_nested_forms_all_the_way
---


What a rewarding project to complete by Christmas! I had so much fun creating this Ruby on Rails application. There were many challenges but I learned so much in the process, including the the wonderful convenience of the Active Record method, build, and nested forms. 

I developed an application for musicians, where active contractors and musicians can connect in one application for the same concern; finding work and finding employees . Users have access to a listing of gigs, can submit applications upon interest of gigs, and meet fellow musicians/contractors through their profile. As a contractor, you can post gigs and will be provided with a listing of applications received at your home page. The contractor will be able to review their profile to aid in the hiring process.

This MVC application has 5 models with belongs_to, has_many, has_many: :through, and many to many relationships. THAT was the first challenge. Thanks to many draw.io drafts and a kind ear from my friends in this cohort that I was able to establish these relationships: 

![](https://imgur.com/5enANuI)

I had initially struggled to figure out how to make my model associations work based on how I had set my user attributes. User has a contractor attribute with a boolean datatype so that the user has the option to sign up as a musician looking for gigs or a contractor having the extra feature to post gigs). I decided that a Gig could belong_to :user AND has_many: users, through: :requests.

```class Gig < ApplicationRecord
    
       belongs_to :user
    
       has_many :requests
       has_many :users, through: :requests
		
		   has_many :gig_instruments
       has_many :instruments, through: :gig_instruments
		
		end ```
		
		and
		
		```class User < ApplicationRecord
   
    has_many :requests
    has_many :posted_gigs, through: :requests, source: :gigs
    
    has_many :gigs
		
		end ```
		
		I was having issues when calling ```has_many :gigs, though: :requests``` because it was conflicting with ```has_many: gigs```  and Rails could not possibly know which association I was referring to. I learned a great syntax to resolve this issue. With **:source**, I was able to ask Rails to look for an association **:posted_gigs** through its root source, **:gigs** on the User model.
		
		
	The process of having functioning nested forms was a bit pesky. The Form Builder **fields_for** allows you to create a scope around another object outside of its class.  This allowed me to create an object associated to the scoped model. Sounds pretty straight forward, right? WELL now in your controllers, you need ensure that in the new action, you are initiating that object. Did I also want to associate this form and nested form to a user? YUP! So I needed to make that **IF** the nested user was in fact the current user that I wanted to build a gig on, then I could instatiate that object and then also instantiate the object to build an instruments on the gig form.
	
	```    def new
        if params[:user_id] && @user = User.find_by_id(params[:user_id])
            @gig = @user.gigs.build
          else
            @gig = Gig.new
        end
        @gig.instruments.build
    end```
		

Building this project challenged me to follow my breadcrumb of errors to understand the MVC seperation of concerns and associations of this application. I was happy to build an application of my own construction and design to further build my knowledge. Looking forward to ringing into the new year to Javascript!
		

	
	
	
		
		




