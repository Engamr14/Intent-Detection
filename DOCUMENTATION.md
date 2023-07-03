# Intent-Detection
## Problem Description
Intent detection is a crucial task in the field of natural language processing (NLP) and speech recognition. It involves identifying the underlying intention or purpose of a spoken or written statement. This task is particularly important in the development of voice assistants and other audio-based applications, as it allows the system to understand and respond appropriately to the user’s requests and commands. In the context of audio intent detection, the system must be able to accurately interpret the user’s spoken words and determine their intended meaning. This requires the system to have a deep understanding of language and context, as well as the ability to recognize and differentiate between different intents or purposes. The importance of accurate audio intent detection cannot be overstated. It plays a vital role in the usability and effectiveness of voice assistants and other audio-based applications, and can greatly impact the user experience. A system that is unable to correctly interpret the user’s intentions will be frustrating and difficult to use, leading to a poor user experience and potentially even causing the user to abandon the application altogether. On the other hand, a system that is able to accurately detect and respond to the user’s intentions will be much more user-friendly and effective, leading to a better overall experience for the user. For this project, the goal is to predict both the action requested and the object that is affected by the action.

## Dataset
The dataset consists in a collection of audio file in a WAV format. Each record is characterized by several attributes. The following is a short description for each of them.

• path: the path of the audio file.

• speakerId: the id of the speaker.

• action: the type of action required through the intent.


• object: the device involved by intent.

• Self-reported fluency level: the speaking fluency of the speaker.

• First Language spoken: the first language spoken by the speaker.

• Current language used for work/school: the main language spoken by the speaker during daily activities.

• gender: the gender of the speaker.

• ageRange: the age range of the speaker.

An intent is given by the combination of an action with an object, therefore the information in the two respective columns must be combined to obtain the label to be used to address this task. The way this
information should be combined is a simple string concatenation (e.g., if the action is “increase” and the object is “volume”, the corresponding intent will be “increasevolume”).

## Proposed Solution & Methodology
We are proposing better accuracy of objects, using Natural language processing tools and multiple python libraries, one of them is python speech features. First, we analyze the features, filter audio, and labels, then we preprocess audio using preprocessing techniques following are aggregation, data reduction, and feature subset selection.

### A. Preprocessing
The data we have are just audio records, which are not suitable to be used directly as input for a machine learning classifier. ML classifiers need well-defined features as input to capture the  relationships and differences between different classes. Also, the data we have are raw data that may contain noise and other side data that are not important for our task. Therefore, the NLP pipeline we develop will start with preprocessing the raw data we have, in order to generate features to be used later in our classifier. The preprocessing part consist of 2 main operations, which are: Data Filtering and Feature Generation. For Data Filtering, the audio records have some time periods that are free of any voice, which reside there at the beginning of the record and at the end of the record. Also, there are smaller empty parts reside between spelled words. These empty time parts are not useful for our goal, and they can also affect the classifier accuracy. Therefore, we decided to remove these time parts. To do so, we must define them by a filtering threshold. So, the samples that have amplitude under the defined threshold Fth will be removed. The figure here show the signal and the spectrogram before and after audio filtering. For Feature Generation, the audio data is defined naturally as an amplitude in time and frequency. Therefore, we have combined the time domain representation of the audio signal with the frequency domain representation of it, to get what is called Spectrogram matrix, where rows represent time pins and columns represent frequency pins. Then, we split the matrix into N x M blocks, where N is the number of frequency splits and M is number of time splits. N and M are parameters that can be chosen to be any value. This step is done to reduce the number of features, which enhances the classifier performance. Then, for every block, we compute some statistical metrics that can represent this block, like mean, variance, and others, and these will be our features to be used as input for our classifier. The below figure show how this can be done.

### B. Model Selection
The following algorithms have been tested: Random forest: RF allows multiple decision trees trained on various subsets of data and features to make predictions. This typically avoids the overfitting problems of decision trees but still maintains some degree of interpretability. Random forests, like decision trees, work on one feature at a time, so no normalization is necessary. We chose to use random forests as it is supposed to work well on audio signal classification problems. Support Vector Machine: SVM algorithm applies a (possibly nonlinear) transformation to the data points and identifies the maximum-margin hyperplane that separates the classes. It is supposed to work well with our data nature. Quadratic Discriminant Analysis: QDA is a generative model. QDA assumes that each class follows a Gaussian distribution, also it is known that it works well with high dimensional data. So, it is supposed to work well on audio signal classification problems.

### C. Hyperparameter Tuning
There are two main sets of hyperparameters to be tuned: Preprocessing Hyperparameters: M, N and Fth. Classifiers Hyperparameters: random forest and SVM parameters For the preprocessing hyperparameters, the M and N, which are the numbers of splits used, we chose to use equal number for them where M = N = n. We have experimented the n’s range of 2 10. However, the threshold filter has been tested in the range of 10 3000. For the classifiers hyperparameters, the SVM hyperparameters are the kernel type (rbf, poly, sigmoid) and the C value (1, 10, 100, 1000). The RF hyperparameters are the max depth (None, 2, 5, 10, 50), number of trees (100) and criterion (gini, entropy). The QDA hyperparameters are regressionparameter(0)andpriors(None).

## Results
In this section, we present the experiments we did to get the best results from our models. The first experiment was about the suitable statistical metrics to be used as features for our classifier, and the metrics that achieved the best performance were the mean, the geometric mean, the variance, the median, the mode, and the accumulative sum (CDF). However, we excluded: maximum, minimum, 1st quarter (25The second experiment is about the preprocessing parameters, the filter threshold value the achieved the best performance is Fth = 400. The number of splits n achieved the best results when n = 5, this means that we split the spectrogram matrix to 25 blocks. The third experiment is measuring the performance of the different classifiers considering their different possible combinations of hyperparameter. The below figure shows the different hyperparameters of SVM model and the corresponding performance. The best result achieved by the Support Vector Machine classifier with parameters of Kernel = RBF, C = 100.
So, finally after choosing the highest performance options in all these experiments, we could achieve f1 score of 0.83. However, the na¨ıve solution that did not use audio filtering and just used mean and standard deviation as features achieved f1 score of 0.62. This means that our proposed solution achieved a great performance enhancement.

