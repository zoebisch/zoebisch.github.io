---
layout: post
title:  "Sinatra Portfolio Project - "The Recipator""
date:   2017-08-30 01:16:17 -0400
---


Ok, ok, yes it's a corny name.  But it was the first thing that popped into my head!  Once again, I overdid it. But it wasn't intentional, really it wasn't! I just like to make the best use of my time and like to program things that I will come back and use later.

So, since I have an actual Culinary background and have been both informally Classically trained (Haute) and helped co-own and run a successful sandwich shop in Seattle (for as long as it lasted), I decided to work with recipes, ingredients, cultures...and later adding users into the mix, created a recipe database CRUD project.  

I was trying to wrap my head around what relationships I needed and only really had a few sticking points. One for the life of me, I can't seem to understand why the migration just wouldn't take for the following:

```
class Recipe < ActiveRecord::Base
  include Concerns::InstanceMethods
  extend Concerns::ClassMethods
  belongs_to :author
  belongs_to :culture #(Most have only one, for purist reasons we will decide on ONE)
  has_many :recipe_ingredients, :class_name => 'Recipe_Ingredients'
  has_many :ingredients, through: :recipe_ingredients
  has_many :user_recipes, :class_name => 'User_Recipes'
  has_many :users, through: :user_recipes
end
```

You see that ":class_name => 'Recipe_Ingredients'?  Same thing with User_Recipes.  I was going nuts trying to associate Recipes with Ingredients and I kept getting a "has_many :through uninitialized constant" error, and specifically calling out the class forced it to work. On a suggestion from a stackexchange post, I changed RecipeIngredients to Recipe_Ingredients, but that didn't work either.  Didn't make me too happy, because it *should* have worked without stressing, but here's the rest of it:

```
class Recipe_Ingredients < ActiveRecord::Base
  belongs_to :recipe
  belongs_to :ingredient
end
```

```
class Ingredient < ActiveRecord::Base
    include Concerns::InstanceMethods
    extend Concerns::ClassMethods
    has_many :recipe_ingredients, :class_name => 'Recipe_Ingredients' #ActiveRecord was refusing to initialize constant?
    has_many :recipes, through: :recipe_ingredients
end

```

So the long and short of it is maybe there is something I am missing, if anyone knows feel free to reach out.  

At any rate this was a really fun project in the end, and although it took me about 2.5 full days of work, it was worth it. I learned a lot about these associations and even more importantly, learned about some design choices.  

For instance, since this is designed to be a more localized version rather than some massive database, I decided not to allow users to kill off Authors, Recipes or Cultures.  In fact, only an authenticated admin can do that, and only under certain conditions. For instance, the admin can't kill off an Author if they have recipes, and can't delete a Culture if it has recipes. Ingredients cannot be removed unless they are not being used in a recipe.  In this sense, the recipe is lock-step with the ingredients and while ingredients can exist without a recipe, a recipe cannot exist without ingredients.

In the end, I have something that I have always wanted to code, and I truly mean that.  With as many custom recipes as I have created and as many ingredients that I have in my cupboard (likely around serveral hundred herbs, spices, salts, and staples, all of which I use on a regular basis) I can easily see myself ordering a barcode scanner and making my own system. I am not joking. :D

Hope this finds you well, dear reader!

Until next time,

Cheers!
