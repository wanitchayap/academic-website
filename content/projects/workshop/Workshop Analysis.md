---
title: "Analysis of 2019 Fall Quarter UChicago Computational Social Science Workshop Questions"
categories:
 - Side Projects
---

The [Computational Social Science Workshop]( https://macss.uchicago.edu/content/computation-workshop ) at UChicago has a very unique way of gathering questions for the speaker. Prior to the workshop, students enrolled in [Masters in Computational Social Science Program]( https://macss.uchicago.edu/ ) read some materials (usually the research article the presentation is based on) related to the topic of the workshop, and ask questions about the materials on the issue page of a [Github repository]( https://github.com/uchicago-computation-workshop ). Afterward, students later read through the questions posted on the issue page, and upvote ("thumbs up") the question they like. After the workshop presentation, the question(s) with most upvotes gets asked the speaker. This method is to ensure that the "best" questions are asked and answered in the workshops.

# But Are We Picking the Best Questions?

On the first workshop this quarter, or the [Fall Welcome Mixer]( https://github.com/uchicago-computation-workshop/fall2019mixer ), a student (Bhargav) suggested that people tended to vote the questions that were asked first, not the questions with the best quality. (From now on, I will call this as the _primacy effect_, a term I borrowed from memory science. This is not an accurate use of the term - if you are curious about this topic, [Wikipedia page]( https://en.wikipedia.org/wiki/Serial-position_effect#Primacy_effect ) might be a good place to start.) On that workshop, we discussed how we can abate this _primacy effect_ and agreed on implementing two solutions. One solution was to put all questions into a single issue thread instead of putting each question into separate issue pages. This was intended to decrease the amount of work students have to undergo in order to read all the questions. Another solution was to make everybody upvote questions after the question submission deadline to prevent people from not reading questions posted relatively late. This blog post addresses if these two solutions were enough to prevent this primacy effect.

# Main Results: Primacy Effect Persists

One simple way to measure if the questions that were posted earlier tend to get more upvotes is to see the pattern between the time the question was posted relative to the deadline (`relative time`) and the number of upvotes (`upvotes`). I fitted a linear regression model using `relative time` as the predictor variable and `upvotes` as the dependent variable. The result indicates that the `relative time` is a significant predictor variable for `upvotes` (__Table 1__ and __Figure 1__; f(1,401) = 926.5, p = 2.74e-106, R2: .698, Adjusted R2: .697).

|     Table 1     | coefficient | std err  |    t     | p > \|t\| |   95% interval   |
| :-------------: | :---------: | :------: | :------: | :-------: | :--------------: |
|    intercept    |    .4707    |   .021   |  21.901  |   .000    |   (.428, .513)   |
| `relative time` |   - .0005   | 2.63e-05 | - 18.617 |   .000    | (- .001, - .000) |


![Figure 1](/img/workshop_figures/Figure1.png)

I also tried using the first 5 workshop data as the training data to fit the regression, and use the last workshop data as the test data. The result is shown in __Figure 2__ (`upvotes` = 1.732 - 0.018 * `relative time`, R2: .561).

![Figure 2](/img//workshop_figures/Figure2.png)

Another way to see the problem is to see the relation between the position of each question in the thread (`position`) and `upvotes`. Analogous to what I did with `relative time`, I fitted a linear regression model using `position` as a predictor variable and  `upvotes` as the dependent variable. Indeed, `position` was another significant predictor variable (__Table 2__ and __Figure 3__, f(1, 401) = 310.0, p = 8.12e-52, R2: .436, Adjusted R2: .435).

|  Table 2   | coefficient | std err |   t    | p > \|t\| |   95% interval   |
| :--------: | :---------: | :-----: | :----: | :-------: | :--------------: |
| intercept  |   22.1502   |  .905   | 24.479 |   .000    | (20.371, 23.929) |
| `position` |   - .3913   |  .022   | 17.607 |   .000    | (- .435, - .348) |

![Figure 3](/img//workshop_figures/Figure3.png)

__Figure 4__ shows the result if we use the first 5 workshop data as the training data and the last workshop as the test data (`upvotes` = 22.150 - 0.391 * `position`, R2: .436)

![Figure 4](/img/workshop_figures/Figure4.png)

Ignoring the obvious multicollinearity of two predictor variables, if we fit the regression model using both `relative time` and `position` as the predictor variables and `upvotes` as the dependent variable __for fun__, the R2 becomes as high as .726 (__Table 3__, f(2,400) = 528.8, p = 4.88e-113, adjusted R2 = .724 ).

|     Table 3     | coefficient | std err |   t    | p > \|t\| |   95% interval   |
| :-------------: | :---------: | :-----: | :----: | :-------: | :--------------: |
|    intercept    |   7.3112    |  .960   | 7.618  |   .000    |  (5.424, 9.198)  |
| `relative time` |   - .0126   |  .001   | 20.544 |   .000    | (- .014, - .011) |
|   `position`    |   - .1278   |  .020   | 6.346  |   .000    | (- .167, - .088) |

To conclude, I think the results indicate that __the primacy effect persists even though we had applied some solution to address the problem__. However, I want to note that many people I talked about this issue suggested that better questions are actually posted early. My explanation for this, if it is true, is that people who are more interested in the topic will start reading the materials early, and post questions early. (And questions asked by students who are interested in the topic are likely to be "better")

# Other Results

## Does longer questions get more upvotes?

One of the simplest and crudest way to measure if a student put a lot of effort into a question will be to look at the length of the question. Building on this, I was curious if the length of the question (`question length`) could be a predictor variable. (I note that a friend suggested seeing this relation - Aabir) I fitted a linear regression model using `question length` as the predictor variable and  `upvotes` as the dependent variable. `question length` was another significant predictor variable for `upvotes`, although it was not as strong as the variables related to the primacy effect (__Table 4__ and __Figure 5__, f(1, 401) = 86.32, p = 9.88e-19, R2: .177, adjusted R2: .175).

|      Table 4      | coefficient | std err |   t   | p > \|t\| |  95% interval   |
| :---------------: | :---------: | :-----: | :---: | :-------: | :-------------: |
|     intercept     |   - .1857   |  1.084  | .171  |   .000    | (-2.316, 1.944) |
| `question length` |    .0121    |  .001   | 9.291 |   .000    |  ( .010, .015)  |

![Figure 5](/img/workshop_figures/Figure5.png)

Like the previous analysis, __Figure 6__ shows the result if we use the first 5 workshop data as the training data and the last workshop as the test data (y = - 0.186 + 0.012 * x, R2: .188).

![Figure 6](/img/workshop_figures/Figure6.png)

Finally, we can incorporate this result with the results regarding the primacy effect by trying to fit the data using `relative time` (which showed a better fit than position) and `question length` as predictor variables. The R2 value and adjusted R2 value goes as high as .764 and .763 if we do this. (__Table 5__, f(2, 400) = 647.5, p = 3.79e-126) I confess that the reason behind the lack of a figure is because I have no idea how to visualize regression with multiple variables.

|      Table 5      | coefficient | std err |   t    | p > \|t\| |   95% interval    |
| :---------------: | :---------: | :-----: | :----: | :-------: | :---------------: |
|     intercept     |  - 3.2243   |  .589a  | 5.475  |   .000    | (- 4.382, -2.067) |
|  `relative time`  |   - .0141   |  .000   | 31.540 |   .000    | (- .015, - .013)  |
| `question length` |    .0075    |  .001   | 10.583 |   .000    |   (.006,  .009)   |

Again, I tried fitting a multivariable linear regression model using the first 5 workshops as train data and tested on the last workshop data. It showed a good prediction of R2 = .665 (`upvotes` = - 3.224 - 0.014 * `relative time` + 0.008 * `question length`).

## Does the first-year students post earlier than the second-year students?

Completely unrelated to the main question, I was curious if there is any significant difference between the first-year students and the second-year students. (I had the data already processed!) As __Figure 7__ shows, first-year students posted their questions significantly more early than the second-year students (t(401) = 2.56, p = .0109).

![Figure 7](/img/workshop_figures/Figure7.png)

I suspected that this might be caused by some outliers in the first-year group, so I examined the mean relative time for each unique ID and found out that there are indeed two very distinct outliers in the first year group (__Figure 8__).

![Figure 8](/img/workshop_figures/Figure8.png)

If we remove these two people from the dataset (10 questions), the difference between the two groups becomes insignificant(or marginally significant; __Figure 9__, t(391) = 1.83, p = .0680)

![Figure 9](/img/workshop_figures/Figure9.png)

Although the first-year students tend to post questions earlier than the second-year students, they did not get significantly more upvotes (__Figure 10__, t(401) = 1.43, p = .153).

![Figure 10](/img/workshop_figures/Figure10.png)

## When did we post the questions?

After showing these results to my friends, A friend of mine (Aabir again) said that "Also might be cool to see the distribution of times before deadline that posts are made." __Figure 11__ and __Figure 12__ is based on his suggestion. Specifically, __Figure 12__ only shows the questions that were posted within 20 hours of the deadline and includes 363 questions out of 403 total.

![Figure 11](/img/workshop_figures/Figure11.png)

![Figure 12](/img/workshop_figures/Figure12.png)

## How did the top upvote scorers do on other variables?

Finally, these are the "stats" of the 5 students who had the most upvotes on average for the 6 workshops. (I did not include the name of the students just in case - but I note that the second is me.)

| Number of Upvotes |    Relative time    |    Position     |  Question Length  |    Year     |
| :---------------: | :-----------------: | :-------------: | :---------------: | :---------: |
|  39.00 (__1st__)  | - 2136.33 (__1st__) | 2.833 (__1st__) |   1084.83 (6th)   | First year  |
|  33.83 (__2nd__)  | - 1315.00 (__3rd__) | 10.50 (__3rd__) | 1794.17 (__2nd__) | First year  |
|  29.83 (__3rd__)  |   - 841.67 (10th)   |   17.17 (7th)   | 1424.50 (__4th__) | Second year |
|  29.75 (__4th__)  | - 2016.75 (__2nd__) | 9.75 (__2nd__)  |   710.50 (32rd)   | First year  |
|  24.50 (__5th__)  | - 1231.50 (__4th__) |   16.83 (6th)   |   896.50 (16th)   | First year  |



# Methods

Unfortunately, all the applications for scrapping Github, including the [Github API]( https://developer.github.com/v3/ ), did not scrap the number of upvotes each comment had. So I downloaded the source code files of 6 workshop issue pages in Github and processed each comment using [Beautiful Soup library]( https://www.crummy.com/software/BeautifulSoup/bs4/doc/ ). I extracted 6 primary variables for each question:
* `name`: the username of the comment
* `time`: the time the question was posted
* `position` (position at the thread): the position of the comment in the thread (for example, if the question was posted fourth, this variable becomes 4)
* `upvotes` (number of upvotes): number of upvotes the question received
* `question length`: the text length of the question. I note that the text here includes the html formatting, such as <a href: "www.example.com">, so if somebody asked a question using a lot of markdown flavors, this could be a confounding factor.
* `workshop date`: the date of the workshop

For analysis, I added 2 additional variables:
* `relative time` (relative time to the deadline in minutes): defined as the time difference between the time the question was posted and the deadline for uploading questions. Calculated as (time) - (deadline time), utilizing that variable time was saved as a datetime object in python.
* `year`: whether the question was asked by a first-year student or a second-year student. Each user names were classified as first-year  (n=244, 50 unique names) , second-year (n=132, 27 unique names) , or unidentifiable (n=27, 6 unique names) using the following criterion:
  * first-year: comparing the name registered as Github with the list of students I had, and also using human heuristics to figure out the name from the username
  * second-year: if they had a forked repository from a [2018 MACSS course](https://github.com/UC-MACSS/persp-analysis_A18).
  * unidentifiable: if they fitted to none of the criteria above.

Questions that were posted later than the deadline was excluded from analysis, but I note that including them does **not** alter the main results of this analysis. Also, posts from the faculties and preceptors were also excluded, since this was intended to see the students' patterns.

All analysis was done using python. The data was handled using the [pandas library](https://pandas.pydata.org/). The regression analysis was done using [statsmodels library]( http://www.statsmodels.org/devel/index.html) (for using entire data) and the [scikit-learn library](https://scikit-learn.org/stable/) (for using training the model with train data and applying them with test data). The t-test was done using [scipy.stats library]( https://docs.scipy.org/doc/scipy/reference/stats.html ), and the visualization was done using the [seaborn  library]( https://seaborn.pydata.org/ ).
