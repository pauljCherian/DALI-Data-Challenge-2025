# DALI Data Challenge 2025

This preamble is also included in the notebook* 

## Problem: Count the number of barnacles in an image 
A simple to understand, but difficult problem to solve. My notebook below takes a model-based approach to solving the challenge. While I think that an end-to-end model that accurately count barnacles is acheivable, it ended up being much more difficult than I expected. I experiment with traditinoal CV techniques of chunking, thresholding, binarization, and contour detection, as well as SOTA methods using Meta's SAM. 

If I were to progress this project with more time, there are several avenues I think would bear fruit, both to improve on my implementation of a method, and to try others. I make a distinction and describe below:

## Improvements to Implementation of Current Methods
1.   *Define improved metrics to evaluate the quality of classifers.* This is an incredibly important part of the ML engineering pipeline that I only scratched the surface of during my work. I define 4 total metrics with which I can evaluate my model. The first (naive) approach is a pure comparison based on barnacles counted. The 2nd, 3rd, and 4th are more sophisticated (and faithful) metrics of Intersection over Union (IoU), Dice, and Accuracy. As I will mention later, each of these metrics demonstrate a different view of the quality of the model.
2. *Experiment with SAM prompting instead of automatic mask detection.* This is another implementation improvement that I think would bear fruit in improving performance. In the notebook, I use SAM's automatic mask detection, tuned with several hyperparameters that define how the segmentor should operate on our specific image. The biggest issue in this implementation is that SAM finely masks everything in the image, including non-barnacle objects. This leads to a gross overcounting (and increased union) of masks in the image. I think traditional CV methods could be used to filter out some of these erroneous masks —  barnacles tend to be convex, of a certain minimum area, and certainly simply connected (unlike a torus shape). As shown below, the current SAM segmentor has a huge swath of non-barnacle masks. Accordingly, the metrics on it show it has poor performance. 
To rememdy this, SAM allows for object mask [prompting](https://github.com/facebookresearch/segment-anything/blob/main/notebooks/predictor_example.ipynb), by specifying a pixel location which is of the object it should segment. If one could roughly do this for barnacles in the image, I think SAM would have a much higher precision on barnacle masks. 
3. *More CV techniques and pre-processing.* Barnacles' color, size, shape, and specific features make them extremely identifiable to humans. Though I use several traditional CV techniques, I am sure there are more that I have not experimented with that would make simple contour detection or feature detection for ML models much better.


## Alternative Methods


1. **Model-based approach:** *Though I tried two extremes from classic CV to pretrained SOTA SAM, I think a traditional ML approach might also be worth attempting.* As mentioned, barnacles have several characteristics that make them identifiable. The reason why the two prior approaches were relatively good fits for this problem was because we did not have an abundance of data to train a prior generation ML model on. But I think there are potential solutions to this issue. Specifically, my idea is to crop the segmentations we do have with a radius of 40 or so pixels around them, to curate a dataset of thousands of barnacles that could then be used to train a more traditional CNN or sklearn classifier. Potentially impementation challenges about what to do with overlapping barnacles and what to use as negative examples exist, but nonetheless I think it is a promising idea.
2. **Visualization based approach:** it's worth considering that a model simply predicting the total number of barnacles in an image may not be the best way to solve the core challenge. If these statistics are especially important, would there even be enough trust from organizations to use the model? What other information do scientists gain from the process of annotating barnacles? Do they scan for disease, sickness, mutation, or other ecological observations that come from manually inspecting barnacles? These are all worthy questions to ask which might lead to a non-model-based approach actually being better. This might be a visualization tool that makes annotation much easier for the biologists, a triaging system that allocates the least confident barnacle segmentations to human annotators, or something that can point out anomalous portions of an image that require a ecological follow-up. This all goes much farther than the black-box model that spits out an integer of barnacles from an image. It would be worth discussing with the clients what solution they think would best augment their workflow.




