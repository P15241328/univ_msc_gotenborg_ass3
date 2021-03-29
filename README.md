# LT2222 V21 Assignment 3

Your name: Mark Grech

 
## Part 1
The routing responsible for Model training is already complete. Explain what the functions a, b, and g do, as well as the meaning of the command-line arguments that are being processed via the argparse module.

Function a. Responsible for the loading and parsing of the textfile which comprises of a list of sentences sourced from newspaper articles written in Swedish. This routine parses each sentence,word and in turn each letter. Letters are stored a list data structure. The function returns two lists, the first being a list of characters including non-alphanumeric characters representing the entire text. The list is delimited with two tags. The second data structure entails of a set of unique characters which is present in the entire training corpus X.

Function b. The parameters returned by previous function namely a() are both passed as u and p respectively. The tokenised text (list u) is parsed and consonants are excluded from further processing. In the presence of a vowel, the 2 preceeding characters and the 2 successive characters are taken note of and are passed to function g() for vectorisation. Two data structures are returned:
  a. Vectors comprising of the 4 characters.Since a total of 4 characters are taken into account, that is the two preceeding and the two successive, the resulting vector has a shape (1,4*len(X)). As n vowels is inferred within the text, then the shape of the data structure is: (n, (1,4*len(X)))
  b. The indices of the vowels. The actual vowels found within the newspaper text are not passed to the data model but rather these are encoded as the positions of the respective vowel within the vowels array. The resulting array comprising of n vowels, having a shape of (n,1) represents the target/independent feature.  

Function g. Accepts two parameters - a vowel and a list of unique characters that was inferred by function a(). This routine is responsible for the vectorisation of the vowels/characters. A numpy array having the length of the entire corpus (length of X) is created and the position of the respective vowel is essentially marked. 

The function of the argparse module is simply to read the parameters specified when the train.py is invoked from the shell. In this case a total of 4 arguments are to be expected to be specified. 

            usage: train.py [-h] [--k K] [--r R] --d D] m h

The definition of the command-line parameters follows below:<br>
a. Parameter --k K. Number of Hidden Units for each layer within the neural network. Optional. K must be of Integer type having a default value of 200 in case the parameter is omitted.<br>
b. Parameter --r R. Number of epochs. Optional. R must be of Integer type having a default value of 100 in case the parameter is omitted.<br>
c. Parameter --d D. Denotes the dropout rate for the regularisation layer in the neural network. Optional. D must be of Float data type having a default value of 0.0 in case the parameter is omitted.<br>
d. Parameter m. Name of training data file. Parameter is mandatory and its value must be of String type. <br>
e. Parameter h. Denotes the file name of the trained model which is serialised in binary format (pickled) to local disk. Mandatory. The name of the output file must be a value of String type.

## Part 2
The eval.py script evaluates the performance of the trained model on unseen data. Similar to the training procedure, the python file can be executed from the command-line. The script can be involved as follows:

          usage: eval.py [-h] [--cmfile CMFILE] [--v V] h m 

a. Parameter -cmfile CMFILE. Optional. Denotes the name of the confusion matrix graphic to be saved to local disk. This default file name is set to confusion_matrix.jpg.
b. Parameter m and h. See above.

An overview of the functionality implemented in this script follows.

a. <b>read_command_line_params</b>. Reads the command-line parameters that were supplied to the script.<br><br>
b. <b>load_model</b>. Loads the trained PyTorch model from local disk. The model includes the 3-layer neural network and the weights inferred during forward and backward propogation. The VowelModel class which essentially represents the Pytorch neural network model stores other properties including the vocabulary/corpus pertaining to the same data model.<br><br>
c. <b>load_test_data</b>. Reads the newspapers articles from the test data file, builds a unique set of letters that represents the vocabulary and tokenises each letter.<br><br> 
d. <b>vectoriser</b>. Parses each token, deduces the 2 preceeding and 2 successive characters and builds a vector. This function slightly differs from the vectorisation method implemented in the training script as vectorisation solely occurs on the known set of characters inferred during model training. The vectors built by this routine represent the predictive features whilst the actual vowel represents the target variable. <br><br> 

e. <b>predict</b>. Feeds the vectors to the neural network and performs multi-class classification. Being a multi-class problem it is important to make note of the final layer within the neural network which comprises of a LogSoftMax function. The SoftMax function computes the probability of each class whereby the sum of all probabities sum up to 1. On the other hand, LogSoftMax is essentially:

                        f(x) = log(softmax)

Unlike softmax, the function makes use of log probabilties and resulting probabilities do not tally up to 1. The probability for the respective class can be calculated by the exp(x). The advantage of LogSoftMax over SoftMax is the improved mathematical statbility and gradient optimisation which yield better predictive performance.  

f. <b>replace_vowels_test_text</b>. Replaces the vowels in the test data with those predicted.<br>

## Part 3

This part of the task entails the training and evaluation of the model with various values of K and R. To this effect,two bash scripts were developed that execute the two processes. The scripts have been extended to include added functionality so that an audit is retained for each run. In essence, the current configuration is stored by train.py in a JSON file to be latter read by the eval.py since each module runs independently from one another. The auditing routine creates a unique folder for each run and retains a number of files. The log file comprises of the command-line parameters, training and test accuracy scores amongst others. A copy of the serialised model and confusion matrix is also retained in the respective folder.   

