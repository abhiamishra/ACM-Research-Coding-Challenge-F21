# ACM Research Coding Challenge (Fall 2021)

## [](https://github.com/ACM-Research/Coding-Challenge-F21#no-collaboration-policy)No Collaboration Policy

**You may not collaborate with anyone on this challenge.**  You  _are_  allowed to use Internet documentation. If you  _do_  use existing code (either from Github, Stack Overflow, or other sources),  **please cite your sources in the README**.

## [](https://github.com/ACM-Research/Coding-Challenge-F21#submission-procedure)Submission Procedure

Please follow the below instructions on how to submit your answers.

1.  Create a  **public**  fork of this repo and name it  `ACM-Research-Coding-Challenge-F21`. To fork this repo, click the button on the top right and click the "Fork" button.

2.  Clone the fork of the repo to your computer using  `git clone [the URL of your clone]`. You may need to install Git for this (Google it).

3.  Complete the Challenge based on the instructions below.

4.  Submit your solution by filling out this [form](https://acmutd.typeform.com/to/zF1IcBGR).

## Assessment Criteria 

Submissions will be evaluated holistically and based on a combination of effort, validity of approach, analysis, adherence to the prompt, use of outside resources (encouraged), promptness of your submission, and other factors. Your approach and explanation (detailed below) is the most weighted criteria, and partial solutions are accepted. 

## [](https://github.com/ACM-Research/Coding-Challenge-S21#question-one)Question One

[Sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis) is a natural language processing technique that computes a sentiment score for a body of text. This sentiment score can quantify how positive, negative, or neutral the text is. The following dataset in  `input.txt`  contains a relatively large body of text.

**Determine an overall sentiment score of the text in this file, explain what this score means, and contrast this score with what you expected.**  If your solution also provides different metrics about the text (magnitude, individual sentence score, etc.), feel free to add it to your explanation.   

**You may use any programming language you feel most comfortable. We recommend Python because it is the easiest to implement. You're allowed to use any library/API you want to implement this**, just document which ones you used in this README file. Try to complete this as soon as possible as submissions are evaluated on a rolling basis.

Regardless if you can or cannot answer the question, provide a short explanation of how you got your solution or how you think it can be solved in your README.md file. However, we highly recommend giving the challenge a try, you just might learn something new!


----------------------------------------------------------------------------------------------------------------
## Explanation of Submission
Sources used:

-[VADER Sentiment Analysis](https://github.com/cjhutto/vaderSentiment#introduction)

-[Large Movie Review Dataset](https://ai.stanford.edu/~amaas/data/sentiment/)

-[NLTK](https://www.nltk.org/)

-[SciKit](https://scikit-learn.org/stable/modules/classes.html)


In the solution, I implemented two different implentations of sentiment analysis. 

## VADER implementation 
The first implementation was with the VADER library in python. VADER is a sentiment analyis package
that utilizes previously collected data of lexicons and their sentiments to generate scores for new
words and inputs. This can be thought as a dictionary that contains many words and their sentiments. The interesting aspect about this dictionary is that it has certain rules for context that have been coded in by researchers. As such, these rules allow it to pick sentiment from grammatical order and other structures surrounding text. Putting in input means accessing this dictionary to obtain certain sentiments and aggregating them into a score - called the compound score. 

As per the VADER guidelines,
the scientific literature surrounding NLP has determined that compound scores above 0.05 are termed as positive,
compound scores below -0.05 are termed as negative, and scores between -0.05 and 0.05 as neutral. Using
this rule-based library - it's purely looking at words and their strict significance and also 
implementing context - I was able to obtain the compound scores for each sentence as well as the 
proportions of positive/negative/neutral sentiments in each sentence.

Finding the mean compound score gave us a overall score - 0.118. This score is above 0.05 and as such, can be
translated as the overall text has a overall positive sentiment. Making a graph of how the sentiment changes per
sentence gives us information that throughout the text, the score varies. The first pargraph is especially negative while the other two paragraphs are positive. This matches up with how I read the paragraph. The first paragraph has a lot of strong, negative words such as "furious", "yelled", "Power" and has exclamation points 
which, in that context, have a negative connotation. As the text progresses, the tone changes to a more general/neutral tone. Since this is a rule-based approach, the presence of positive words such as "excellent" and "chuckled" mean that it ends up categorizing them as "positive". Looking at the ratios instead of the compound score, we see that as the paragraph progresses, there are high neutral proportions which matches up the tone of the second and third paragraph.

## Random Forest Classifier implementation
 was with using Random Forest Classification. I sought to see sentiment as a two distinct classes, 1 (positive) and -1 (negative). As such, now the task became classifying and random forest classifiers are a good model to apply for such classification. 

The problem I had was a lack of training data. I thought maybe to construct a small training dataset with fiction books since the input was fiction and had a lot of complex words and peculiar English words such as "bro't". However, that would take time and would not be efficient. The closest thing to match the input's style of writing, that was easy accessible, would be movie reviews. Normally, movie reviews, on IMDb, are discussing artistic concepts and plots and as such, I figured the vocabulary would be somewhat close.

I used the Large Movie Review Dataset that contained training and testing dataset labellings for many reviews.
Using those reviews, I implemented a preprocessing function that stripped away the review from unnecessary words such as "am" and "and". My approach was similar to only have the most important words that conveyed the most meaning and use those to estimate the sentiment. After that, I passed the words through a Bag Of Words approach to really only consider the words. Here, I used CountVectorizer to obtain the counts of the words in the training dataset and to build of a "Bag of Words". With that, I build a Random Forest Classifier with 150 trees and applied the dataset onto the input.txt

I obtained the binary class predictions along with the probability for being positive or negative for each sentence. Taking the mean of class predictions, I got a overrall score of 0.962 which estimates the overall sentiment to be overwhelmingly positive. However, the reason it's predicting so positively is because of a few factors. The biggest factor is the vocabulary in the training is somewhat similar but is not a good representation of the vocabulary used in the input. As such, key words that would have directed the sentiment were missed by the model. Secondly, a loss of context, as we're just looking at the words, contributed to over-estimation. The complex sentences and wording mean that when the words are taken by themselves, the sentiment formed is not as accurate as we would desire.

Looking at mean of probability of being positive, we get a score of 0.751. Assuming that 50% is neutral, we can see how this is a better representation of the sentiment score. Just like in the VADER implementation, we got a overall positive score that was nearer to the neutral score. Seeing the dispersion of the overall positive score, we got more variations however it is still not as accurate in finding negative sentiments. As such, compared to the overall paragraph, I think this metric does a better job of predicting the sentiment however it falls short of picking up negative sentiments - especially seen in the first paragraph. 