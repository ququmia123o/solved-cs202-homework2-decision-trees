Download Link: https://assignmentchef.com/product/solved-cs202-homework2-decision-trees
<br>
You must draw the trees in this question by hand, and embed the scans or photographs of your drawings in your report. Make sure your drawings are clear, as ambiguities would make you lose points.

<ul>

 <li>[<em>5 points</em>] Insert 58, 85, 93, 24, 19, 44, 13, 57, 37, 94 into an empty binary search tree (BST) in the given order. Show the resulting BSTs after every insertion.</li>

 <li>[<em>5 points</em>] What are the preorder, inorder, and postorder traversals of the BST you have after (a)?</li>

 <li>[<em>5 points</em>] Delete 94, 19, 44, 24, 58 from the BST you have after (a) in the given order.</li>

</ul>

Show the resulting BSTs after every deletion.

<h1>Question 2</h1>

A decision tree is a widely used approach that consists of sequences of questions for modeling input observations for classification and regression tasks in machine learning problems. In this question you will implement a decision tree and a learning algorithm for a classification task. A brief introduction to decision trees is provided on the course home page. The model needs to handle binary features and discrete class labels, i.e., the value of each feature of an observation is either False or True, and the class label associated with an observation is an integer. In this basic setting, each non-leaf node of the decision tree uses the value of one feature, which can be False or True, to determine its decision, i.e., selecting one of its two children to continue with the decision process. Each leaf node of the tree has a final decision, i.e., a class label to be assigned to the input. Two nodes on the same path from the root to a leaf cannot use the same feature for decision, i.e., once a feature is used for decision by a node, its descendants will not use that feature for decision again.

You will implement a training algorithm that builds a decision tree from a training data set. Learning is done by partitioning the set of training examples into smaller and smaller subsets where each subset is as pure as possible. Purity for a particular subset is measured according to the number of training samples in that subset having the same class label. The algorithm uses the training samples that reach a particular node to make the best decision at that node. The decision outcome at a node is called a split. The algorithm will select, at each node, a feature for splitting the data set, and recursively repeat for the child nodes until either (1) a node is pure, i.e., all samples that the node is considering are of the same class, or (2) there are no unused features left, in which case the node will decide on the class label by choosing the majority (most frequent) class within the set of samples.

An effective learning algorithm needs to select the feature for decision (split) at a node based on a good metric. You will use the information gain metric for this purpose. Information gain with respect to a particular split is defined as

<h2>                                                                             IG(P,S) = H(P) − H(S)                                                     (1)</h2>

where <em>H</em>(<em>P</em>) is the entropy for the current node (parent) and <em>H</em>(<em>S</em>) is the entropy after a potential split <em>S</em>. Entropy at a node <em>P </em>is computed as

(2)

where <em>P<sub>i </sub></em>is the probability of class <em>i </em>at that node. Entropy of a split <em>S </em>is computed as

<em>H</em>(<em>S</em>) = <em>P<sub>left</sub>H</em>(<em>left</em>) + <em>P<sub>right</sub>H</em>(<em>right</em>)                                            (3)

where <em>P<sub>n </sub></em>is the probability of choosing the child <em>n</em>, and <em>H</em>(<em>n</em>) is the entropy of child <em>n </em>where <em>n </em>is <em>left </em>or <em>right</em>. The feature with the highest information gain among the unused features will be selected for the split at that node.

<ul>

 <li>[<em>5 points</em>] Implement a function for calculating the entropy of a node by using the set of samples at that node. You only need the number of observations that belong to each class. The function must return the calculated entropy. The prototype of the function must be the following:

  <ul>

   <li>double calculateEntropy(const int* classCounts, const int numClasses);</li>

  </ul></li>

</ul>

Example: classCounts = {32, 0, 7} (there are three classes)

(4)

<ul>

 <li>[<em>10 points</em>] Implement a function for calculating the information gain for a particular split at a node. This function should call the calculateEntropy function you implemented in the previous step. The prototype of the function must be the following:

  <ul>

   <li>double calculateInformationGain(const bool** data, const int* labels, const int numSamples, const int numFeatures, const bool* usedSamples, const int featureId);</li>

  </ul></li>

</ul>

