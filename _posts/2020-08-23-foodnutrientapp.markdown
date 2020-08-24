---
layout: post
title:      "FoodNutrientApp  "
date:       2020-08-23 20:37:13 -0400
permalink:  foodnutrientapp
---

I developed a rails application that imports some of data provided by the USDA Food Data Central database via their API to build an index of food records that each contain unique nutrient values.  This app uses ActiveRecord to create food objects that contain their unique nutrient values as attributes for (relatively) quick and painless querying!  This allows us to be able to query our objects by name, and attribute (nutrient value).  
For example: 

![Search](https://i.ibb.co/QMFGBbC/Filtered-Search.png)



How do we access this information?  The [Food Data Central API](https://fdc.nal.usda.gov/api-guide.html) provides us multiple end points, but we will only be using the query endpoint to build a local database of the information.  I used  [Postman](https://www.postman.com/downloads/) to help build the code to connect to the API with my credentials.  (note the Food Data Central Key is stored in your local .env file, and will require your own  [API Key.](https://fdc.nal.usda.gov/api-key-signup.html))


Taking a peak into the seed file for the app:
```
def food_index
    url = URI("https://api.nal.usda.gov/fdc/v1/foods/search?dataType=SR Legacy, Foundation&currentPage=1&sortOrder=desc&pageSize=8000")

    https = Net::HTTP.new(url.host, url.port);
    https.use_ssl = true

    request = Net::HTTP::Get.new(url)
    request["x-api-key"] = ENV["FDC_KEY"]

    response = https.request(request)
    index = response.read_body
    JSON.parse(index)
end

```

The JSON object that the FDC API returns to us looks something like this: 

```
 {
            "fdcId": 171687,
            "description": "Acerola juice, raw",
            "dataType": "SR Legacy",
            "ndbNumber": "9002",
            "publishedDate": "2019-04-01",
            "foodNutrients": [
                {
                    "nutrientId": 1062,
                    "nutrientName": "Energy",
                    "nutrientNumber": "268",
                    "unitName": "kJ",
                    "value": 96.00000000
                },
                
```

Each food record contains an array of "foodNutrients" that yields individual hashes containing the unique nutrient identifiers, and nutrient value.  Initially, as a quick and easy way to store this information ActiveRecord provides us a [seralize ](https://api.rubyonrails.org/classes/ActiveRecord/AttributeMethods/Serialization/ClassMethods.html) method that will store and return objects, in this case our in tact array of hashes.  The create method for our model would look something like this:

```
serialize :nutrient_hash


    def self.create_by_food_hash(food)
        create(
            name: food["description"],
            nutrient_hash: food["foodNutrients"],
        )
    end
```

This allows for us to create individual show pages for our foods that can list their nutrient values by iterating through the saved array of nutrient hashes.

In order to fully utilize [ActiveRecordQuery](https://guides.rubyonrails.org/active_record_querying.html) we had to somehow attach the information contained in the array of hashes with a method in the food model.



To achieve this I converted the array of nutrient hashes (saved during intial creation) into one hash that only contained the Nutrient ID as the key, and it's associated value.

```
    def food_nutrient_hash(food)
        new_hash = Hash.new
        food.nutrient_hash.each do |nutrient|
            nutrient_key = nutrient["nutrientNumber"]
            new_hash[nutrient_key] = nutrient["value"] if nutrient_key
        end
        food.update_nutrient(food, new_hash)
    end
```

This hash can then be used for mass assignment and updating each food nutrient attribute by calling the appropriate nutrient ID.  Calling the key (Nutrient ID) that corresponds to the attribute we want to update will provide us either the value we stored from the FDC hash, or nil.  


```
  def update_nutrient (food, nutrient_hash)
        food.update(
            total_lipid: nutrient_hash["204"],
            protein: nutrient_hash["203"],
            carbs: nutrient_hash["205"],
            fiber: nutrient_hash["291"],
```

This was necessary as a work around to the fact that when certain foods did not contain a nutrient we were updating or creating this returned a no method error.  

This is all called when you run 
```
rake db:seed
```
and will take quite some time, but in the end we will be able to use the [order method](https://guides.rubyonrails.org/active_record_querying.html#ordering) to sort your food queries by our newly saved nutrient attributes.

Using [ActiveRecord Relationships](https://api.rubyonrails.org/classes/ActiveRecord/Relation.html), the application uses a Meals model that allows a user to save a list of foods though a join model that allows for many meals to share the same food, and access each food's attributes in the saved meals. 
```
class Meal < ApplicationRecord
    belongs_to :user
    has_many :meal_foods
    has_many :foods, through: :meal_foods
		scope :published, -> { where(published: true) }
```

Our meals model also contains an attribute for publishing the listed foods for non-registered users to see, which can be accessed in our views via the above [scope method](https://api.rubyonrails.org/classes/ActiveRecord/Scoping/Named/ClassMethods.html).

This allows users to choose to keep their lists private, or listed publicly.  There is a Comments model that creates a join table between meals and users, that validates for registered users and allows them to post notes or comments on published meals, or their own meals.
```
class Comment < ApplicationRecord
    belongs_to :meal, counter_cache: true
    belongs_to :user 
    validates :content, presence: true, uniqueness: true, length: {maximum: 500}
end

```

I will attempt to deploy the app or change the database to a PostgreSQL server to avoid the initial database setup.
All suggestions (especially for improving the meal update functionality) and comments are greatly appreciated!  






