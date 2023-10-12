---
layout: default
---
 <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 



# Regular Expressions, Text Normalization, Edit Distance


```
User: I am unhappy.
ELIZA: DO YOU THINK COMING HERE WILL HELP YOU NOT TO BE UNHAPPY
User: I need some help, that much seems certain.
ELIZA: WHAT WOULD IT MEAN TO YOU IF YOU GOT SOME HELP
User: Perhaps I could learn to get along with my mother.
ELIZA: TELL ME MORE ABOUT YOUR FAMILY
User: My mother takes care of me.
ELIZA: WHO ELSE IN YOU FAMILY TAKES CARE OF YOU
User: My father.
ELIZA: YOUR FATHER
User: You are like my father in some ways.
```

The dialogue above is from ELIZA, an early natural language processing system that could carry on a limited conversation with a user by imitating the responses of a Rogerian psychotherapist. ELIZA is a surprisingly simple program that uses pattern matching to recognize phrases like “I need X” and translate them into suitable outputs like “What would it mean to you if you got X?”. This simple technique succeeds in this domain because ELIZA doesn’t actually need to know anything to mimic a Rogerian psychotherapist.

The modern conversational agents can answer questions, book flights, or find restaurants, functions for which they rely on a much more sophisticated understanding of the user’s intent. We’ll begin with the most important tool for describing text patterns: the **regular expression**. Regular expressions can be used to specify strings we might want to extract from a document, from transforming “I need X” in ELIZA above, to defining strings like ₹199 or ₹24.99 for extracting tables of prices from a document.

We’ll then turn to a set of tasks collectively called **text normalization**, in which regular expressions play an important part. Normalizing text means converting it to a more convenient, standard form. Three major ways for text normalization :

1. **Tokenization** - Most of what we are going to do with language relies on first separating out or tokenizing words from running text, the task of tokenization. For example "New York" and "rock ’n’ roll" are sometimes treated as large words despite the fact that they contain spaces. "I’m" into two words I and am.
2. **Lemmatization** - The task of determining that two words have the same root, despite their surface differences. For example, the words sang, sung, and sings are forms of the verb sing.
3. **Stemming** - It refers to a simpler version of lemmatization in which we mainly just strip suffixes from the end of the word.

## Regular Expression

Regular expressions are particularly useful for searching in texts, when we have a pattern to search for and a corpus of texts to search through. A regular expression search function will search through the corpus, returning all texts that match the pattern. The corpus can be a single document or a collection.

### Basic Regular Expression Patterns

