---
layout: post
title: "Edtech Webinar Notes - AI Assisted Learning"
author: "Daniel E"
categories: notes
tags: [notes, edtech, literature, ethics]
image: edtech42.png
---

In a bid to stay up-to-date with the latest developments in the field of AI, I attended a webinar on Edtech organised by KidsLoop.
In this webinar I joined Dr Mutlu Cukurova, who painted a holistic picture of AI in Education with a particular focus on its potential to help us support teaching and learning processes.

Dr Cukurova presented three different conceptualisations of AI in Education and argue that each might have different contributions to the education field.  He will substantiate the talk with examples from his recent research projects in which AI technologies and multimodal data are used to interpret and support complex social skills in digital and physical hybrid spaces.

In this article I refined my webinar notes into a piece of writing that I and others can revisit.

Here's what I learnt:

## Summary
When it comes to AI in education, the literature is paramount. We must first understand how humans learn before we can even think about how AI methods can be designed. Interdisciplinary research from learning sciences has helped us understand a great deal about how humans learn. Now, this same body of research must now be used to better inform the development of AI technologies for use in education.

It is likely a better approach to explore this solution space with the wider goal of improving education outcomes rather than only seeking to improve the state-of-the-field in AI.
Intelligence augmentation approaches in which human compatible AI supports educators are preferred over ones that replace them through automation.
To that end, Dr Cukurova explains MMLA, multi-modal learning analytics systems, designed to empower decision making in teaching.

MMLA basically takes advantage of the fact that everything we do generates data. As I've said numerous times: "we are drowning in information and starving for knowledge". Oftentimes, we don't even have data collection infrastructure to collect and analyse richer types of data. That was not the case for Dr Cukurova. In their 2017 paper, Cukurova et al (1) collected various streams of data, processed and extracted multimodal interactions to answer the following question: which features of MMLA are good predictors of collaborative problem-solving in open-ended tasks in project-based learning?

The literature landscape on potential predictors looks like this:
![Pic of data types](/assets/img/dataliterature.png)

Collecting data like heart rate, facial expressions, blinks, postures, head rotations, seat pressure can help us model student alertness and figure out if students are stressed, experiencing cognitive load, or if they need a break.
With multimodal data from learners, wakefulness can also be predicted, as shown in one paper Dr Cukurova contributed to (0). Wakefulness of learners strongly relates to educational outcomes, and so when e-learning, it would be useful to measure/monitor attention/drowsiness from learning behaviour. Doing this is not an easy task however, especially if only doing it from log data, though it was found CatBoost was the most performant.

In their 2018 paper, Cukurova et al (2) find promising results when it comes to using this data on traditional machine learning methods with multimodal data, though none of the findings are viable to be framed for a commercial use case yet. More feature selection can certainly be done to uncover the best combination of data modalities in terms of predictive power, as it was found that more modalities does not automatically mean better prediction.

In another one of Dr Cukurova's papers (3), on debate mentoring, multi modal data was collected in the form of
psychometric data, experience and audio of debates. With some dimensionality reduction (PCA) and a simple multinomial logistic regression algorithm, debating success could be modelled quite well.
As a result, with the use of analytically transparent models, factors that lead to higher scores could be relayed to mentors and users of the e-learning platform to help improve their practices and thus, their scores. What's really encouraging is that mentors said findings/machine suggested actions made sense or were at least thought provoking.

MMLA is not without its challenges however. On top of data and machine learning considerations (data collection, noise, defining outcome variable, system design, imbalanced data, technical/human bias, explainability (and the complexity/explainability tradeoff), drift, etc) one is also faced with practical considerations. How do you collect all this data without being invasive? How can we find enough teachers that are as passionate about this endeavour as we are? How do we know we can trust these algorithms? Is this ethical?
When it comes to decisions relating to learning processes , will learners and educators trust these machines/models enough to use them?
Generally speaking, when there is decisions have high consequences, it is best to leave those to a human, and delegate/automate the processes that have low consequence to a machine.

The main takeaway from this talk is that, it's still early doors. Dr Cukurova's work uncovered things that we did not know (were possible) even a couple of years ago. Maybe in a couple of years all these questions will be answered and we're further into the problem solving process.

### Keywords
notes, edtech, literature, ethics

### Learn more:
* (0) Kawamura, Ryosuke, et al. "Detecting Drowsy Learners at the Wheel of e-Learning Platforms With Multimodal Learning Analytics." IEEE Access 9 (2021): 115165-115174. [url:](https://doi.org/10.1109/ACCESS.2021.3104805)
* (1) Spikol, Daniel, et al. "Current and future multimodal learning analytics data challenges." Proceedings of the seventh international learning analytics & knowledge conference. 2017. [url:](https://doi.org/10.1111/jcal.12263)
* (2) Spikol, Daniel, et al. "Supervised machine learning in multimodal learning analytics for estimating success in project‐based learning." Journal of Computer Assisted Learning 34.4 (2018): 366-377. [url:](       )
* (3) Cukurova, Mutlu, Carmel Kent, and Rosemary Luckin. "Artificial intelligence and multimodal data in the service of human decision‐making: A case study in debate tutoring." British Journal of Educational Technology 50.6 (2019): 3032-3046. [url:](https://doi.org/10.1111/bjet.12829)
