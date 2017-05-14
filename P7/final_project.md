# P7 - A/B Testing

## Experiment Overview

> At the time of this experiment, Udacity courses currently have two options on the home page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

> In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.
The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

> The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## Metric Choice

>List which metrics you will use as invariant metrics and evaluation metrics here. (These should be the same metrics you chose in > the "Choosing Invariant Metrics" and "Choosing Evaluation Metrics" quizzes.)

>For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment.

##### 1. Number of cookies ( Invariant )
Cookies are information kept by browsers for every site. Cookies as assigned when a user visit a site (URL) for the first time and persist within each browser until it expires or be cleared. So the number of unique cookies is mostly driven by the number of users and the number of different browsers people use to visit the site. Our experiment of showing the question should have no effect on the number of users or their browsing behaviour. Hence, we expect the number of cookies to be invariant in our experiment.

##### 2. Number of user-ids
This experiment is likely to results less user to enroll in the free trail. So it can't be used as an invariant metrics for validation.

It is also a poor choice for evaluation metric. To illustrate this, suppose in our experiment, 100 unique cookies are assigned to the control group and 120 to the experiment. Then, even if there are significantly more unique-id enrolled in our experiment group than the control, we can't conclude that the experiment treatment is effective. The observed difference in `user-ids` could simply because there were more traffic diverged to the experiment group.

In order to draw meaningful conclusions, the number of user-ids should be "normalized" by divided with the unique number of clicks. And this is in fact the gross conversion discussed below.

##### 3. Number of clicks ( Invariant )
The question is shown after users have clicked the "Start free trail" button. Hence, we don't expect any difference in the number of clicks between the two groups. Therefore, it can be used as an invariant metrics for validation.

##### 4. Click-through-probability ( Invariant )
The `Click-through-probability` is the ratio of `number of clicks` and `number of cookies`. As both `number of cookies` and `number of clicks` are expected to be invariant, we will expect `click-through-probability` to be invariant too.

##### 5. Gross conversion (Evaluation)
If the experiment is effective, we expect to see the `gross conversion` to decrease. That is we expect fewer proportion of student will complete the enrollment process. To launch the experiment, we requires the gross conversion to be both statistically and practically significantly less in the experiment group.

##### 6. Retention (Evaluation*)
If the experiment is effective, we would expect the experiment group to have a higher retention as the experiment. In order for us to launch the experiment, we require the `retention` in our experiment group to be both statistically and practically greater than the one in our control group.
(As it turns out, this metrics required too long to complete for the given alpha and beta, and hence not used in the final evaluation )

##### 7. Net conversion (Evaluation)
`Net conversion` is the ratio of the `number of user-ids remind enrolled pass the trail` over the `number of clicks`. Because our hypothesis states that the experiment would not "significantly reduce the number of students to continue past the free trial", and as discussed earlier that we expect the number of clicks to be invariant, it follows that we expect the `net conversion` to doesn't change significantly. Hence, in order for us to launch the experiment, we require the difference of `net conversion` between the two groups to be small, both statistically and practically.

### Measuring Standard Deviation
>List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)

>For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the empirical variability, or whether you expect them to be different (in which case it might be worth doing an empirical estimate if there is time). Briefly, give your reasoning in each case.

| Metrics | Standard Deviation | Comparable to empirical ? |
| --- | --- |
| Gross conversion | 0.0202 | Yes |
| Retention | 0.0549 | No |
| Net conversion | 0.0156 | Yes |

The analytical estimate tends to be comparable to the empirically estimates when the unit of diversion and
the unit of analysis are the same.

Hence, I expect the estimate for gross conversion and net Conversion to be comparable to empirical estimate. Similarly, the empirical and analytical estimate for retention are not likely to be comparable, because the unit of analysis ( number of unique user id) is different to the unit of diversion.

### Sizing
#### Number of Samples vs. Power

> Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.)

No. I will not use Bonferroni correction duration my analysis. Because the evaluation metrics is highly correlated, using Bonferroni correction, in this case, seems to be too conservative.

Using alpha = 0.05 and beta = 0.2, a total of 685,275 page views is required to conduct this experiment.

#### Duration vs. Exposure
> Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)

> Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

**Risks**
The experiment is not a risky one. It does not expose the users to risk that exceeds the "minimal risk" level. No sensitive information is involved in the experiment. Asking an extra question about time available has hard any physical, psychological, emotional, social and economic effects on the users.

**Exposure and Duration**
Even for experiments with minimal risk, it is always a good practice to expose the experiment to a smaller subset of traffic whenever possible. This is because no matter how careful you are, there are always some risk involved. For example, there could be a bad implementation problem that causes user unable to complete the enrollment. Running experiment on 100% of the traffic also prevents other experiment being run at the same time.

