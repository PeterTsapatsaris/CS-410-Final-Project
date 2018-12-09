### CS 410 Final Project
### Final Project Submission for Cristian Jansenson and Peter Tsapatsaris

### 1)  Overview of Code Function

Our project is an implementation of aspect based sentiment analysis (“ABSA”). Product “aspects” are those product concepts in which an opinion is expressed in a customer review. For cellphones, as an example, aspects could include “camera”, “performance”, “screen”, or “battery”. ABSA, as a general procedure, involves associating document text with product aspects, and then conducting sentiment analysis on aspect-associated text.

Our software performs two primary tasks: 
1.	*Aspect Extraction:*  The code takes a corpus of customer reviews and breaks them into phrases associated with a list of user specified product aspect terms, where a “phrase” is defined as the aspect and neighboring terms within a sentence.  The output is a dataframe containing the phrase, related aspect, review id, and other information relating to the product.
2.	*Sentiment Analysis:*  Once aspects phrases are extracted, the code assigns a sentiment polarity to each phrase.  

The end result is a dataframe containing aspect phrases for each product and their respective sentiment scores. There are many use cases for this application, and we give three specific examples in the implementation: 
1.	*Study the strengths and weaknesses of a selected brand:*  Once the aspects for a given product are extracted and assigned a sentiment polarity, it is easy to pull all products for a specific brand and examine consumer sentiment towards their aspects. In our example, we analyze a corpus of cellphone reviews and create a series of histograms showing the distribution of sentiment for each aspect of the phones of a specific brand.
2.	*Sort brands by aspect rating:*  Using the mean sentiment reviews for each aspect by brand, we can identify which brands have the best rated aspects, e.g., which cellphone makers have the cameras best liked by consumers, or batteries consumers like the least. The same can be done at the product level.
3.	*Discovery of insightful reviews:*  By finding conflicting aspect sentiment scores with the numerical reviews assigned by customers can yield useful information. For example, we found many examples where consumers rated a cellphone a 1, but the sentiment scores were high. These reviews tended to identify a discrete technical problem with the phone that performed well otherwise. These cases would potentially be ripe for customer service interventions.  


### 2)  Software Implementation

Our Python implementation is contained in a Jupyter notebook, and our example implementation uses a corpus of Amazon cell phone reviews found here:   https://www.kaggle.com/nehasontakke/amazon-unlocked-mobilecsv. We chose the terms “battery”, “screen”, “camera”, “performance” for aspects on this data set. 

*Aspect Extraction Implementation:*  Aspect extraction was the most difficult problem to solve for this project. We did not find any libraries containing pre-existing implementations, so instead wrote our own implementation from scratch. We initially attempted a variety of topic mining approaches in order to automate the discovery of product aspects. Although this was certainly helpful in identifying aspect categories, the topic vectors were not aspect discrete and contained too many non-aspect words to prove useful as aspects themselves. We therefore manually chose the topics we were interested in.  

To implement the extraction, we relied on a few heuristics that we found to yield the best results. We parsed each customer review into sentences, and then searched each sentence for the aspect terms. If an aspect term existed in the sentence, we extracted a phrase consisting of the five words to the left and to the right of the aspect term, stopping if the beginning or ending of a sentence was reached. We then placed that phrase into a dataframe along with columns for the related aspect and other review information.  

*Sentiment Analysis:*  Rather than re-invent the wheel and develop our own sentiment analyzer, we opted to use one of the many publicly available analyzers. Our criteria for selection were (1) interpretability; and (2) public availability. We found that the VADER analyzer (standing for “Valence Aware Dictionary and sEntiment Reasoner”) satisfied these criteria. It is available under the open-source MIT license. VADER uses a lexicon of words pre-labeled with semantic polarity. It therefore does not require training, and performs very quickly. VADER generates an overall sentiment score ranging from -1 to 1. It also generates separate negative and positive sentiment scores, which range from 0 to 1. 

*Potential Future Improvements:*  Tinkering with the heuristics used by the aspect extractor may yield improvements to the function. For example, finding additional aspect terms through the discovery of syntagmatic and paradigmatically words may yield good results. 

### 3) Software Usage 

*Dependencies:*
-	Python 3:  python.org/download/releases/3.0
-	Jupyter Notebook is required to run the software: jupyter.org
-	Pandas: pandas.pydata.org
-	Numpy: numpy.org  
-	NLTK: nltk.org – NOTE please make sure that the entire NLTK library is installed! 
-	Matplotlib: matplotlib.org

The Jupyter notebook contains detailed instructions for how to run the software, and should be run in the same folder containing the data (the data is contained in the "Amazon_Unlocked_Mobile.rar" and must be unzipped before running) and “fromDFtoDF1.jpg”.  The implementation is very easily adaptable, but as written requires a CSV file of customer reviews containing the following columns:  "Product Name", "Brand Name", "Price", "Rating", "Reviews", "Review Votes." The data file is extracted into a pandas dataframe.

The user must select the aspects they wish to extract by setting the *relevant_aspects* variable. In our example we use: "battery", "screen", "camera", and "performance". Aspect extraction is performed by the *get_all_phrases_containing_tar_wrd* function, which takes as input the target aspect term, the sentence to be analyzed, and two integers representing how far in each direction from the aspect term defines the phrase. It returns phrases containing the aspect word with the length specified. The function is called repeatedly in the body of the program, which loops through the text of each review in the dataframe, parses them into sentences, and passes those sentences to *get_all_phrases_containing_tar_wrd*. The end result is a dataframe with the columns “brand”, “phrase”, rating”, “aspect” and “review_id”. Each phrase is in turn analyzed by VADER and assigned sentiment scores, which are appended as new columns.

We further implement three use cases:
1.	*Study the strengths of a selected brand:*  Using simple dataframe selection, we are able to easily isolate individual brands and their aspects. We further use the pandas’s *query* function to remove neutral comments, which tend to obscure the charts in our study. We use matplotlib’s *hist* function to plot histograms of the data for examination.
2.	*Sort brands by aspect rating:*  Using pandas’s *groupby*, *stack*, and *reset_index functions*, we are able to create a multilevel dataframe that displays each brand, their aspects, and their mean scores for each aspect.  Once flattened, we filter by a specific aspect and sort to retrieve the best and worst brands for the given aspect. 
3.	*Discovery of insightful reviews:*  Using pandas’s *groupby* function, we create a dataframe containing the average sentiment score and customer rating by product. We then select products with a rating of 1, but an average sentiment score greater than 0.8. 
These use cases are just examples. Once the aspects are extracted and assigned sentiment scores, there are a multitude of use cases. 

A video presentation demonstrating usage of the software is available at https://youtu.be/MzhlXeYCjgk 

### 4) Team member contributions

Both team members made substantial contributions to the project and deserve equal allotment of credit. Cristian did the majority of the coding for the aspect extraction loop in the code. He also found the sample data, and recorded the video walkthrough of the program. Peter formed the concept for the project, coded the example use cases, and wrote the documentation. Both team members reviewed all aspects of the project and provided edits and debugging. 

### Citations:
-	We referred to the following code when writing the text parsing algorithm:  simply-python.com/2014/03/14/saving-output-of-nltk-text-concordance
-	We referred to the following code relating to the use of  vader sentiment analyzer:  opensourceforu.com/2016/12/analysing-sentiments-nltk
