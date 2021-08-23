# Xanadu-Transfer-Learning

This repository is to document my testing of a code developed to employ quantum transfer learning, demonstrated by a team at Xanadu. 

The paper can be found here: https://arxiv.org/abs/1912.08278

The raw code can be found here: https://pennylane.ai/qml/demos/tutorial_quantum_transfer_learning.html

The codes in this repository include :

**Quantum vs Linear: Accuracy**

This code tests the final classification accuracy of the given code, against one in which the quantum classifier is replaced with a fully connected linear layer. This provides an initial baseline to see if this quantum system is more accurate than the simplest classical replacement. The test is run 100 times, and a histogram is produced to demonstrate this comparison. 

**Quantum vs Linear: Efficiency**

This code tests the efficiency of the given code against one in which the quantum classifier is replaced with a fully connected linear layer. For each code, the accuracy is compared to the epoch iteration, which demonstrates how many epochs it takes for each code to achieve its highest possible level of accuracy. This provides an initial baseline to see if this quantum system is more efficient than the simplest classical replacement. The test is run 100 times, and an averaged loss plot is produced for the quantum and classical case. 

**Confusion Matrix for Train and Test Accuracy**

Both of the above tests had an anomaly. When we ran them, the test accuracy was consistently higher than the training accuracy, which is very unusual for machine learning. This code initialises a confusion matrix to determine the true training and testing accuracy. From this, we determined that the train accuracy as calculated automatically by the provided code was not an accurate representation of the ability of the system to classify the training set. However, the test accuracy, which tends to be the more impactful number, was consistent across all calculations. 

**Quantum without Entangling Gates: Accuracy**

An analysis of the provided quantum processing unit revealed that the only capacity of the processing unit that could not be efficiently replicated classically was the use of CNOT gates, which entangled the qubits. With the removal of these gates, the modified quantum processing unit was analogous to a simple classical classifier. As such, a comparison between these two quantum systems would be able to reveal if the original quantum processing unit has any quantum advantage. A comparison test was run 100 times, and a histogram is produced to demonstrate this comparison.

**Quantum without Entangling Gates: Efficiency**

This code tests the efficiency of the quantum processing unit with and without its entangling gates. For each processing unit, the accuracy it is able to achieve is compared to the current epoch iteration, which demonstrates how many epochs it takes for each to achieve its highest possible level of accuracy. This provides a measure of each units efficiency.  The test is run 100 times, and an averaged loss plot is produced for the entangled quantum and non-entangled classical case. 