data is a boolean array with numSamples rows and numFeatures columns, labels is a vector of length numSamples, usedSamples is also a vector of length numSamples, and featureId is the column id (between 0 and numFeatures-1) of the considered feature in the data array. usedSamples indicates, for each sample, whether that sample reached that node or not. In other words, the samples whose values are true are in the subset that is used for the decision at that particular node.

<ul>

 <li>Define two classes named DecisionTree and DecisionTreeNode. Name the files as</li>

</ul>

DecisionTreeNode.h, DecisionTreeNode.cpp, DecisionTree.h, and DecisionTree.cpp. DecisionTreeNode class must keep track of whether or not the node is a leaf node, the feature index for its decision (split) if it is a non-leaf node, its class decision if it is a leaf node, and pointers to its children. The data members and functions you will implement for this purpose is up to you.

DecisionTree class must keep a pointer to the root DecisionTreeNode, and has the public functions listed below. You can define any number of private data members and private class functions. However, you cannot modify the prototypes for the functions listed below.

<ul>

 <li>void DecisionTree::train(const bool**, const int*, const int, const int);</li>

 <li>void DecisionTree::train(const string, const int, const int);</li>

 <li>int DecisionTree::predict(const bool*);</li>

 <li>double DecisionTree::test(const bool**, const int*, const int);</li>

 <li>double DecisionTree::test(const string, const int);</li>

 <li>void DecisionTree::print();</li>

</ul>

<ul>

 <li>[<em>25 points</em>] Implement the train functions.

  <ul>

   <li>void DecisionTree::train(const bool** data, const int* labels, const int numSamples, const int numFeatures);</li>

  </ul></li>

</ul>

data is a boolean array with numSamples rows and numFeatures columns,

labels is a vector of length numSamples. Together, they define the training data set. Using this data set, the function must build the decision tree. The calculateInformationGain function you implemented earlier should be used here.

<ul>

 <li>void DecisionTree::train(const string fileName, const int numSamples, const int numFeatures);</li>

</ul>

Opens and parses the text file fileName that contains numSamples lines, each of which represents a sample (observation). Each observation is given by numFeatures+1 space-separated numbers. The first numFeatures numbers are binary (0 or 1) numbers indicating the feature values. The last number is an integer corresponding to the class label for that observation. The class numbers are between 1 and <em>N </em>where <em>N </em>is the number of classes (the largest number in this column). After parsing the text input, the above train function must be called for the actual training.

<ul>

 <li>[<em>15 points</em>] Implement the predict function.

  <ul>

   <li>int DecisionTree::predict(const bool* data):</li>

  </ul></li>

</ul>

This function can only be called after train is called. data is a boolean array of size numFeatures (numFeatures is already known by this point), and it represents a single observation. The function must return the integer class label assigned to the input observation by the decision tree.

<ul>

 <li>[<em>5 points</em>] Implement the test functions.

  <ul>

   <li>double DecisionTree::test(const bool** data, const int* labels, const int numSamples);</li>

  </ul></li>

</ul>

This function can only be called after train is called. data is a boolean array with numSamples rows and numFeatures columns, labels is a vector of length numSamples. Together they define a test data set for measuring the accuracy of the decision tree classifier. Note that the training data and test data can have different number of samples (numSamples). This function must utilize the predict function to predict the class of each observation in the test data set, compare the predictions with the true classes of the observations, and return the accuracy (proportion of true predictions among the total number of observations).

<ul>

 <li>double DecisionTree::test(const string fileName, const int numSamples); Opens and parses a text file fileName. The specification of the text file is the same with the one described in the train function. After parsing the text input, the above test function must be called for actual testing.</li>

</ul>

<ul>

 <li>[<em>10 points</em>] Implement the print function. The function prints the nodes of the tree using <em>preorder </em> Each node should be printed on a separate line. In the beginning of each line, put a number of tabs according to the level of the node being printed (e.g., the root node has no tabs and a node at level 5 has 4 tabs). The indentation will help the user understand the printed tree structure. If the node is a leaf node, the integer corresponding to the selected class is printed as “class=x” (e.g., for the second class, “class=2” is printed). For non-leaf nodes, the integer feature index used for the split at that node is printed. Note that the feature indices are zero-based but the class numbers start from 1.</li>

</ul>

<h1>Question 3</h1>

In this question you need to explain your implementations in Question 2, and analyze the time complexity of your implementations. Your answer will be written in your report.

You may, if you wish, write parts of your code or pseudo-code while explaining it.