The simplest kind of regular expression is a sequence of simple characters; putting characters in sequence is called concatenation. To search for woodchuck, we type `/woodchuck/`. Regular expressions are case sensitive. We can solve this problem with the use of the square braces [ and ]. The string of characters inside the braces specifies a disjunction of characters to match. For example, the pattern `/[wW]/` matches patterns containing either w or W. In cases where there is a well-defined sequence associated with a set of characters, the brackets can be used with the dash (-) to specify range any one character in a range. The pattern `/[2-5]/` specifies any one of the characters 2, 3, 4, or 5. The pattern `/[b-g]/` specifies one of the characters b, c, d, e, f, or g. The square braces can also be used to specify what a single character cannot be, by use of the caret ˆ. If the caret ˆ is the first symbol after the open square brace [, the resulting pattern is negated. For example, the pattern `/[ˆa]/` matches any single character (including special characters) except a. For optional things we use the question mark `/?/`, which means “the preceding character or nothing”.

| Regex        | Match          | Example Pattern |
|:-------------|:------------------|:------|
| `/woodchucks/`           | woodchuck | interesting links to **woodchucks** and lemurs |
| `/[wW]oodchuck/` | Woodchuck or woodchuck | **Woodchuck** |
| `/[abc]/` | ‘a’, ‘b’, or ‘c’ | In uomini, in sold**a**ti |
| `/[a-z]/` | a lower case letter | **m**y beans were impatient to be hoed! |
| `/[ˆA-Z]/` | not an upper case letter | O**y**fn pripetchik |
| `/woodchucks?/` | woodchuck or woodchucks | **woodchuck** |

The set of operators that allows us to say things like “some number of a's” are based on the asterisk or \*, commonly called the Kleene\* . The Kleene star means “zero or more occurrences of the immediately previous character or regular expression”. So `/a*/` means “any string of zero or more a's”. This will match a or aaaaaa, but it will also match the empty string at the start of **Off Minor** since the string Off Minor starts with zero a’s. So the regular expression for matching one or more a is /aa*/, meaning one a followed by zero or more a's. Sometimes it’s annoying to have to write the regular expression for digits twice, so there is a shorter way to specify “at least one” of some character. This is the Kleene +, which means “one or more occurrences of the immediately preceding character or regular expression”. Thus, the expression `/[0-9]+/` is the normal way to specify “a sequence of digits”.

One very important special character is the period (/./), a wildcard expression that matches any single character. The wildcard is often used together with the Kleene star to mean “any string of characters”. For example, suppose we want to find any line in which a particular word, for example, aardvark, appears twice. We can specify this with the regular expression `/aardvark.*aardvark/`.

Anchors are special characters that anchor regular expressions to particular places in a string. The caret ˆ matches the start of a line. The pattern `/ˆThe/` matches the word The only at the start of a line. The dollar sign `$` matches the end of a line. So the pattern `$` is a useful pattern for matching a space at the end of a line, and `/ˆThe dog\.$/` matches a line that contains only the phrase The dog. There are also two other anchors: `\b` matches a word boundary, and `\B` matches a non-boundary. Thus, `/\bthe\b/` matches the word the but not the word other.

### Disjunction, Grouping, and Precedence

We might want to search for either the string cat or the string dog. We need a new operator, the disjunction operator, also called the pipe symbol `|`. The pattern `/cat|dog/` matches either the string cat or the string dog. To make the disjunction operator apply only to a specific pattern, we need to use the parenthesis operators ( and ). Enclosing a pattern in parentheses makes it act like a single character for the purposes of neighboring operators like the pipe `|` and the Kleene\*. So the pattern `/gupp(y|ies)/` would specify that we meant the disjunction only to apply to the suffixes **y** and **ies**. With the parentheses, we could write the expression `/(Column [0-9]+ *)*/` to match the word Column, followed by a number and optional spaces, the whole pattern repeated zero or more times. 

The following table gives the order operator precedence of RE operator precedence, from highest precedence to lowest precedence.

| Name       | Icon          |
|:-------------|:------------------|
| Parenthesis           | () | 
| Counters           | * + ? {} | 
| Sequences and anchors           | the ^my end$ | 
| Disjunction           | `|` | 

Regular expressions always match the largest string they can; we say that patterns are greedy, expanding to cover as much of a string as they can. There are, however, ways to enforce non-greedy matching, using another meaning of the ? qualifier. The operator *? is a Kleene star that matches as little text as possible. The operator +? is a Kleene plus that matches as little text as possible.

### A Simple Example

Suppose we wanted to write a RE to find cases of the English article **the**. A simple (but incorrect) pattern might be `/the/`. One problem is that this pattern will miss the word when it begins a sentence and hence is capitalized (i.e., The). This might lead us to the following pattern `/[tT]he/`. But we will still incorrectly return texts with the embedded in other words. So we need to specify that we want instances with a word boundary on both sides `/\b[tT]he\b/`.

Suppose we wanted to do this without the use of /\b/. We need to specify that we want instances in which there are no alphabetic letters on either side of the the `/[ˆa-zA-Z][tT]he[ˆa-zA-Z]/`.  But there is still one more problem with this pattern: it won’t find the word the when it begins a line. This is because the regular expression [ˆa-zA-Z], which we used to avoid embedded instances of the, implies that there must be some single character before the the. We can avoid this by specifying that before the the we require either the beginning-of-line or a non-alphabetic character, and the same at the end of the line:
`/(ˆ|[ˆa-zA-Z])[tT]he([ˆa-zA-Z]|$)/`.

The process we just went through was based on fixing two kinds of errors: false positives, strings that we incorrectly matched like other or there, and false negatives, strings that we incorrectly missed, like The. Addressing these two kinds of errors comes up again and again in implementing speech and language processing systems. Reducing the overall error rate for an application thus involves two antagonistic efforts:
1. Increasing precision (minimizing false positives)
2. Increasing recall (minimizing false negatives)

### More Operators

Besides the Kleene* and Kleene+ we can also use explicit numbers as counters, by enclosing them in curly brackets. The regular expression `/{3}/` means “exactly 3 occurrences of the previous character or expression”. So `/a\.{24}z/` will match a followed by 24 dots followed by z.

| Regex       | Match          |
|:-------------|:------------------|
| \d |  any digit |
| \D |  any non-digit |
| \w |   any alphanumeric/underscore |
| \W |   a non-alphanumeric |
| \s |   whitespace (space, tab) |
| \S |  Non-whitespace |
| * | zero or more occurrences of the previous char or expression | 
| + | one or more occurrences of the previous char or expression |
| ? | zero or one occurrence of the previous char or expression |
| {n} | exactly n occurrences of the previous char or expression |
| {n,m} | from n to m occurrences of the previous char or expression |
| {n,} | at least n occurrences of the previous char or expression |
| {,m} | up to m occurrences of the previous char or expression |
| \* | an asterisk |
| \. | a period “.” |
| \? | a question mark |
| \n | a newline |
| \t | a tab |

### Substitution, Capture Groups, and ELIZA

An important use of regular expressions is in substitutions. It is often useful to be able to refer to a particular subpart of the string matching the first pattern. For example, suppose we are looking for the pattern “the Xer they were, the Xer they will be”, where we want to constrain the two X’s to be the same string. We do this by surrounding the first X with the parenthesis operator, and replacing the second X with the number operator \1, as follows `/the (.*)er they were, the \1er they will be/`. Here the \1 will be replaced by whatever string matched the first item in parentheses. So this will match "the bigger they were, the bigger they will be" but not "the bigger they were, the faster they will be".

This use of parentheses to store a pattern in memory is called a capture group. Every time a capture group is used (i.e., parentheses surround a pattern), the resulting match is stored in a numbered register. If you match two different sets of parentheses, \2 means whatever matched the second capture group. Occasionally we might want to use parentheses for grouping, but don’t want to capture the resulting pattern in a register. In that case we use a non-capturing group, which is specified by putting the special non-capturing group commands ?: after the open parenthesis, in the form (?: pattern ). `/(?:some|a few) (people|cats) like some \1/` will match "some cats like some cats" but not "some cats like some some".

ELIZA works by having a series or cascade of regular expression substitutions each of which matches and changes some part of the input lines. Input lines are first uppercased. The first substitutions then change all instances of MY to YOUR, and I’M to YOU ARE, and so on. The next set of substitutions matches and replaces other patterns in the input. Here are some examples:

```
s/.* I’M (depressed|sad) .*/I AM SORRY TO HEAR YOU ARE \1/
s/.* I AM (depressed|sad) .*/WHY DO YOU THINK YOU ARE \1/
s/.* all .*/IN WHAT WAY/
s/.* always .*/CAN YOU THINK OF A SPECIFIC EXAMPLE/
```

## Words

Before we talk about processing words, we need to decide what counts as a word. Let’s start by looking at one particular corpus (plural corpora), a computer-readable collection of text or speech. How many words are in the following Brown Corpus sentence?

```
He stepped out into the hall, was delighted to encounter a water brother.
```
> This sentence has 13 words if we don’t count punctuation marks as words, 15 if we count punctuation.

The Switchboard corpus of American English telephone conversations between strangers was collected in the early 1990s; it contains 2430 conversations averaging 6 minutes each, totaling 240 hours of speech and about 3 million words. Such corpora of spoken language don’t have punctuation but do introduce other complications with regard to defining words. Let’s look at one utterance from Switchboard; an utterance is the spoken correlate of a sentence :

```
I do uh main- mainly business data processing
```

This utterance has two kinds of disfluencies. The broken-off word main- is called a fragment. Words like uh and um are called fillers or filled pauses. Should we consider these to be words? It depends on the application.

How about inflected forms like cats versus cat? These two words have the same lemma cat but are different wordforms. A lemma is a set of lexical forms having the same stem, the same major part-of-speech, and the same word sense. The wordform is the full inflected or derived form of the word.

**Types** are the number of distinct words in a corpus; if the set of words in the vocabulary is V, the number of types is the word token vocabulary size `|V|`. **Tokens** are the total number N of running words. If we ignore punctuation, the following Brown sentence has 16 tokens and 14 types :

```
They picnicked by the pool, then lay back on the grass and looked at the stars.
```

The larger the corpora we look at, the more word types we find, and in fact this relationship between the number of types `|V|` and number of tokens N is called **Herdan’s Law or Heaps’ Law** after its discoverers. It is shown where k and β are positive constants, and 0 < β < 1.

> `|V|` = kN<sup>β</sup>

## Corpora

Words don’t appear out of nowhere. Any particular piece of text that we study is produced by one or more specific speakers or writers, in a specific dialect of a specific language, at a specific time, in a specific place, for a specific function. Another dimension of variation is the genre. The text that our algorithms must process might come from newswire, fiction or non-fiction books, scientific articles, Wikipedia, or religious texts. Because language is so situated, when developing computational models for language processing from a corpus, it’s important to consider who produced the language, in what context, for what purpose. The best way is for the corpus creator to build a datasheet or data statement for each corpus. A datasheet specifies properties of a dataset like :

1. **Motivation** : Why was the corpus collected, by whom, and who funded it?
2. **Situation** : When and in what situation was the text written/spoken? 
3. **Language variety** : What language (including dialect/region) was the corpus in?
4. **Speaker demographics** : What was, e.g., the age or gender of the text’s authors?
5. **Collection process** : How big is the data? If it is a subsample how was it sampled? Was the data collected with consent? How was the data pre-processed, and what metadata is available?
6. **Annotation process** : What are the annotations, what are the demographics of the annotators, how were they trained, how was the data annotated?
7. **Distribution** : Are there copyright or other intellectual property restrictions?

## Text Normalization

Before almost any natural language processing of a text, the text has to be normalized. At least three tasks are commonly applied as part of any normalization process :

1. Tokenizing (segmenting) words
2. Normalizing word formats
3. Segmenting sentences

### Word Tokenization

For most NLP application we'll need to keep all the numbers and punctuation in our tokenization. We often want to break off punctuation as a separate token; commas are a useful piece of information for parsers, periods help indicate sentence boundaries. But we’ll often want to keep the punctuation that occurs word internally, in examples like m.p.h., Ph.D., AT&T, and cap’n. Special characters and numbers will need to be kept in prices ($45.55) and dates (01/02/06); we don’t want to segment that price into separate tokens of “45” and “55”. And there are URLs, Twitter hashtags, or email addresses.

A tokenizer can also be used to expand clitic contractions that are marked by apostrophes, for example, converting what’re to the two tokens what are, and we’re to we are. A clitic is a part of a word that can’t stand on its own, and can only occur when it is attached to another word. Tokenization is thus intimately tied up with **named entity recognition**, the task of detecting names, dates, and organizations.

One commonly used tokenization standard is known as the Penn Treebank tokenization standard, used for the parsed corpora (treebanks) released by the Linguistic Data Consortium (LDC), the source of many useful datasets. This standard separates out clitics (doesn’t becomes does plus n’t), keeps hyphenated words together, and separates out all punctuation :

```
Input: "The San Francisco-based restaurant," they said, "doesn’t charge $10".
```

> Output: "\_The_San_Francisco-based_restaurant_,\_"\_they_said\_,\_"\_does_n’t_charge\_$\_10_"\_.

In practice, since tokenization needs to be run before any other language processing, it needs to be very fast. The standard method for tokenization is therefore to use deterministic algorithms based on regular expressions compiled into very efficient finite state automata. Example python code :

```python
>>> text = 'That U.S.A. poster-print costs $12.40...'
>>> pattern = r'''(?x) 			# set flag to allow verbose regexps
... (?:[A-Z]\.)+ 				# abbreviations, e.g. U.S.A.
... | \w+?:(-\w+)* 				# words with optional internal hyphens
... | \$?\d+(?:\.\d+)?%? 		# currency, percentages, e.g. $12.40, 82%
... | \.\.\. 					# ellipsis
... | [][.,;"'?():_`-] 			# these are separate tokens; includes ], [
... '''
>>> nltk.regexp_tokenize(text, pattern)
```

> ['That', 'U.S.A.', 'poster-print', 'costs', '$12.40', '...']

Word tokenization is more complex in languages like written Chinese, Japanese, and Thai, which do not use spaces to mark potential word-boundaries. In Chinese, for example, words are composed of characters. In fact, for most Chinese NLP tasks it turns out to work better to take characters rather than words as input, since characters are at a reasonable semantic level for most applications, and since most word standards, by contrast, result in a huge vocabulary with large numbers of very rare words.

### Byte-Pair Encoding for Tokenization

There is a third option to tokenizing text. Instead of defining tokens as words, or as characters, we can use our data to automatically tell us what the tokens should be. This is especially useful in dealing with unknown words, an important problem in language processing. To deal with this unknown word problem, modern tokenizers often automatically induce sets of tokens that include tokens smaller than words, called subwords. Subwords can be arbitrary substrings, or they can be meaning-bearing units like the morphemes -est or -er. Every unseen word like lower can thus be represented by some sequence of known subword units, such as low and er, or even as a sequence of individual letters if necessary.

Most tokenization schemes have two parts: a token learner, and a token segmenter. The token learner takes a raw training corpus and induces a vocabulary, a set of tokens. The token segmenter takes a raw test sentence and segments it into the tokens in the vocabulary.

The Byte-pair Encoding algo (BPE) - The BPE token learner begins with a vocabulary that is just the set of all individual characters. It then examines the training corpus, chooses the two symbols that are most frequently adjacent (say ‘A’, ‘B’), adds a new merged symbol ‘AB’ to the vocabulary, and replaces every adjacent ’A’ ’B’ in the corpus with the new ‘AB’. It continues to count and merge, creating new longer and longer character strings, until k merges have been done creating k novel tokens; k is thus a parameter of the algorithm. The resulting vocabulary consists of the original set of characters plus k new symbols.  Let’s see its operation on the following tiny input corpus of 18 word tokens with counts for each word (the word low appears 5 times, the word newer 6 times, and so on), which would have a starting vocabulary of 11 letters :

| corpus       | vocabulary          |
|:-------------|:------------------|
| 5 l o w _  | _ , d, e, i, l, n, o, r, s, t, w |
| 2 l o w e s t _ | |
| 6 n e w e r _ | |
| 3 w i d e r _ | |
| 2 n e w _ | |

The BPE algorithm first counts all pairs of adjacent symbols: the most frequent is the pair e r because it occurs in newer (frequency of 6) and wider (frequency of 3) for a total of 9 occurrences. We then merge these symbols, treating er as one symbol, and count again :

| corpus       | vocabulary          |
|:-------------|:------------------|
| 5 l o w _  | _ , d, e, i, l, n, o, r, s, t, w, er |
| 2 l o w e s t _ | |
| 6 n e w er _ | |
| 3 w i d er _ | |
| 2 n e w _ | |

Now the most frequent pair is er _ , which we merge; our system has learned that there should be a token for word-final er, represented as er_ :

| corpus       | vocabulary          |
|:-------------|:------------------|
| 5 l o w _ | _ , d, e, i, l, n, o, r, s, t, w, er, er_ |
| 2 l o w e s t _ | |
| 6 n e w er_ | |
| 3 w i d er_ | |
| 2 n e w _ | |

Next n e (total count of 8) get merged to ne :

| corpus       | vocabulary          |
|:-------------|:------------------|
| 5 l o w _ | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne |
| 2 l o w e s t _ | |
| 6 ne w er_ | |
| 3 w i d er_ | |
| 2 ne w _ | |

If we continue, the next merges are: 

| merge       | current vocabulary          |
|:-------------|:------------------|
| (ne, w) | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne, new |
| (l, o) | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne, new, lo |
| (lo, w) | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne, new, lo, low |
| (new, er_) | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne, new, lo, low, newer_ |
| (low, _) | _ , d, e, i, l, n, o, r, s, t, w, er, er_, ne, new, lo, low, newer_, low_ |

Algorithm :

```
function BYTE-PAIR ENCODING(strings C, number of merges k) returns vocab V

V←all unique characters in C 		# initial set of tokens is characters
for i = 1 to k do 					# merge tokens k times
	t_L, t_R ←Most frequent pair of adjacent tokens in C
	t_NEW ←t_L + t_R 					# make new token by concatenating
	V←V + t_NEW 						# update the vocabulary
	Replace each occurrence of t_L, t_R in C with t_NEW 		# and update the corpus
return V
```

### Word Normalization, Lemmatization and Stemming

Word normalization is the task of putting words/tokens in a standard format, choosing a single normal form for words with multiple forms like USA and US or uh-huh and uhhuh. Case folding is another kind of normalization. Mapping everything to lower case means that Woodchuck and woodchuck are represented identically, which is very helpful for generalization in many tasks, such as information retrieval or speech recognition. For sentiment analysis and other text classification tasks, information extraction, and machine translation, by contrast, case can be quite helpful and case folding is generally not done.

Lemmatization is the task of determining that two words have the same root, despite their surface differences. The words am, are, and is have the shared lemma be; the words dinner and dinners both have the lemma dinner. The most sophisticated methods for lemmatization involve complete morphological parsing of the word. Morphology is the study of the way words are built up from smaller meaning-bearing units called morphemes. Two broad classes of morphemes can be distinguished: stems—the central morpheme of the word, supplying the main meaning—and affixes—adding “additional” meanings of various kinds. So, for example, the word fox consists of one morpheme (the morpheme fox) and the word cats consists of two: the morpheme cat and the morpheme -s. 

This naive version of morphological analysis is called stemming. For example, the Porter stemmer, a widely used stemming algorithm. The algorithm is based on series of rewrite rules run in series: the output of each pass is fed as input to the next pass. Here are some sample rules https://tartarus.org/martin/PorterStemmer/ .

Example Paragraph :

```
This was not the map we found in Billy Bones’s chest, but an accurate copy, complete in all things-names and heights and soundings-with the single exception of the red crosses and the written notes.
```

Produces output :

> Thi wa not the map we found in Billi Bone s chest but an accur copi complet in all thing name and height and sound with the singl except of the red cross and the written note

### Sentence Segmentation

Sentence segmentation is another important step in text processing. The most useful cues for segmenting a text into sentences are punctuation, like periods, question marks, and exclamation points. Question marks and exclamation points are relatively unambiguous markers of sentence boundaries. Periods, on the other hand, are more ambiguous. The period character “.” is ambiguous between a sentence boundary marker and a marker of abbreviations like Mr. or Inc. In general, sentence tokenization methods work by first deciding (based on rules or machine learning) whether a period is part of the word or is a sentence-boundary marker.

## Minimum Edit Distance

Much of natural language processing is concerned with measuring how similar two strings are. For example in spelling correction, the user typed some erroneous string—let’s say graffe–and we want to know what the user meant. The user probably intended a word that is similar to graffe. Among candidate similar words, the word giraffe, which differs by only one letter from graffe, seems intuitively to be more similar than, say grail or graf, which differ in more letters. Edit distance gives us a way to quantify both of these intuitions about string similarity. More formally, the minimum edit distance between two strings is defined as the minimum number of editing operations needed to transform one string into another. The gap between intention and execution, for example, is 5 (delete an i, substitute e for n, substitute x for t, insert c, substitute u for n).

```
I N T E * N T I O N
| | | | | | | | | |
* E X E C U T I O N
d s s   i s
```

The Levenshtein distance between two sequences is the simplest weighting factor in which each of the three operations has a cost of 1, we assume that the substitution of a letter for itself, for example, t for t, has zero cost. The Levenshtein distance between intention and execution is 5.

### The Minimum Edit Distance Algorithm

How do we find the minimum edit distance? We can think of this as a search task, in which we are searching for the shortest path—a sequence of edits—from one string to another. We can do this by using dynamic programming. The intuition of a dynamic programming problem is that a large problem can be solved by properly combining the solutions to various subproblems.

Let’s first define the minimum edit distance between two strings. Given two strings, the source string X of length n, and target string Y of length m, we’ll define D[i, j] as the edit distance between X[1..i] and Y[1.. j], i.e., the first i characters of X and the first j characters of Y. The edit distance between X and Y is thus D[n,m]. We’ll use dynamic programming to compute D[n,m] bottom up, combining solutions to subproblems. In the base case, with a source substring of length i but an empty target string, going from i characters to 0 requires i deletes. With a target substring of length j but an empty source going from 0 characters to j characters requires j inserts. Having computed D[i, j] for small i, j we then compute larger D[i, j] based on previously computed smaller values. The value of D[i, j] is computed by taking the minimum of the three possible paths through the matrix which arrive there :

$$ D[i,j] = min 
\begin{cases}
D[i−1, j]+\text{del-cost(source[i])} \\
D[i, j −1] +\text{ins-cost(target[j])} \\
D[i−1, j −1] +\text{sub-cost(source[i],target[j])}
\end{cases}
$$

If we assume the version of Levenshtein distance in which the insertions and deletions each have a cost of 1 (ins-cost(·) = del-cost(·) = 1), and substitutions have a cost of 2, the computation for D[i, j] becomes :

$$ D[i,j] = min 
\begin{cases}
D[i−1, j] +1 \\
D[i, j −1] +1 \\
D[i−1, j −1] + \begin{cases}
2; if source[i] \neq target[j] \\
0; if source[i] = target[ j]
\end{cases}
\end{cases}
$$

The algorithm is :

```
function MIN-EDIT-DISTANCE(source, target) returns min-distance

	n←LENGTH(source)
	m←LENGTH(target)
	Create a distance matrix D[n+1,m+1]
	
	# Initialization: the zeroth row and column is the distance from the empty string
	D[0,0] = 0
	for each row i from 1 to n do
		D[i,0]←D[i-1,0] + del-cost(source[i])
	for each column j from 1 to m do
		D[0,j]←D[0, j-1] + ins-cost(target[j])

	# Recurrence relation:
	for each row i from 1 to n do
		for each column j from 1 to m do
			D[i, j]←MIN( D[i−1, j] + del-cost(source[i]),
						 D[i−1, j−1] + sub-cost(source[i], target[j]),
						 D[i, j−1] + ins-cost(target[j]))

	# Termination
	return D[n,m]
```

