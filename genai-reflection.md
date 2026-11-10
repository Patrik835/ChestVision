# Reflection on GenAI usage

This document reflects on the integration of generative AI during the phase 1 of our project, where we have evaluated a pre-trained model on the CheXpert dataset as a black box.

We executed this project in two pairs: the AI-assisted one, which used AI to generate code, to analize data and results and also as a collaborator to bring some ideas and propose different points of view. And the Non-AI pair, which did a more traditional exploratory data analysis without AI assistance.


## Documented use

Generative AI was mainly used with two objectives during this phase. The first one is as a tool, transforming data and generating code, and the second was is as a collaborator of the project, suggesting ideas, hypothesis, analizing and extracting conclusions from visualizations...

## Benefits and limits

One of the main benefits of AI usage was the time required to see first results. The AI pair was able to iterate rapidly through more complex visual designs and consequently, having first conclusions or hypothesis. Whereas the other pair had to build a more structured, step-by-step pipeline using simpler graphics, and took more time to have an idea of how the model was behaving.

While the non-AI pair spent time writing code and analizing the plots and the results, the AI assisted spent comparable time validating the AI-generated code, fixing and adjusting some things, and verifying the interpretations of the artificial intelligence.

Another benefit was the quick debugging of environment-specific problems, git conflicts, or incompatibilities with the libraries.

In some cases one of the limitations was some really innacurate proposed explanations of the plots or the numeric results due to AI hallucinations.

## Human checks still required

While AI accelerated code generation and visual design, rigorous human oversight was essential to ensure validity of the results.

We performed direct manual reviews of some of the X-ray images of the extreme clusters the model was predicting, to see directly what was going on, and what they had in common.

Also we spent a lot of time verifying the generated code, modifying it, adjusting prompts and always checking the AI analytical claims.

