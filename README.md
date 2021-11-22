# Xanadu-Transfer-Learning
This repository is to document my testing of a code developed to employ quantum transfer learning, demonstrated by a team at Xanadu. 

The paper can be found here: https://arxiv.org/abs/1912.08278

The raw code can be found here: https://pennylane.ai/qml/demos/tutorial_quantum_transfer_learning.html

The original code demonstrates a machine learning algorithm with a quantum processing unit element that acts to classify input images of ants and bees. The testing of this code is to determine if the quantum element has any effect on this output, or if it could be achieved classically. In general, this was done by comparing the original code with a modified version, considering the maximum test accuracy each was able to achieve, and the convergence rate. The maximum test accuracy describes the percentage of classifications the algorithm was able to correctly identify on an unseen ‘test’ data set. This is a good benchmark for the accuracy of the algorithm. The convergence rate described how many iterations of the code (or ‘epochs’) it takes to reach this maximum accuracy. It is a decent measure of the efficiency of the algorithm, although is not the only one.

This repository is split into several testing phases, each with a different purpose. In each, the only modifications have been made to the quantum processing unit, or the quantum element of the machine learning process, and the output. Because of the random nature of machine learning tasks and quantum processing, each experiment was repeated many times to ensure any outlying results were obvious. To reduce the processing time of these code, I would suggest first reducing this number, defined as N throughout. All other changeable values (such as input qubit number, circuit depth, and batch size) were kept consistent with the values provided by Xanadu. If you would like to run these experiments yourself, you will need to download the data files. The download and instructions are given on the same webpage as the raw code. The data directory will need to be changed. 

 The testing phases and related codes are outlined below. 
 
## The effect of Entanglement

In general, when considering a quantum system, we want to find a quantum advantage, or something that the system can do that cannot be replicated classically. From an analysis of the quantum processing unit in the original code, it was apparent that the only element that could not be strictly replicated classically was the entanglement created through the use of CNOT gates. As such, we were interested to determine the effects of removing these entangling gates. 

The codes in this section of the repository include :

**Quantum without Entangling Gates: Accuracy**

This code prepares each system, one with the original quantum processing unit and one with a modified quantum processing unit without entangling gates, and then trains and tests the model to determine the best test accuracy. The system is reset and re-trained 100 times, and all the best test accuracy values are presented in a histogram. 

**Quantum without Entangling Gates: Efficiency**

To determine the convergence rate, we use the value of the loss function. For each of the two processing units, the loss function value at each epoch iteration is recorded. The system is reset and re-trained 100 times, and the loss function value at each epoch is averaged. The loss function value against epoch number is then plotted for each processing unit case. We expect the loss to fall, and then platau at some minimum value. The quicker the system is able to reach this platau, the better its convergence rate. This code took about 50 hours to run on my machine, and the results are posted in the results folder under “Quantum without Entangling Gates: Efficiency results”. 

**Effect of Qubit Number and Depth**

Demonstrated over three codes:

Epoch_vs_depth_with_CNOT

Epoch_vs_depth_without_CNOT

Analyse_Epoch_vs_Depth_data

This system of codes aimed to determine if the qubit number and circuit depth had any impact on the effect of entanglement of the system. The first two codes code calculate the best test accuracy from each combination of 1-8 qubits and a depth of 1-10, 10 times, and outputs these files using pickle.dump. There are two versions, one using the quantum processing unit with CNOT gates and one without. The purpose of these codes is to create data to be analysed using the code Analyse_Epoch_vs_Depth_Data. In this analysis, a heat map is created of the average best test accuracy for each combination of qubit number and depth. This is to analyse the effect of these two parameters on best test accuracy, and whether these parameters impact the results with and without the entangling CNOT gates. These codes together took over a week to run. The results are presented in the results folder under “Effect of Qubit Number and Depth”. 

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

This code takes the machine learning algorithm as designed by Xanadu and fully trains it once, creating a set model. This model is then subjected to the scikit confusion matrix, which determines its classification capabilities for both the training and test data sets. The accuracy, as calculated form this measure, is given for both cases. This process is repeated 100 times. Two histograms are produced. The first describes the difference between the train and test accuracy, as calculated automatically, and the frequency with which both values occur. The second gives the same measurement, but for the accuracy values as determined by the confusion matrix. If the test accuracy was higher than the train, we would see mostly negative numbers. If the numbers are mostly positive, this implies the train accuracy is higher than the test, which is more to what we would expect. This code took about half an hour to run, and the results are posted in the results folder under “Confusion Matrix for Train and Test Accuracy”.

## Contributions of the QPU

From the results collected above, changing the QPU in a variety of ways had a limited impact on the overall abilities of the system. We wanted to determine how much processing ability was being added by the QPU by isolating the processing ability of the system before and after its application. 

The codes in this section of the repository include :

**Resnet_feature_ouput**

The consistency of the above results prompted analysis into how much processing work was left for the quantum processing unit to achieve after the data had passed through ResNet and the first linear layer. This was done by using two qubits at each processing stage, and initialising a confusion matrix after the algorithm as a whole, then replacing the quantum processing unit and last linear layer with an identity layer and initialising a confusion matrix for this new system. In this way, we are able to determine how much extra processing is achieved by the quantum processing unit, without restarting the system to eliminate the random element that might inpact this comparison. Using this method, the algorithm is not re-trained between measurements. This code took about half an hour to run, and the results are posted in the results folder under “ Resnet_feature_ouput”. 

## New Quantum Processing Unit 

The codes in this section of the repository include :

**Quantum Kernel Utalising KME and MMD**

This research prompted us to look into a new form of quantum processing unit. This quantum processing unit is based on encoding features into a superposition of single photon states, and evaluating their similarity measure as a form of kernel based machine learning. However, at this stage this is explored through the simulation of the classical output, and does not contain a strictly quantum simulation. This code takes about 10 minutes to run. 
