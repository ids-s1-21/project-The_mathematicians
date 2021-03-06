Students’ Beliefs about Mathematics
================
by The Mathematicians

## Summary

We conduct the project because we want to investigate students’ beliefs
about Mathematics. And in detail, we want to research into the following
questions:  
1. How are answers distributed between students in different schools &
genders?  
2. Are there any correlations between answers?  
3. How do the answers of students differ from the expected answers?

The data we used to analyse comes from an anonymous survey regarding
“Beliefs about Mathematics” which was completed by students of the
University of Edinburgh for the academic year 2019-2020. The survey has
32 questions (one of them is a filter question, so only 31 questions
actually investigate the beliefs of students). We mainly used three data
sets during the analysis. They are results of the survey, gender, and
the experts’ answers for each question. All these data sets are
anonymous and each student is given an anonymous ID.

Before we approach these questions, we tried two different methods of
assigning a numeric mark/score to categorical answers such as “Agree”
and “Disagree”. The methods are the following:  
1. Each student will get 1 mark for answering the question at the same
direction as the experts (e.g. “Agree” and “Strongly Agree” is at the
same direction as “Agree”) and 0 otherwise.  
2. For questions of which expected answers are “Agree”, we could give
marks for “Strongly Agree”, “Agree”, “Neutral”, “Disagree” and “Strongly
Disagree” as follows: 2, 1, 0, -1, -2. vice versa.

We used both methods in question 3 and it turned out that the first
method is better because as the answer from the experts comes in only
two variants (“agree” and ““disagree”), and we cannot know to what
degree they actually agree with a choice like the one that’s been given
to the students (So it doesn’t make sense that “Strongly Agree”
necessarily earns more marks than “Agree”).

For the third question, using the first method, our visualisation is
shown as followed.

![](README_files/figure-gfm/score-visualisation-1-1.png)<!-- -->

For the first question, our analysis shows that, first of all, the
average number of “correct answers” (answers that are in the same
direction as the experts’ answer) ranked by schools is shown as follows:
- School of Mathematics (22.7)  
- School of Physics and Astronomy (21.8)  
- School of Informatics (20.5)  
- School of Philosophy, Psychology and Language Science (19.8)  
- School of Economics (18.6)

Also, if we consider questions individually, we found that the rate of
correctness for each question follows a pattern that’s similar to all
schools. It means that although the overall average marks are different,
schools tend to do better/worse on similar questions.  
When it comes to gender, first of all, we found that the overall average
marks for males and females are very close (male: 21 vs. female: 20.9).
However, if we consider questions individually, we found that females
answered better in questions related to “Nature of the Answers” (Whether
answers to mathematical questions are just numbers or reveals deeper
concepts) while males answered better in questions related to
“Persistence in Problem Solving”.

For the second question, first of all, we put questions into different
categories based on the research by Code et al. (See References). The
categories and their corresponding questions are listed below:  
- Persistence in Problem Solving: Questions No.8, 10, 24, 29  
- Growth Mindset: Question No.5, 6, 22, 31  
- Interest in Mathematics: Question No.12, 26, 32  
- Relationship between Mathematics and Real World: Question No.13, 15,
21  
- Sense Making: Question No.3, 4, 11, 18, 23  
- Nature of the Answers: Question No.2, 7, 9, 16, 28, 30  
- No category: Question No.25, 27

After that, we tested the linear correlation of each pair of the
categories using Pearson correlation coefficient. However, there are no
pair of categories that can get a coefficient greater than 0.4. So it
means that these categories don’t have significant linear
correlations.  
As a result, we then tried to fit a logistic regression model to each
pair of the categories. In order to apply logistic regression, we
decided that for each category, we will calculate a “critical” number
such that when this category is a dependent variable, the total mark
students obtained from questions in this category will be converted to 0
if their raw total marks are lower than the number, otherwise it will be
1.

Then, we fit a logistic regression model for each pair of these
categories and calculated their AUCs to assess how well these models
perform. The results show that the model relating “Interest” (dependent)
and “Confidence” (independent) has the highest AUC (0.8321314), meaning
it’s doing the best. The model generally shows that if a student answers
more questions in the category “Confidence” correctly, there is a higher
possibility for him to answer more than 1/3 questions in the category
“Interest” correctly.

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

Data provided by Dr. George Kinnear

## References

Code, W., Merchant, S., Maciejewski, W., Thomas, M., & Lo, J. (2016).
The Mathematics Attitudes and Perceptions Survey: an instrument to
assess expert-like views and dispositions among undergraduate
mathematics students. International Journal of Mathematical Education in
Science and Technology, 47(6), 917–937.
<https://doi.org/10.1080/0020739X.2015.1133854>

Liddell, T. M., & Kruschke, J. K. (2018). Analyzing ordinal data with
metric models: What could possibly go wrong? Journal of Experimental
Social Psychology, 79, 328–348.
<https://doi.org/10.1016/J.JESP.2018.08.009>