On the other side, the trade-off to exposing only part of the users to experiments is longer experiments
duration. Running longer experiments might be more costly (ie. idle resources waiting for experiment result). One might also argue, a shorter duration is preferred in an "Agile" environment. As it allows for shorter feedback cycle.

The decision on the duration and exposure trade-off is ultimately subjective and depends on external factors other than the experiment itself. Personally, I would divert 50% of the traffic to this experiment. It would take 35 days to complete. If everything go well and there is no other experiment need to run at the same time, I might then speed up the experiment by diverge more traffic to it.

## Experiment Analysis
### Sanity Checks
>For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)

>For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass.

| Invariant Metrics | Lower bound | Upper bound | Observed | Passes |
| --- |--- | --- | --- |
| Number of cookies | 0.4988 | 0.5012 | 0.5006 | Yes |
| Number of clicks on "Start free trail" | 0.4959 | 0.5041 | 0.5005 | Yes |
| Click-through-probability on "Start free trail" | -0.0013 | 0.0013 | 0.0001 | Yes |

The table above shows the lower and upper bound of the confidence interval of each metrics. The observed value for both metrics are within the confidence interval, meaning there is no statically significant difference between the control and the experiment groups for each invariant metrics. Hence,
we can continue with our experiment.

### Result Analysis
>Effect Size Tests
For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

| Evaluation Metrics | Lower bound | Upper bound | Statistically significant | Practically significant |
| --- | --- | --- | --- |
| Gross conversion | -0.029123 | -0.011986 | Yes | Yes |
| Net conversion | -0.011605 | 0.001857 | No | No |

The table above calculates the lower and upper bounds of the confidence interval of the differences between control and experiment group for evaluation metrics. Because the lower bound for the difference of gross conversion is smaller than zero, we can conclude that the difference of `Gross conversion` is statistically significant. Similarly, because zero is within the CI for `Net conversion`, we conclude that the `Net conversion` is not statistically significant.

The practical significant level for `Gross conversion` and `Net conversion` are 0.01 and 0.0075 respectively. Because the absolute value of the lower bound of `Gross conversion` is greater than 0.01, it is practically significant. And because, the practically significant interval of -0.0075 to 0.0075 overlaps with the CI for `Net conversion`, we conclude that the `Net conversion` is *NOT* practically significant.

### Sign Tests
>For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)

| Invariant Metrics | p-value | Statistically significant |
| --- | --- |
| Gross conversion | 0.0026  | Yes |
| Net conversion | 0.6776 | No |

The p-value for gross conversion is 0.0026. This is saying that if there is no actual difference in gross conversion, then there is only 0.26% of the chance that we would observe the experiment result. Because 0.26% is well below our significant level 0f 5%, we conclude the difference for `Gross conversion` is significant.

The p-value for `Net conversion` says there is a 67% of the probability that we would get the result just by chance. Hence it is not statistically significant.

### Summary
>State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

I didn't use Bonferroni correction.

The two metrics Gross conversion and net conversion is most likely to be highly corrected.ß
Using Bonferroni correction will cause the tests to be too conservative.

There is no discrepancy between the effect size test and sign test. The difference in gross conversion is significant is both tests, and the difference in net conversion is not in both case.

### Recommendation
>Make a recommendation and briefly describe your reasoning.

Based on the result, I will recommend launch the experiment.

The gross conversion is both statistically and practically significantly lower in the experiment group, This is what we expected in order to launch. Similarly, the net conversion in group experiment is not significantly different from our control. This is also waht we expected in order to launch the experiment.

## Follow-Up Experiment
>Give a high-level description of the follow-up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.

#### description
This experiment we have examined in this project asked the student on the amount of time they had available to devote to the course. While time is an important factor, lack of prerequisite skills and experience could also
cause students' frustration.

I propose to have questions that ask students about their previous experience, for example, whether the student has any coding experience in Python. If the user doesn't have any of the specified skills required for a course, then it should suggests the student to enrol in the more appropriate course or degree instead (i.e. introduction to programming)

#### hypothesis
_null hypothesis_: asking questions regarding student background before the student will not change the net conversion significantly.
_alternative hypothesis_: asking questions regarding student background before the student will increase the net conversion significantly.

#### unit of diversion
The proposed experiment is essentially the same as the one we have examined in the project. Hence, the
same unit of diversion could be used (cookies).

#### Invariant metrics
As the nature of the experiment is essentially the same, we could use the same metrics used in this project including, number of clicks, number of unique cookies,
and Click-through-probability.

#### Evaluation metrics
could use the same metrics used in this project, most importantly, the net conversion.
