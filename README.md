# DALI Data Challenge 2025

## Problem: Count the number of barnacles in an image

A simple-to-understand but difficult problem to solve. This notebook takes a model-based approach to tackling the challenge. While an end-to-end model that accurately counts barnacles seems achievable, it turned out to be much more difficult than expected. The notebook experiments with traditional computer vision techniques—such as chunking, thresholding, binarization, and contour detection—as well as state-of-the-art methods using Meta's SAM (Segment Anything Model).

## Future Directions

If given more time to progress this project, several avenues could improve both the implementation of the current methods and the exploration of alternative approaches. Below, these are outlined:

### Improvements to Implementation of Current Methods
1. **Define improved metrics to evaluate the quality of classifiers.**  
   Metrics are a critical part of the ML engineering pipeline, which was only partially addressed in this project. Four total metrics are defined to evaluate the model:
   - Naive comparison based on barnacle counts.
   - Intersection over Union (IoU).
   - Dice coefficient.
   - Accuracy.  
   Each metric provides a unique view of model quality.

2. **Experiment with SAM prompting instead of automatic mask detection.**  
   The current implementation uses SAM's automatic mask detection, tuned with several hyperparameters. However, this approach results in SAM masking everything in the image, including non-barnacle objects, leading to overcounting and reduced precision.  
   Potential improvements include:
   - Using traditional CV techniques to filter non-barnacle objects (e.g., by leveraging convexity, area thresholds, and connectivity).  
   - Utilizing SAM's object mask [prompting](https://github.com/facebookresearch/segment-anything/blob/main/notebooks/predictor_example.ipynb), which allows for segmentation based on specified pixel locations. Prompting SAM for barnacle masks could significantly increase precision.

3. **Explore additional CV techniques and preprocessing methods.**  
   Barnacles are visually identifiable by their color, size, shape, and other features. While several traditional CV techniques were applied, more could be explored to improve contour detection or feature extraction for ML models.

### Alternative Methods
1. **Model-based approach:**  
   A traditional ML approach might also be worth attempting. Barnacles have distinct visual characteristics that could be leveraged by models such as CNNs or other classifiers. While the dataset used in this project was limited, one solution could involve curating a dataset by cropping existing segmentations with a fixed radius (e.g., 40 pixels). Challenges such as handling overlapping barnacles and selecting appropriate negative examples exist but are surmountable.

2. **Visualization-based approach:**  
   Predicting the total number of barnacles might not be the best way to address the core challenge. Important considerations include:  
   - How much trust would organizations place in a model outputting only an integer?  
   - Are additional insights (e.g., signs of disease, sickness, or mutation) gained through manual barnacle annotation?  
   Alternative solutions could involve:  
   - Visualization tools to ease the annotation process for biologists.  
   - A triaging system to flag the least confident segmentations for human review.  
   - Tools to highlight anomalous image portions that require ecological follow-up.  
   Such solutions might augment workflows more effectively than a black-box counting model.
