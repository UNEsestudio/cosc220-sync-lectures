## UML Use Case tutorial

Problem description
(from a UQ exam question in 2014, used with permission)

You are designing a system for an electronic recipe book. The system has the following requirements.

* Each recipe has a name, and includes a set of written instructions, and a set of ingredients. A recipe is stored within the account of the user who created it.

* Some ingredients are basic ingredients. Basic ingredients include the name of the ingredient, and can also specify a quantity and a unit of measure.

* Other ingredients can be references to other recipes, e.g. a recipe for bread dough, which can be included in other recipes. These ingredients do not specify a quantity or unit of measure.

* The system allows users to generate a shopping list of all basic ingredients required to prepare a recipe, including ingredients for preparing ingredients.

* Basic ingredients must be specified as gluten-free, or lactose-free. The system can check whether a recipe is gluten-free or lactose-free by checking whether the recipe contains any non-gluten-free or non-lactose-free ingredients, either directly or indirectly.

* When creating an account, the user provides their email address and a password. After this, an email is sent to the email address provided, with a link to either confirm or cancel the registration.

* Users can maintain a list of friends. When a user adds another user as a friend, they will receive an email whenever that friend adds a new recipe. 

* Unregistered users can view recipes, but not post recipes

* Internally, the system has editors, who can promote recipes (causing them to be highlighted on the product's website)

* The recipe book system also has a copyright interface -- sometimes users will post recipes that come from (copyrighted) cookbooks. The authors and publishers of those cookbooks can flag a recipe as in copyright breach, in which case it will be removed from view until an editor can examine the claim.

---

### Tasks

1. What kind of actors are there in the system? Draw the inheritance relationship between these actors.

2. Identify use cases from the description

3. Link the actors to their use cases

4. Do any use cases seem to *extend* or *include* other use cases?

5. For two use cases, write a short description of them

6. For one of your use cases (preferably an interesting one), sketch its main flow as a sequence diagram
