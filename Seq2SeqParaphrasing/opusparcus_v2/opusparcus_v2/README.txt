OPUSPARCUS 1.0

Mathias Creutz, 4 April 2018


1. General

This archive contains the first release of Opusparcus, a paraphrase
corpus for six European languages: German, English, Finnish, French,
Russian, and Swedish.  The paraphrases are extracted from the
OpenSubtitles2016 corpus, which contains subtitles from movies and TV
shows.

The construction of the Opusparcus corpus has been described in the
following conference paper:

Mathias Creutz (2018). Open Subtitles Paraphrase Corpus for Six
Languages. In Proceedings of the 11th edition of the Language
Resources and Evaluation Conference (LREC 2018), 7-12 May, Miyazaki,
Japan.

Please cite the above paper in any work that utilizes any part of the
Opusparcus corpus.

The data in Opusparcus has been extracted from OpenSubtitles2016
(http://opus.nlpl.eu/OpenSubtitles2016.php), which is in turn based on
data from http://www.opensubtitles.org/.


2. License

Opusparcus 1.0 is licensed under the latest version of the Creative
Commons CC BY-NC (Attribution-NonCommercial) license.  You may copy
and redistribute the material in any medium or format, and you may
remix, transform, and build upon the material.  However, you must give
appropriate credit (see instructions above), provide a link to the
license, and indicate if changes were made.  You may not use the
material for commercial purposes.

Read more at: http://urn.fi/urn:nbn:fi:lb-2018021221


3. Data sets

Opusparcus contains training, development and test sets for six
European languages: German (de), English (en), Finnish (fi), French
(fr), Russian (ru), and Swedish (sv).

The training sets are orders of magnitudes larger than the development
and test sets and consist of lists of automatically ranked sentence
pairs, where a high rank means a higher probability that the two
sentences are paraphrases.

The development and test sets consist exclusively of sentence pairs
that have been annotated manually.  This is to guarantee the high
quality of these sets.  However, quality comes at the expense of
quantity, so the development and test sets are smaller than the
training sets.

The development sets can be used to refine whatever training
algorithms one might want to devise.

The test sets should be used in final evaluations only.  It is
important to keep the test sets aside while developing new methods,
since otherwise the test sets may affect the design or parameter
optimization of the methods under development.


4. File formats

4.1 Training sets

The training sets are stored as compressed bzip2 files in the train
directories of each language.  The text is encoded using UTF-8.

Each file consists of lines that contain the following seven tab
separated fields:

Field 1:
Sentence pair ID.  The ID consists of the language ID, followed by a
hyphen, followed by the letter N (for traiN), followed by a number.

Field 2:
First sentence of the sentence pair.  The sentence has been tokenized
and normalized to some degree; for instance, punctuation at the end of
the sentence other than a question mark (?) has been converted to a
single period.

Field 3:
Second sentence of the sentence pair.  The sentence has been tokenized
in the same way as the first sentence.  These two sentences are
potential paraphrases.  The lower the sentence pair number, and the
earlier in the file the sentence pair appears, the higher the
likelihood is that the two sentences are paraphrases.  See Table 2 in
the LREC 2018 paper for an assessment of the quality of the paraphrase
candidates.

Field 4:
This value corresponds to Equation 5 in the LREC paper.  It is the sum
of pointwise-mutual information (PMI) values of the sentence pair,
accumulated over all pivot language corpora.  This is the score that
was utilized to rank the sentence pairs and order them into the file,
highest score first (most likely to be paraphrases), lowest score last
(least likely to be paraphrases).

Field 5:
Expected number of times the two sentences in the sentence pair would
be aligned with each other when translated to the pivot languages and
back.  This is the same as the joint probability in Equation 2 of the
LREC paper, multiplied by the total number of sentence pairs in the
corpus.  That is, instead of indicating a probability, we indicate an
expected frequency here.  This value can be used as an alternative
ranking criterion, as is reported in the paper.  If used for ranking, a
higher value indicates a higher likelihood of being paraphrases.

Field 6:
Number of pivot languages in which the two sentences in this sentence
pair have a translation in common.  The highest possible value is 5,
which means that in all other languages these sentences have a common
translation.  For instance, when the value is 5 for the English
sentence pair "Get up , Sam ." vs. "On your feet , Sam .", this means
that in all five other languages (de, fi, fr, ru, sv) there exists a
single sentence that translates to both of these English sentences.
The lowest possible value is 1, which means that only one other
language directly supports the hypothesis that these two sentences are
paraphrases.

Field 7:
(Adjusted) edit distance between the two sentences in this sentence
pair.  The edit distance is not used in the ranking of the sentences,
but the edit distance can be used as a filter for finding "more
interesting" paraphrases, which do not differ from each other in just
one or a few characters.  This adjusted edit distance is computed
without taking into account the "tails" of the longer of the two
sentences.  For instance, the adjusted edit distance between the
sentences "Frankfurt , Germany ." vs. "Oh , Frankfurt , Germany ." is
zero, because the first shorter sentence fits within the second longer
one without any modifications.


4.2 Development and test sets

The development and test sets are stored as uncompressed plain text
files in the dev and test directories of each language, respectively.
The format of the dev and test files are the same.  The text is
encoded using UTF-8.

Each file consists of lines that contain four tab separated fields, as
follows:

Field 1:
Sentence pair ID.  The ID consists of the language ID, followed by a
hyphen, followed by the letter D or T (D for Development, T for Test),
followed by a number.

Field 2:
First sentence of the sentence pair.  The sentence has been tokenized
and normalized to some degree; for instance, punctuation at the end of
the sentence other than a question mark (?) has been converted to a
single period.

Field 3:
Second sentence of the sentence pair.  The sentence has been tokenized
in the same way as the first sentence.  These two sentences are
potential paraphrases.  Whether they are paraphrases or not is
indicated in the fourth field.

Field 4:
Average score given by two independent annotators.  The scores given
by an individual annotator are: 4) Good example of paraphrases = Dark
green button in the annotation tool, 3) Mostly good example of
paraphrases = Light green button in the annotation tool, 2) Mostly bad
example of paraphrases = Yellow button in the annotation tool, 1) Bad
example of paraphrases = Red button in the annotation tool.  See the
LREC paper for a more extensive description of the annotation
procedure and guidelines.  If the two annotators fully agreed on the
category, the value in this field will be 4.0, 3.0, 2.0 or 1.0.  If
the two annotators chose adjacent categories, the value in this field
will be 3.5, 2.5 or 1.5.  For instance, a value of 2.5 means that one
annotator gave a score of 3 ("mostly good"), indicating a possible
paraphrase pair, whereas the other annotator scored this as a 2
("mostly bad"), that is, unlikely to be a paraphrase pair.  If the
annotators disagreed by more than one category, the sentence pair was
discarded and won't show up in the development or test set files.