a. <b>run_train_test_part1.sh</b>. Runs different variations of the --k option, holding the --r option at its default.<br>
b. <b>run_train_test_part2.sh</b>. Runs different variations of the --r option, holding the --k option at its default.<br>

The predictive performance of the models is evaluated by means of the accuracy metric, that is the number of the correct/incorrect classification of vowels. The table below lists both the training and testing accuracy. It can be observed that run 10 yielded the best model having an accuracy score of 40%.   

<table border="0" cellpadding="0" cellspacing="0" width="488" >
 <colgroup><col width="67" >
 <col width="108" >
 <col width="81" >
 <col width="119">
 <col width="113">
 </colgroup><tbody><tr height="21" style="height:16.0pt">
  <td height="21" class="xl65" width="67" style="height:16.0pt;width:50pt;font-size:
  10.0pt;color:white;font-weight:700;text-decoration:none;text-underline-style:
  none;text-line-through:none;font-family:Calibri, sans-serif;border-top:.5pt">run ID</td>
  <td class="xl65" width="108" style="width:81pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt ">hidden
  units</td>
  <td class="xl65" width="81" style="width:61pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt">epochs</td>
  <td class="xl65" width="119" style="width:89pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt ">train
  accuracy</td>
  <td class="xl65" width="113" style="width:85pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt">test accuracy</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >1</td>
  <td align="right" >25</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.314</td>
  <td align="right" >0.314</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >2</td>
  <td align="right" >50</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.216</td>
  <td align="right" >0.215</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >3</td>
  <td align="right" >100</td>
  <td align="right" >100</td>
  <td class="xl66" align="right">0.154</td>
  <td align="right" >0.153</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >4</td>
  <td align="right" >150</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.380</td>
  <td align="right">0.377</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >5</td>
  <td align="right" >200</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.352</td>
  <td align="right" >0.354</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >6</td>
  <td align="right" >300</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.396</td>
  <td align="right" >0.395</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >7</td>
  <td align="right" >200</td>
  <td align="right" >20</td>
  <td class="xl66" align="right" >0.181</td>
  <td align="right" >0.181</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >8</td>
  <td align="right" >200</td>
  <td align="right" >50</td>
  <td class="xl66" align="right" >0.383</td>
  <td align="right" >0.383</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >9</td>
  <td align="right" >200</td>
  <td align="right" >100</td>
  <td class="xl66" align="right" >0.386</td>
  <td align="right" >0.385</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >10</td>
  <td align="right" >200</td>
  <td align="right" >150</td>
  <td class="xl66" align="right" >0.400</td>
  <td align="right" >0.399</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >11</td>
  <td align="right">200</td>
  <td align="right">200</td>
  <td class="xl66" align="right">0.150</td>
  <td align="right">0.148</td>
 </tr>
 <tr height="0" style="display:none">
  <td width="67" style="width:50pt"></td>
  <td width="108" style="width:81pt"></td>
  <td width="81" style="width:61pt"></td>
  <td width="119" style="width:89pt"></td>
  <td width="113" style="width:85pt"></td>
 </tr>
</tbody></table>


## Bonuses
Bonus Part A: Perplexity (4 points)

Bonus Part B: Sequence (15 points)

Bonus Part C: Dropout (3 points)<br>
Regularization was included in the VowelsModel through the inclusion of a dropout layer which should theoretically address issues related to overfitting. The accuracy scores shown in the table hereunder suggest an improvement in the model's performance.    

<table border="0" cellpadding="0" cellspacing="0" width="488" >
 <colgroup><col width="67" >
 <col width="108" >
 <col width="81" >
 <col width="119">
 <col width="113">
 </colgroup><tbody><tr height="21" style="height:16.0pt">
  <td height="21" class="xl65" width="67" style="height:16.0pt;width:50pt;font-size:
  10.0pt;color:white;font-weight:700;text-decoration:none;text-underline-style:
  none;text-line-through:none;font-family:Calibri, sans-serif;border-top:.5pt">run ID</td>
  <td class="xl65" width="108" style="width:81pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt ">hidden
  units</td>
  <td class="xl65" width="81" style="width:61pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt">epochs</td>
    <td class="xl65" width="81" style="width:61pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt">dropout rate</td>
  <td class="xl65" width="119" style="width:89pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt ">train
  accuracy</td>
  <td class="xl65" width="113" style="width:85pt;font-size:10.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt">test accuracy</td>
 </tr> 
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >12</td>
  <td align="right" >200</td>
  <td align="right" >150</td>
  <td align="right" >0.2</td>
  <td class="xl66" align="right" >0.585</td>
  <td align="right" >0.629</td>
 </tr>
  <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >13</td>
  <td align="right" >200</td>
  <td align="right" >150</td>
  <td align="right" >0.4</td>
  <td class="xl66" align="right" >0.498</td>
  <td align="right" >0.598</td>
 </tr>
  </tr>
  <tr height="21" style="height:16.0pt">
  <td height="21" align="right" >13</td>
  <td align="right" >200</td>
  <td align="right" >150</td>
  <td align="right" >0.6</td>
  <td class="xl66" align="right" >0.358</td>
  <td align="right" >0.499</td>
 </tr>
 </table>

## Other notes
The solution entails the development of additional modules also written in Python. <b>

a. helper.py. Includes a number of commonly used routines and auditing functions. <b>

b. plot.py. Includes routines to create a confusion matrix in graphical format. 