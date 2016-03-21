# Revolutionize it! Kick the stale. Make mobile shopping a real fun experience!

<p align="center">
    <img alt="Lidl Logo" src="http://www.lidl.com/images/logo.svg" width="250px" />
</p>

## Challenge Description
Let us together disrupt the mobile shopping exerperience.

The challenge can be taken by various angles. Which one is your choice?

- **Design**: how to stand out from profane shopping and create a WOW experience?
- **UX**: lacklustre clicking or your GREAT IDEA???
- **BI / Business Acumen**: How to create shopping-patterns (incl. shelf and product allocation that just provide FUN)?
- **Algorithms**: Intelligently combine searched products, proposals, assortment (shelf / product) allocation and maybe more??
- **App Coding**: How to code that app?

### Goals

- Create a fun mobile (grocery) shopping experience by means you choose
- Meaning: there are various angles you can tackle the challenge!!
- More users should love to shop mobile
- More users should love to shop mobile for a longer time and discover more products they find interesting


## What Lidl provides:
**Important**: depending on your method of solving the challenge you may only need one or even none oft he material we provide
-	Product database (several json files which represents a list of products)
- E-commerce shopping basket data (d.h. für den Bereich Food)

### Dataset Description

Lidl will provide two datasets as mentioned above. A detailled description can be found here

#### Product Database
| Field                       	| Datatype                   	| Description                                                                                 	|
|-----------------------------	|----------------------------	|---------------------------------------------------------------------------------------------	|
| productId                   	| Long                       	| The unique identifier of a product                                                          	|
| maxOrderQuantitiy           	| Int                        	| The max amount which can be ordered                                                         	|
| Price                       	| Float                      	| The price of the product                                                                    	|
| oldPrice                    	| Float                      	| The old price of the product                                                                	|
| daysForDelivery             	| Int                        	| Days until the product is delivered after it was ordered                                    	|
| availableOnline             	| Boolean                    	| Product can be ordered online                                                               	|
| availableInStore            	| Boolean                    	| Product can be bought only in the store                                                     	|
| Image [Object]              	| All attributes are strings 	| The product image which provide different sizes of the same image                           	|
| ProductLanguageSet [Object] 	| All attributes are strings 	| Contains different string attributes like title, subtitle, priceText and discountText       	|
| Stock                       	| Int                        	| The available amount of the product. If the stock is “0” the product can’t ordered anymore. 	|
| Rating.ratingCount          	| Int                        	| How often the products was rated                                                            	|
| Rating.ratingStarts         	| Float                      	| The average rating of the product                                                           	|

#### Shopping Basket Data
TBA

## Contacts
- Arthur Jonas (Teamleader Lidl Mobile)
- Philipp Götting (Head of Lidl Digital)


