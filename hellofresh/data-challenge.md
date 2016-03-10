#HelloFresh Challenge - Data

##General Dataset Description
- HelloFresh will provide you with a list of ingredient names and the language they're written in. 

- Publicly available datasets are your friend! (NLTK, WordNet, WikiPedia)

##Challenge Description
- The goal for this challenge is to extract more information from ingredients of recipes. 
- We have thousands of ingredients that are easily categorizable using a human eye, but not so easily categorizable using machines. For example, we as humans know that "Sirloin Steak" is "beef", but a machine does not. 

- From a list of 10,000 categories, the algorithm you build should be able to successfully extract all "beef" ingredients. 

- You might want to integrate WikiPedia or (introduce other source of info here) to be able to match the HelloFresh ingredient names to a particular category.

- You may try and categorize ingredients in  broader categories first, and then proceed to sub-categorize. For example:
 Level 1) Protein, Dairy, Vegetable, Fruit, Carb 
 Level 2) sub-category (beef, pork, chicken; milk, cheese, yoghurt; tomatoes, carrots, beans; orange, apple, pear; potatoes, rice, pasta)
 Level 3) detailed name (sirloin steak; gouda cheese; heirloom tomato; mandarin orange; linguine)

- An additional approach could be to categorize each ingredient by cuisine or cuisines where they can traditionally be found.

- There should be no more than 100 sub-categories. 