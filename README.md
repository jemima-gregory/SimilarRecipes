Author: Jemima Gregory

Date:    01 June 2023

### This project investigates common ingredients between recipes in the Food.com recipe and review dataset. 

The research question is: what is the difference in listed recipe ingredients between a given test recipe and all other recipes in the Food.com recipe and review dataset? The nearest neighbours algorithm, calculating the distance using cosine similarity, is used to determine which recipes have the most similarity in terms of ingredients. The intended result is two lists, presenting the recipes with the most ingredients in common with the test recipe, and the least ingredients in common with the test recipe. This result holds significance because it helps to recommend other recipes, based on a given recipe’s ingredient list.


The data is first loaded in from Google Drive, and imported into two RDDs, one for the recipe file and one for the interactions file. They are csv files and so they are processed using the csv reader.
The recipe rdd is first preprocessed, removing the unnecessary attributes and appropriately formatting the required attributes. The unique ingredients list is created for use in the function which converts a list of ingredients into a sparse vector. Despite using collect and list functions on the data at this point, the effect on the timing is minimal, and this was tested specifically in the timing testing at the end of the project. This function matches the ingredients in the recipe ingredient list with the index of the ingredient in the previously created unique ingredients list. It uses these indices to create a Pyspark Sparse Vector object. The function is used to convert the ingredient list of every recipe in the recipe rdd.
The test recipe is defined as the first recipe in the recipe rdd, as an example for testing the algorithm.
The key part of the project is calculating the recipes with the most and least ingredients in common with the given test recipe. The nearest neighbours algorithm uses the cosine similarity to determine the similarity between ingredient lists. The function to compute the cosine similarity between lists uses the values from within the sparse vector objects to return the cosine similarity value approximately between 0 and 1, where 0 indicates no similarity and 1 indicates complete similarity/equality. The recipes with the top 15 and bottom 15 values for cosine similarity are taken, and the recipes with similarity = 0 are ignored at this point as they don’t provide value to the results.
Then the average rating for each recipe in the returned lists is calculated. The interactions rdd is preprocessed to remove the unnecessary attributes and appropriate format the required attributes.  The averages are calculated by separately getting the sum and count for each recipe and joining the results together on the recipe id to divide one by the other.
Finally, the list of 15 recipes which have the most ingredients in common with the test recipe and the list of 15 recipes which have the least amount of ingredients in common with the test recipe are returned, ordered by the average recipe rating.


This project utilised parallel processing using google cloud servers.
