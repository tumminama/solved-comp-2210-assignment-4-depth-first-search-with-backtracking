Download Link: https://assignmentchef.com/product/solved-comp-2210-assignment-4-depth-first-search-with-backtracking
<br>
In this assignment, you will implement a version of a word search game much like Boggle<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> and other popular word games. The approach you take to finding words on the board will be a direct application of <em>depth-first search </em>with <em>backtracking</em>.

The version of the game that you will implement is played on a square board according to the following rules.

<ol>

 <li>Each position on the board contains one or more uppercase letters.</li>

 <li>Words are formed by joining the contents of adjacent positions on the board.</li>

 <li>Positions may be joined horizontally, vertically, or diagonally, and the board does not wrap around.</li>

 <li>No position on the board may be used more than once within any one word.</li>

 <li>A specified minimum word length (number of letters) is required for all valid words.</li>

 <li>A specified lexicon is used to define the set of all legal words.</li>

</ol>

Below, from left to right, is <em>(i) </em>a sample 4 × 4 board with single letters in each position, <em>(ii) </em>a sequence of positions forming the word PEACE, and <em>(iii) </em>the list of all words with a minimum length of 5 found on the board using the words in the standard Unix /usr/share/dict/words file as the lexicon.

<table width="288">

 <tbody>

  <tr>

   <td width="39">EAHQ</td>

   <td width="30">ELNT</td>

   <td width="30">CEBT</td>

   <td width="17">APOY</td>

   <td width="56"> </td>

   <td width="39">EAHQ</td>

   <td width="30">ELNT</td>

   <td width="30">CEBT</td>

   <td width="17">APOY</td>

  </tr>

 </tbody>

</table>

ALBEE ALCAE            ALEPOT

ANELE BECAP           BELAH

BELEE BENTHAL BENTY

BLENT CAPEL           CAPOT

CENTO CLEAN          ELEAN

LEANT LENTH           LENTO

NEELE PEACE            PEELE

PELEAN PENAL          THANE

TOECAP TOPEE

The following words are in that lexicon but <em>not </em>on the board: PLACE (rule 2), POPE (rule 4), PALE (rule 3). Although the word BOY can be constructed according to rules 2 and 3, if it does not appear in the lexicon being used (rule 6) or if it does not meet the specified minimum word length (rule 5), it would not be considered a word.

For a word to be <em>valid </em>it must be constructed in accordance with all six rules above. A score for each valid word is calculated as follows: one point for the minimum number of characters, and one point for each character beyond the minimum number. Thus, the set of all words of length five or more from the board above would earn a score of 31. (22 words of length 5, 3 words of length 6, and 1 word of length 7 give 22 + 6 + 3 = 31 points.)

1

<h1>Implementation Details</h1>

Your must implement your solution to the assignment in terms of two provided files: WordSearchGame.java and WordSearchGameFactory.java, plus one class that you create from scratch all on your own. The interface WordSearchGame describes all the behavior that is necessary to play the game. So, we can think of this interface as the specification for a game engine. You must develop your own game engine that meets this specification; that is, you must write a class that implements the WordSearchGame interface. You can name this class anything you want and you can add as many additional methods as you like.

The WordSearchGameFactory is a class with a single <em>factory </em>method for creating game engines. You must modify the createGame method to return an instance of your class that implements the WordSearchGame interface. Factory classes like this are convenient ways of completely separating an implementation (your class) from a specification (the provided interface). So, the test suite used for grading can be written completely in terms of the interface and without any knowledge of the specific classes used in the implementation.

A brief example client along with the corresponding output are given below. Although the class that implements the WordSearchGame interface must be in the same directory, the ExampleGameClient code is independent of its name or any other details of the class.

<strong>public class </strong><strong>ExampleGameClient </strong>{ <strong>public static </strong><strong>void </strong>main(String[] args) {

WordSearchGame game = WordSearchGameFactory.createGame(); game.loadLexicon(“wordfiles/words.txt”); game.setBoard(<strong>new </strong>String[]{“E”, “E”, “C”, “A”, “A”, “L”, “E”, “P”, “H”, “N”, “B”, “O”, “Q”, “T”, “T”, “Y”});

System.out.print(“LENT is on the board at the following positions: “);

System.out.println(game.isOnBoard(“LENT”));

System.out.print(“POPE is not on the board: “);

System.out.println(game.isOnBoard(“POPE”));

System.out.println(“All words of length 6 or more: “);

System.out.println(game.getAllValidWords(6));

}

}

Output:

LENT is on the board at the following positions: [5, 6, 9, 14]

