# keygraph

== HOW TO RUN=========================

Command:
java topicDetection/Main inputFile  outputFile configFile

The main file is topicDetection/Main.java. It takes three arguments as input:

1- inputFile: 
	- If inputFile is a directory, all documents in that directory will be loaded.
	- If inputFile is a file, there should be one document per line. Each line should have two columns separated with a tab character. The first column is the document id and the second column is the content of the document.
	* If you want to provide your own feature vector, you can use the first input option (input is a directory). When we process the documents in the directory, if a document file name is ending with ".keywords", it will be considered as a feature vector file (Look input format section). Any other extensions will be considered as text files.

2- outputFile: This is the name of the output file which will contains the topics, topics features and assigned documents

3- configFile: This is the name of the file that specifies the parameters of KeyGraph algorithm (Look config file section).   

=== INPUT FORMAT =====================

KeyGraph accepts two types of input: Text and Keyword files. Keyword files can be used if you need to perform your own feature extraction. The type of input file/document will be determined from its extension.   
1-Keyword file: if a document file name is ending with ".keywords", it will be considered as a keyword file. Each line in keyword file includes one keyword in the following format:
keyword==term_frequency

2-Text file: if a document file name is NOT ending with ".keywords", it will be considered as a text file. A text file includes the content of the document.

=== OUTPUT FORMAT ====================

Each topic is presented in 4 lines. Each topic is separated from the next topic with a empty line:
-First line includes the list of keywords
-Second line includes the list of documents
-Third and fourth line includes the list of nodes and edges representing the topic features, keywords co-occurrence:
	Third line includes the nodes each of which is represented as node_id:node_labelBaseForm:node_label. Both node_id and node_labelBaseForm are unique.   
	Fourth line includes the edges each of which is represented as node1_id:node1_labelBaseForm-node2_id:node2_labelBaseForm

Here is a synthetic output file: 
 
KEYWORDS:	keyword1,keyword2,...
Documents:	document1,document2,...
KEYGRAPH_NODES:	node1_id:node1_labelBaseForm:node1_label,node2_id:node2_labelBaseForm:node2_label,...
KEYGRAPH_EDGES:	node1_id:node1_labelBaseForm:node1_label-node2_id:node2_labelBaseForm,node3_labelBaseForm:node3_label-node4_id:node4_labelBaseForm,...

KEYWORDS:	keyword1,keyword2,...
Documents:	document1,document2,...
KEYGRAPH_NODES:	node1_id:node1_labelBaseForm:node1_label,node2_id:node2_labelBaseForm:node2_label,...
KEYGRAPH_EDGES:	node1_id:node1_labelBaseForm:node1_label-node2_id:node2_labelBaseForm,node3_labelBaseForm:node3_label-node4_id:node4_labelBaseForm,...

KEYWORDS:	keyword1,keyword2,...
Documents:	document1,document2,...
KEYGRAPH_NODES:	node1_id:node1_labelBaseForm:node1_label,node2_id:node2_labelBaseForm:node2_label,...
KEYGRAPH_EDGES:	node1_id:node1_labelBaseForm:node1_label-node2_id:node2_labelBaseForm,node3_labelBaseForm:node3_label-node4_id:node4_labelBaseForm,...


=== CONFIG FILE ======================

Config file is located under config directory. We also provided 3 recommended config files for Blogs, news, and tweets.

//*input documents and features --------
TEXT_WEIGHT = 1;
KEYWORDS_WEIGHT = 1;

//*KeyGraph Settings -----------------
HARD_CLUSTERING = false; // KeyGraph by default does soft clustering by allowing documents to be assigned to multiple topics. By setting this parameter to 'true' KeyGraph will not assign a document to multiple topics.

REMOVE_DUPLICATES = false; // This parameter is used to filter duplicated documents in topic detection process. We strongly suggests to set this parameter to "true" for tweets. Note that this will not remove the duplicated documents from output. They will only be ignored in generating KeyGraph which helps to improve the quality of topics extracted.  

NODE_DF_MIN = 7; // This parameter is used to filter rare words from the KeyGraph. Its value is the minimum document frequency.
NODE_DF_MAX = .084; // This parameter is used to filter common words from the KeyGraph. Its value is maximum document frequency specified in percentage.

EDGE_CORRELATION_MIN = .15434; // This parameter is used to filter rare links from the KeyGraph. Its value is the minimum correlation between the corresponding words.
EDGE_DF_MIN = 5; // This parameter is used to filter rare links from the KeyGraph. Its value is the minimum document frequency.
EDGE_CP_MIN_TO_DUPLICATE = .70; //***NOT USED***

DOC_KEYWORDS_SIZE_MIN = 4; // This parameter is used to filter small documents from the dataset. Its value is the minimum number of keywords for a document.
DOC_SIM2KEYGRAPH_MIN = .3524; // This parameter is used to assign documents to topics. Its value specifies the minimum cosine similarly between a document and the topic feature.

CLUSTER_NODE_SIZE_MAX = 500;
CLUSTER_NODE_SIZE_MIN = 5;
CLUSTER_INTERSECT_MIN = .15;

TOPIC_MIN_SIZE = 5; 

== IGNORE THESE PROPERTIES =================

KEYWORDS_1_ENABLE = true; //***NOT USED***
KEYWORDS_2_ENABLE = true; //***NOT USED***
TEXT_ENABLE = true; //***NOT USED***
KEYWORDS_2_WEIGHT = 1; //***NOT USED***
DATA_KEYWORDS_1_PATH = data/keywords_1/; //***NOT USED***
DATA_KEYWORDS_2_PATH = data/keywords_2/; //***NOT USED***
DATA_TEXT_PATH = data/text/; //***NOT USED***
DATA_TOPIC_PATH = data/topic/; //***NOT USED***
DATA_DATE_PATH = data/date; //***NOT USED***
CLUSTERING_ALG= betweenness; 
TOPIC_MAX = 1000; //***NOT USED***
CLUSTER_VAR_MAX = 1.99; //***NOT USED***
DOC_CHAR_SIZE_MAX = 2000; //***NOT USED***
DOC_SIM2CENTROID_MIN = 0.00; //***NOT USED***
CENTROID_KEYWORD_DF_MIN = 3; //***NOT USED***
SIMILARITY_KEYWORD_DF_MIN = 5; //***NOT USED***
