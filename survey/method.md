# Method Overview

![研究部分-研究方法v2](/Users/bugfrog/Desktop/typora_images/研究部分-研究方法v2.png)

## Practical Research

The research process of RQ1 can be divided into two sub-tasks, one is to compare the syntax of existing mainstream smart contract languages, and the other is to analyze the data sets of the industry smart contract.

Firstly, we extracted the syntax definitions of 11 smart contract development languages in terms of keywords, data types, built-in variables, and built-in functions from the relevant academic papers, official documents, and community tutorials. Secondly, we extracted some data items reflecting developers' practice habits from the dimensions of projects and files in industry data sets such as official examples, GitHub, and industrial contract repositories.  

Through these data extraction, we compare and analyze the syntax definitions of keywords, data types, built-in variables, and built-in functions necessary for the smart contract domain in 11 languages. Based on the data set, we summarize the practice habits of developers and the need to optimize the current smart contract-related API.

## Systematic Literature Review

For RQ2 and RQ3, the selection criteria of the literature include the following three aspects: the literature is from authoritative databases, including ACM Digital Library, IEEE Xplore Digital Library, Springer, ScienceDirect, and Google Scholar, The literature was published no earlier than 2016. For RQ2, the literature topics are related to at least one of Java smart contract security, smart contract security, and Java security. And for RQ3, the literature topics are related to at least one of smart contract porting and language translation.

According to the research question and the above literature topics, we designed the search keywords and composed the search string, i.e., (“Smart Contract” OR Java) AND (Vulnerability OR Defect OR Risk OR Security OR Threat OR Attack OR Portability OR Translator). An initial collection of literature was collected by searching relevant research literature in five databases by searching strings and removing duplicates. The literature that does not meet the selection criteria is eliminated by a quick reading of the title, keywords, and abstract. Finally, to expand the literature collection, we read the full text in detail and performed a simultaneous snowballing step, including rolling forward and rolling backward. After obtaining the final literature collection, this paper first performs data extraction of the literature. To answer RQ2, the types and causes of smart contract vulnerabilities covered in the literature were extracted, along with the corresponding preventive measures. To answer RQ3, the principles of smart contract porting methods involved in the literature and their advantages and disadvantages were extracted. Meanwhile, the result selection was completed in the process of data extraction. Finally, we sorted out 13 common platform-independent vulnerabilities in Java smart contracts and three types of smart contract porting methods.  