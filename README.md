# Xanadu-Transfer-Learning
This repository is to document my testing of a code developed to employ quantum transfer learning, demonstrated by a team at Xanadu. 

The paper can be found here: https://arxiv.org/abs/1912.08278

The raw code can be found here: https://pennylane.ai/qml/demos/tutorial_quantum_transfer_learning.html

The original code demonstrates a machine learning algorithm with a quantum processing unit element that acts to classify input images of ants and bees. The testing of this code is to determine if the quantum element has any effect on this output, or if it could be achieved classically. In general, this was done by comparing the original code with a modified version, considering the maximum test accuracy each was able to achieve, and the convergence rate. The maximum test accuracy describes the percentage of classifications the algorithm was able to correctly identify on an unseen ‘test’ data set. This is a good benchmark for the accuracy of the algorithm. The convergence rate described how many iterations of the code (or ‘epochs’) it takes to reach this maximum accuracy. It is a decent measure of the efficiency of the algorithm, although is not the only one.

This repository is split into three testing phases, each with a different purpose. In each, the only modifications have been made to the quantum processing unit, or the quantum element of the machine learning process, and the output. Because of the random nature of machine learning tasks and quantum processing, each experiment was repeated many times to ensure any outlying results were obvious. To reduce the processing time of these code, I would suggest first reducing this number, defined as N throughout. All other changeable values (such as input qubit number, circuit depth, and batch size) were kept consistent with the values provided by Xanadu. If you would like to run these experiments yourself, you will need to download the data files. The download and instructions are given on the same webpage as the raw code. The data directory will need to be changed. 

 The testing phases and related codes are outlined below. 
 
## The effect of Entanglement

In general, when considering a quantum system, we want to find a quantum advantage, or something that the system can do that cannot be replicated classically. From an analysis of the quantum processing unit in the original code, it was apparent that the only element that could not be strictly replicated classically was the entanglement created through the use of CNOT gates. As such, we were interested to determine the effects of removing these entangling gates. 

The codes in this section of the repository include :

**Quantum without Entangling Gates: Accuracy**

This code prepares each system, one with the original quantum processing unit and one with a modified quantum processing unit without entangling gates, and then trains and tests the model to determine the best test accuracy. The system is reset and re-trained 100 times, and all the best test accuracy values are presented in a histogram. This code took about 50 hours to run on my machine, and the results are posted in the results folder under “ Quantum without Entangling Gates: Accuracy results”. 

**Quantum without Entangling Gates: Efficiency**

To determine the convergence rate, we use the value of the loss function. For each of the two processing units, the loss function value at each epoch iteration is recorded. The system is reset and re-trained 100 times, and the loss function value at each epoch is averaged. The loss function value against epoch number is then plotted for each processing unit case. We expect the loss to fall, and then platau at some minimum value. The quicker the system is able to reach this platau, the better its convergence rate. . This code took about 50 hours to run on my machine, and the results are posted in the results folder under “ Quantum without Entangling Gates: Efficiency results”. 

## Comparison with a classical model 

We also determined it useful to consider the quantum processing unit against the simplest classical replacement model. In this case, the system runs using a neural network structure, and as such the simplest replacement would be a fully connected linear layer. 
The codes in this section of the repository include :

**Quantum vs Linear: Accuracy**

This code prepares each system, one with the original quantum processing unit and one with a fully connected linear layer, and then trains and tests the model to determine the best test accuracy. The system is reset and re-trained 100 times, and all the best test accuracy values are presented in a histogram. This code took about 50 hours to run on my machine, and the results are posted in the results folder under “Quantum vs Linear: Accuracy results”.  

**Quantum vs Linear: Efficiency**

To determine the convergence rate, we again use the loss function. For each of the two processing units, the loss function value at each epoch iteration is recorded. The system is reset and re-trained 100 times, and the loss function value at each epoch is averaged. These results are presented in two graphs of the loss function value against epoch number, one for each system case. This code took about 50 hours to run on my machine, and the results are posted in the results folder under “ Quantum vs Linear: Efficiency results”. 

## Training and Testing Accuracy 

All of the above tests had an anomaly. When we ran them, the test accuracy was consistently higher than the training accuracy, which is very unusual for machine learning. We wanted to determine why this might be the case. 

The codes in this section of the repository include :

**Confusion Matrix for Train and Test Accuracy**

This code takes the machine learning algorithm as designed by Xanadu and fully trains it once, creating a set model. This model is then subjected to the scikit confusion matrix, which determines its classification capabilities for both the training and test data sets. The accuracy, as calculated form this measure, is given for both cases. This process is repeated 100 times. Two histograms are produced. The first describes the difference between the train and test accuracy, as calculated automatically, and the frequency with which both values occur. The second gives the same measurement, but for the accuracy values as determined by the confusion matrix. If the test accuracy was higher than the train, we would see mostly negative numbers. If the numbers are mostly positive, this implies the train accuracy is higher than the test, which is more to what we would expect. 