POPE is not on the board: []

All words of length 6 or more:

[ALEPOT, BENTHAL, PELEAN, TOECAP]

<h2><strong>WordSearchGame </strong>methods relating to the lexicon</h2>

The three methods in the WordSearchGame interface that relate to loading and searching the lexicon are loadLexicon, isValidWord, and isValidPrefix. While loadLexicon is called only once per game, isValidWord and isValidPrefix will be called heavily throughout game play. These methods must be efficient if the game is to run with reasonable response times when using large lexicons. The choice of collection or data structure to store the lexicon will determine just how efficient these methods can be. You have two basic choices to represent the lexicon: use a prebuilt collection from the JCF or implement your own custom data structure or collection for the purpose. If you choose the former option, TreeSet will offer very good performance and provide very convenient methods. If you choose the latter option, a <em>trie </em>will provide even better (time) performance and will be a fun challenge to implement. The choice is completely up to you. A good (and optional) idea would be to develop your solution with a TreeSet to store the lexicon and then, if you have plenty of time at the end, make an attempt at building your own trie.

The loadLexicon method reads a list of words from a text file and stores each <em>unique </em>word in the data structure or collection that you select to represent the lexicon. You will notice that many of the words in the provided lexicon files are in lowercase while the game board is in uppercase. Be sure that the lexicon is loaded and all string comparisons are made in a case-insensitive manner. You will also notice that the provided lexicon files have different content and formats. Using a simple scanner, however, it is possible to use the same code to read in all the provided lexicon files.

Note that the lexicon must be loaded before calling many of the other WordSearchGame methods. If any method that is dependent on a lexicon is called before loadLexicon, your code must throw an IllegalStateException. See the source code documentation for details.

The isValidWord method searches the data structure that holds the lexicon for a specified string and indicates whether or not that string is present. If the string is present then it is a valid word. If the string is not present then it is not a valid word.

The isValidPrefix method searches the data structure that holds the lexicon to determine if any word in the lexicon begins with the specified string. For the purposes of this method, a string should be considered a prefix of itself. For example, “cat” is a prefix of “cat” as it is of “catalog”.

<h2><strong>WordSearchGame </strong>methods relating to the board</h2>

The setBoard method accepts an array of length <em>N</em><sup>2 </sup>that specifies the content of each position on the <em>N </em>× <em>N </em>board. The elements of the array are the Strings on the board listed in <em>row-major </em>order. Thus, the elements in the array from index 0 to <em>N</em><sup>2</sup>−1 correspond to the positions on the board from left to right, top to bottom. The element at index 0 stores the contents of the board position (0, 0) – the upper left corner – and the element at index <em>N</em><sup>2 </sup>−1 stores the contents of the board position (N-1, N-1) – the bottom right corner. In general, the String at index <em>p </em>= <em>row </em>× <em>N </em>+ <em>col </em>is the content of board position (<em>row,col</em>).

For example, the array a = [“E”, “E”, “C”, “A”, “A”, “L”, “E”, “P”, “H”, “N”, “B”, “O”, “Q”, “T”, “T”, “Y”] would correspond to the board show below on the left. The same board is shown below on the right but with each element annotated with its row-major position. Note that the letter N is at board position (2, 1), which corresponds to row-major position 2×4+1 = 9<em>.</em>

<table width="385">

 <tbody>

  <tr>

   <td width="154">


    <table width="116">

     <tbody>

      <tr>

       <td width="39">E</td>

       <td width="30">E</td>

       <td width="30">C</td>

       <td width="17">A</td>

      </tr>

      <tr>

       <td width="39">A</td>

       <td width="30">L</td>

       <td width="30">E</td>

       <td width="17">P</td>

      </tr>

      <tr>

       <td width="39">H</td>

       <td width="30">N</td>

       <td width="30">B</td>

       <td width="17">O</td>

      </tr>

      <tr>

       <td width="39">Q</td>

       <td width="30">T</td>

       <td width="30">T</td>

       <td width="17">Y</td>

      </tr>

     </tbody>

    </table></td>

   <td width="231">


    <table width="193">

     <tbody>

      <tr>

       <td width="58">E <sub>0</sub></td>

       <td width="40">E <sub>1</sub></td>

       <td width="59">C <sub>2</sub></td>

       <td width="36">A <sub>3</sub></td>

      </tr>

      <tr>

       <td width="58">A <sub>4</sub></td>

       <td width="40">L <sub>5</sub></td>

       <td width="59">E <sub>6</sub></td>

       <td width="36">P <sub>7</sub></td>

      </tr>

      <tr>

       <td width="58">H <sub>8</sub></td>

       <td width="40">N <sub>9</sub></td>

       <td width="59">B 10</td>

       <td width="36">O 11</td>

      </tr>

      <tr>

       <td width="58">Q 12</td>

       <td width="40">T 13</td>

       <td width="59">T 14</td>

       <td width="36">Y 15</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

