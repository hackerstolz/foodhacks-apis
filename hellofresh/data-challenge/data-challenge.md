# HelloFresh Challenge - Data

## General Dataset Description
- HelloFresh provides you with a list of ingredient names and the ISO 3166-1 alpha-2 country code from the HelloFresh Kitchen that ships it.
- You can find this ingredient list in `./ingredients.csv`.

## Challenge Description
- The goal for this challenge is to categorize and to enrich each ingredient with more information by means of a computer program. Ingredients are easily categorizable using a human eye, but not so easily categorizable using machines. For example, we as humans know that "Sirloin Steak" is "beef", but a machine does not.

- From a list of around 4,500 ingredients, your algorithm should be able to successfully extract, for instance, all "beef" ingredients. You might want to integrate information from external data sources by matching the HelloFresh ingredient names to a particular category.

- Publicly available datasets are your friend! For instance, we can suggest:
  - Wikipedia Infoboxes of ingredients (e.g. [Brocolli](https://en.wikipedia.org/wiki/Broccoli)).
  - [USDA National Nutrient Database](https://ndb.nal.usda.gov/ndb/foods?qlookup=&new=1).

- Wikipedia Infoboxes can be accessed by querying DBpedia SPARQL endpoint. For instance, retrieving a [list of all the ingredients in Wikipedia](http://dbpedia.org/snorql/?query=SELECT+DISTINCT+%3Fingredient_name%0D%0AWHERE+%7B+%3Ffood_recipe+%3Chttp%3A%2F%2Fdbpedia.org%2Fontology%2Fingredient%3E+%3Fingredient_name+%7D%0D%0AORDER+BY+%3Fingredient_name). For each ingredient we can then retrieve all its properties, for instance, [all the properties of Brocolli](http://dbpedia.org/snorql/?describe=http%3A//dbpedia.org/resource/Broccoli). The USDA National Nutrient Database is [free available for download](http://www.ars.usda.gov/Services/docs.htm?docid=24912)

- You may try and categorize ingredients in a taxonomy of categories with different levels, for instance:

  - Level 1: Main Category (e.g.: meat, dairy, vegetable, fruit, carbohydrate, etc.)

  - Level 2: Sub-Category (e.g.: beef, pork, chicken; milk, cheese, yoghurt; tomatoes, carrots, beans; orange, apple, pear; potatoes, rice, pasta, etc.), there should be no more than 100 sub-categories.

  - Level 3: Unique Name (e.g.: sirloin steak; gouda cheese; heirloom tomato; mandarin orange; linguine) and nutritional values associated to each unique ingredient (e.g.: energy, carbohydrates, fat, protein, vitamins, minerals)

- Another possibility is to categorize by cuisines where the ingredients can traditionally be found (e.g.: mediterranean, vegan, vegetarian, arabic, asian, etc.).

- These are simple suggestions, feel free to come up with any other categorization schemas, datasets to use, information to associate to each ingredient, or any other you ideas you might have! 

Have fun!
