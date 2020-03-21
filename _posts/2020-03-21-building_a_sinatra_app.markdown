---
layout: post
title:      "Building A Sinatra App"
date:       2020-03-21 19:36:16 +0000
permalink:  building_a_sinatra_app
---

Recommended Reading Materials:

Validations: 
https://www.tutorialspoint.com/ruby-on-rails/rails-input-validations.htm
ActiveRecord Methods: https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html#method-i-add_column



Welcome to the Hotel California Auction House!  The aim of this project is to create a Sinatra app that contains multiple models, relationships, and validates certain inputs.  

For this app, since we are looking to store unique users, and secure their passwords, we use ActiveRecord::Base to be able to add functionality of looking up the table associated to this model.  We want to establish:
1) Unique Email Address
2) Secure Password
3) Relationships to additional models

```
class User < ActiveRecord::Base
    has_secure_password
    has_many :bids
    has_many :auctions, through: :bids 
    validates :username, :email, uniqueness: true
```


This project requires multiple models, so this User class has a relationship where it is attached to a bids model, which tracks individual bids made by each user, and an auctions model, which can track auctions created by this user.  For more information regarding the relationships, please refer to the ActiveRecord documentation!

The front-facing portion of this application is stored in the controllers, and best practice dictates that we seperate logic for each action with a different controller.  For example, this project will have a users controller that specifically will control the log-in, sign up functionality.  The main controller, ApplicationController will primarily deal with the home page, and helps us define any functions that can then be extended to other controllers.  
```
class ApplicationController < Sinatra::Base
  
  configure do
    register Sinatra::Flash
    set :public_folder, 'public'
    enable :sessions
    set :views, 'app/views'
    set :session_secret, "secret_password"
  end

```

Here we can set up sessions, to record the unique session ID per user, and help us validate who is currently accesing our app and allow us to gate who has access to certain CRUD actions later on.  

By extending ApplicationController to our UsersController, AuctionsController, and BidsController, we can extend functionality to enable our other controllers to verify user sessions, use the views folder that contains our .erb files, and acess the stylesheets in our "public" folder.  Our subsequent controllers would just simply need to inherit from Application controller

```
class AuctionsController < Applications Controller
```

After your controllers are set up, your models are good to go we also must "mount" our controllers in our config.ru file which would launch our app.

```
use Rack::MethodOverride
use AuctionsController
use UsersController
use BidsController
run ApplicationController
```


> As detailed in Learn.co lesson "Sinatra Multiple Controllers": 
> Only one class can be specified to be run. The other class must be loaded as Middleware. We won't get into MiddleWare now, suffice to say, you simply use it instead of run
> 

From here, we would need to set up our database in ActiveRecord!  Since our models have has_many, and belongs_to relationships, we must be careful to note which tables will have a foreign key attached, and how this relationship will be exposed in our models.  For this application, we would want to be able to locate a users ID, and associate that ID to the bids table, and the auctions table to find records.

The schema for my bids table would look something like this

```
create_table "bids", force: :cascade do |t|
    t.integer "bid_amount"
    t.integer "user_id"
    t.integer "auction_id"
  end
```

By using the shotgun gem, we can load up our config.ru file, and test functionality of our app to make sure things run as they are supposed to, and we can check if our CRUD actions are enabled correctly, that the routes in our controllers are going to the right view pages, and most importantly the session IDs are correctly being mapped.  Our CRUD actions for auctions should ONLY be accessible if the user ID matches the auctions table, so they can create, update, and destroy the auctions resource.  The CRUD actions for bids is a little bit more stringent in that any user that is logged in can view and submit a bid.

Once this is enabled, we can add additional functionality to come:
End Date Functionality => Ideally this would be able to reroute the pages to an alternate view where bidding is no longer allowed, and a winning bid is announced.
Image Upload => Implement file storage capabilities (or for now, just storing image URLs and rendering that).
Editing Bids => Remove a bid, or retract a bid