The primary responsibility of the setBoard method is to populate the data structure that you choose to represent the board with the contents of the provided array of Strings. Once again, the choice of data structure to represent the board should be made in support of the algorithms that will depend on it (see game play methods below). Two obvious data structure choices include <em>(a) </em>just keeping the one-dimensional array of strings as the board representation or <em>(b) </em>creating a two-dimensional array of strings to directly represent the two-dimensional board being modeled. Other choices are possible, and the one you pick is at your discretion.

The implementing class of the WordSearchGame interface must have a default board. Specifically, the above board should be set as the default so that it is available for game play even if the setBoard method has not been called.

The getBoard method returns a string representation of the current board suitable for printing to standard out (i.e., as an argument to System.out.println). There is no particular format required. Choose the format that is most helpful to you. This method will not be tested or graded.

<h2><strong>WordSearchGame </strong>methods relating to game play</h2>

The methods that implement game play options are getAllValidWords, isOnBoard, and getScoreForWords. The getAllValidWords method returns a SortedSet of strings containing all words on the board that are of a specified minimum length and can be constructed according to the game rules. If no words can be found, this method returns an empty SortedSet.

The isOnBoard method takes a string argument and determines if that string can be found on the board. There is no requirement that the string be in the lexicon or have a minimum length. There is, however, the requirement that it be constructed according to rules 2, 3, and 4. If it is possible to find the string on the board, this method returns a List of Integers representing the row-major positions of each substring<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>in the individual board positions used to construct the string. For example, isOnBoard(“PEACE”) would return the list [7, 6, 3, 2, 1].

<table width="193">

 <tbody>

  <tr>

   <td width="58">E <sub>0</sub></td>

   <td width="49">E <sub>1 </sub></td>

   <td width="49">C <sub>2 </sub></td>

   <td width="36">A <sub>3</sub></td>

  </tr>

  <tr>

   <td width="58">A <sub>4</sub></td>

   <td width="49">L <sub>5</sub></td>

   <td width="49">E <sub>6 </sub></td>

   <td width="36">P <sub>7</sub></td>

  </tr>

  <tr>

   <td width="58">H <sub>8</sub></td>

   <td width="49">N <sub>9</sub></td>

   <td width="49">B 10</td>

   <td width="36">O 11</td>

  </tr>

  <tr>

   <td width="58">Q 12</td>

   <td width="49">T 13</td>

   <td width="49">T 14</td>

   <td width="36">Y 15</td>

  </tr>

 </tbody>

</table>

Both getAllValidWords and isOnBoard can be implemented using slightly different versions of depth-first search. The specific depth-first algorithms that you implement must be efficient enough for use on large game boards with large lexicons.

The getScoreForWords method returns the cumulative score of all the scorable words in the given SortedSet. To be scorable, a word must (1) have at least the minimum number of characters, (2) be in the current lexicon, and (3) be on the current board. Each scorable word of length <em>K &gt; M </em>is worth 1+(<em>K </em>−<em>M</em>) points, where <em>M </em>is the specified minimum length.

<h2>Provided word list files for the lexicon</h2>

You have been provided with several word list files for creating lexicons.

<ul>

 <li>txt – 270,163 unique words, used in international Scrabble tournaments</li>

 <li>txt – 167,964 unique words, used in North American Scrabble tournaments</li>

 <li>txt – 234,371 unique words, provided with Unix distributions</li>

 <li>txt – 172,823 unique words, subset of the Unix list</li>

 <li>txt – 19,912 unique words, small subset of the Unix list</li>

</ul>

Your solution should run efficiently with each of these files used for the lexicon.

<h1>Acknowledgements</h1>

Word search games of various sorts are popular CS 2 assignments because they bring together several important topics all in one place. This incarnation of the word search problem owes thanks to (at least): Julie Zelenski, Owen Astrachan, and Mike Smith.

<a href="#_ftnref1" name="_ftn1">[1]</a> <a href="https://en.wikipedia.org/wiki/Boggle">https://en.wikipedia.org/wiki/Boggle</a>

<a href="#_ftnref2" name="_ftn2">[2]</a> Since the board positions contain strings and not just single characters, the List returned by isOnBoard contains strings.