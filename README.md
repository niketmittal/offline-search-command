# offline-search-command

We have operating systems and computer network core subjects this semester(4th) and we need to learn different types of commands and use different operating systems like Ubuntu, PintOS, etc. There are many other different Linux operating systems like centOS, MintOS, etc, and then there is Windows operating system that uses a command prompt. Now, what I learned is that different shells in different operating systems have different commands that give the exact same result. I had to google everytime I needed to check for an equivalent of a command in windows. For example, 'cd' does the exact same thing in windows command prompt that is done by 'pwd' in ubuntu's terminal i.e, it prints the location of current/working directory.

Idea is to create a universal-command which can be run on any operating system that returns the command for the given operating system the user is currently working on. If any linux user(such as myself) needs to look for a command-equivalent in any other operating system, I would just go and google it but what if I can do such simple task in the command prompt or the terminal itself? This will definitely be much more efficient and save time.

Domain(s) of Study : Posting Lists/Inverted Index creation(includes tokenization, stemming, normalisation and stop words), query engine, parsing and ranking

Basic Components :
	- Parsing
		- Tokenization
		- Stop Word Removal
		- Stemming
	- Posting List / Inverted Index Creation
	- Ranking


Why we are making an inverted index, and not a normal index ?
	In our query we will be given words and we have to return, a meaningful command(depending on the operating system used) related to those words. So documentID should not be a key instead every word should be taken as a key and see other properties. That’s why we have to form an inverted index. In case of normal index it will take O(n) time but in inverted index it will take O(1) time.
	Normal index:  documentID -> data
	Inverted index:  data -> documentID
	A good example of inverted index is the index present at the end of book where we search with respect to words.

  My index is in the form of default dictionary where each element is a list. This element will contain information related to that word in a particular document. Relating to every document their will be a list. Hence in index, number of rows will be total vocabulary of manual/help pages and number of columns will be number of manual/help pages.


Default Dictionary special property :
	Usually, a Python dictionary throws a KeyError if you try to get an item with a key that is not currently in the dictionary. The defaultdict in contrast will simply create any items that you try to access (provided of course they do not exist yet). To create such a "default" item, it calls the function object that you pass in the constructor (more precisely, it's an arbitrary "callable" object, which includes function and type objects). For the first example, default items are created using int(), which will return the integer object 0. For the second example, default items are created using list(), which returns a new empty list object.
	
  E.g.)   somedict = {} 
	        print(somedict[3])                                 # KeyError 
          
	        somedict = defaultdict(int) 
	        print(someddict[3])                                # print int(), thus 0 


Query Processing :
	- Convert everything to Lowercase.
	- Now we have to tokenize the query. We do it using regex “\d+ | [\w]+”. It gives list of tokenized words. Like “Email : niket.mittal-00@gmail.com”, we get ['Email', 'niket', 'mittal', '00', 'gmail', 'com']. Its takes only alphanumeric data and removes all the spaces,':','.','-','@'.
	- Then we remove the stop words. Stop words are ‘is’, ’and’, ’the’ etc. which do not add any importance in our information and their removal also reduces our size of data. Stop words are already present in a file provided by Standford. We put these stop words in a default dictionary and make every element(key) value as 1 so that when we have to search this default dictionary it returns 0 when that query word is not in stop list while removing stop words. In case of simple dictionary if word is not found it will return KeyError instead of zero.
	- Now we do the Stemming. It is the process of finding the root word like 'care' for 'cares', 'caring' etc. If we will not do this then 'care', 'cares' and 'caring' will be treated as different words which will not make any sense. This reduces our size of data to handle. We will use the famous Porter Stemmer.
