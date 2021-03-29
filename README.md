# LT2222 V21 Assignment 3

Your name: Mark Grech

 
## Part 1
The routing responsible for Model training is already complete. Explain what the functions a, b, and g do, as well as the meaning of the command-line arguments that are being processed via the argparse module.

Function a. Responsible for the loading and parsing of a textfile which comprises of a list of sentences sourced from newspaper articles written in Swedish. The routine parses each sentence,word and letter and stores it into a list data structure. The function returns two lists, the first being a list of letters including non-alphanumeric characters and sentence delimiters and prefixed with two &lts&lt tag and &lte&gt tags. The second data structure entails of a set of unique letters that is present in the entire training corpus X.

Function b. The parameters returned by previous function namely a() are both passed as function parameters u and p respectively . 
The list of characters is parsed and consonants are excluded from further processing. In the presence of a vowel, the 2 preceeding characters and the 2 successive characters are noted as passed to function g() for further processing. Two datastructures are returned:
  a. Vectors comprising of the 4 characters.Since a total of 4 characters are taken into account, that is the two preceeding and the two successive, the resulting vector has a shape (1,4*len(X)). As n vowels is inferred within the text, then the shape of the data structure is: (n, (1,4*len(X)))
  b. The indices of the vowels. The actual vowels found within the newspaper text are not passed to the data model but rather these are encoded as the positions of the respective vowel within the vowels array. The resulting array comprising of n vowels, having a shape of (n,1) represents the target/independent feature.  

Function g. Accepts two parameters - a vowel and a list of unique characters that was inferred by function a(). This routine is reponsible for the vectorisation of the vowels/characters. A numpy array having the length of the entire corpus (length of X) is created and the position of the respective vowel is essentially marked. 

The function of the argparse module is simply to read the parameters specified when the train.py is invoked from the shell. In this case a total of 
4 arguments are to be expected to be specified. 

Type usage: train.py [-h] [--k K] [--r R] m h

The definition of the command-line parameters follows below:
a. Parameter --k n. Optional. n must be of Integer type having a default value of 200 in case the parameter is omitted.
b. Parameter --r n. Optional. n must be of Integer type having a default value of 100 in case the parameter is omitted.
c. Parameter m. Name of file. Parameter is mandatory and its value must be of String type. 
d. Parameter h. Mandatory. Denotes the file name of the trained model which is serialised in binary format (pickled) to local disk. The name of the output file must be a value of String type.

## Part 2
The eval.py file implements the functionality to evaluate the performance of the trained model on unseen data. Similar to the training procedure, the python file can be executed from the command-line and supports various parameters. The basic usage is as follow - eval.py [-h] [--cmfile CMFILE] [--v V] h m whereby
a. Parameter -cmfile CMFILE. Optional. The file name of the confusion matrix graphic. This defaults to confusion_matrix.jpg
b. Parameter m and h. See above.

An overview of the routines implemented in this file
read_command_line_params(). Reads the command-line parameters that were supplied to the script.
load_model(). Reads the trained and serialised PyTorch model. The model includes the n-layer neural network, the weights which were inferred during forward and backward propogation. The class stores other properties such as the vocabulary/corpus pertaining to the data model. 
load_test_data(). Reads the newspapers articles from the test data file, builds a unique set of letters that represents the vocabulary and tokenises each letter.  
vectoriser(). Parses each token, take note of the 2 preceeding and 2 successive characters and builds a vector. These vectors represent the predictive features whilst the actual vowel represents the target variable. It is important to note that the vocabulary inferred during model building is supplied to the routine so that vectorisation solely occurs on the known set of characters. 
predict(). Feeds the vectors to the neural network and performs multi-class classification.
replace_vowels_test_text(). Replaces the vowels in the test data with those predicted.

<table border="0" cellpadding="0" cellspacing="0" width="488" style="border-collapse:
 collapse;table-layout:fixed;width:366pt">
 <colgroup><col width="67" style="mso-width-source:userset;mso-width-alt:2133;width:50pt">
 <col width="108" style="mso-width-source:userset;mso-width-alt:3456;width:81pt">
 <col width="81" style="mso-width-source:userset;mso-width-alt:2602;width:61pt">
 <col width="119" style="mso-width-source:userset;mso-width-alt:3797;width:89pt">
 <col width="113" style="mso-width-source:userset;mso-width-alt:3626;width:85pt">
 </colgroup><tbody><tr height="21" style="height:16.0pt">
  <td height="21" class="xl65" width="67" style="height:16.0pt;width:50pt;font-size:
  12.0pt;color:white;font-weight:700;text-decoration:none;text-underline-style:
  none;text-line-through:none;font-family:Calibri, sans-serif;border-top:.5pt solid black;
  border-right:none;border-bottom:none;border-left:.5pt solid black;background:
  black;mso-pattern:black none">run ID</td>
  <td class="xl65" width="108" style="width:81pt;font-size:12.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none;background:black;mso-pattern:black none">hidden
  units</td>
  <td class="xl65" width="81" style="width:61pt;font-size:12.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none;background:black;mso-pattern:black none">epochs</td>
  <td class="xl65" width="119" style="width:89pt;font-size:12.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none;background:black;mso-pattern:black none">train
  accuracy</td>
  <td class="xl65" width="113" style="width:85pt;font-size:12.0pt;color:white;
  font-weight:700;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none;background:black;
  mso-pattern:black none">test accuracy</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">1</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">25</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.314</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.314</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">2</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">50</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.216</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.215</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">3</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.154</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.153</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">4</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">150</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.380</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.377</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">5</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">200</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.352</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.354</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">6</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">300</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.396</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.395</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">7</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">20</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">200</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.181</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.181</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">8</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">50</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">200</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.383</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.383</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">9</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">100</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">200</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.386</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.385</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:.5pt solid black">10</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">150</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">200</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:none;border-left:none">0.400</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:none;border-left:none">0.399</td>
 </tr>
 <tr height="21" style="height:16.0pt">
  <td height="21" align="right" style="height:16.0pt;font-size:12.0pt;color:black;
  font-weight:400;text-decoration:none;text-underline-style:none;text-line-through:
  none;font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:.5pt solid black;border-left:.5pt solid black">11</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:.5pt solid black;border-left:none">200</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:.5pt solid black;border-left:none">200</td>
  <td class="xl66" align="right" style="font-size:12.0pt;color:black;font-weight:
  400;text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  none;border-bottom:.5pt solid black;border-left:none">0.150</td>
  <td align="right" style="font-size:12.0pt;color:black;font-weight:400;
  text-decoration:none;text-underline-style:none;text-line-through:none;
  font-family:Calibri, sans-serif;border-top:.5pt solid black;border-right:
  .5pt solid black;border-bottom:.5pt solid black;border-left:none">0.148</td>
 </tr>
 <tr height="0" style="display:none">
  <td width="67" style="width:50pt"></td>
  <td width="108" style="width:81pt"></td>
  <td width="81" style="width:61pt"></td>
  <td width="119" style="width:89pt"></td>
  <td width="113" style="width:85pt"></td>
 </tr>
 <!--[endif]-->
</tbody></table>




There are a number of advantages of using log softmax over softmax including practical reasons like improved numerical performance and gradient optimization. These advantages can be extremely important for implementation especially when training a model can be computationally challenging and expensive. At the heart of using log-softmax over softmax is the use of log probabilities over probabilities, which has nice information theoretic interpretations.
## Part 3

## Bonuses

## Other notes
