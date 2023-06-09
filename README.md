Download Link: https://assignmentchef.com/product/solved-csce-121-lab-4-arrays-and-strings
<br>
This lab will give you additional practice with arrays and a great deal with strings. You’ll see that this lab, like the prior one, asks you to think about useful test cases to help ensure that your code is working correctly. Try to cultivate this skill!

How far must I get?

This lab is assigned for the entire week. Look over the questions and make an attempt at some of the questions before arriving at your lab session. This way you can optimize your lab time and get help with the questions that are most tricky for you. You’re expected to complete the questions that you’re not able to finish in the lab on your own.

<h2>Strings as char arrays</h2>

These exercises in this section build up incrementally, so it’s best to make sure you are quite confident with your answer to a question before moving onto the next. If you get stuck (or frustrated, or bored with strings) then you can skip to the questions on Shuffling or Pig Latin, below, and then return to this question anew.

It will probably be helpful to reuse parts of your previous solutions. Think about this as a way of planning your approach, organizing your code, and testing the validity of your prior answers. <em>Don’t be deceived by the simplicity of the operation being asked for</em>—behind their seemingly trivial requirements lurk many fiddly conditions. Producing code which works correctly for all of these conditions is a challenge, demanding thoroughness and clear logical thinking. If you run into difficulties, perhaps with functions producing unpredictable or unexpected behavior, the likely source is something which is failing to ensure that the resulting array of characters ends with a ‘ ’ character.

<em>Important:</em> Even if you know of a string library that can perform many of these operations, the point of these exercises is to write your own functions. We’re using string processing as a way to practice working with arrays. You should only use char[]’s for these questions, absolutely no fancy C++ classes for strings, or regular expressions, or any other chicanery not covered in the lectures.

<h3>Appending a character</h3>

Write a function that takes a string and modifies it by appending a capital X to its end.

void appendX_unsafe(char str[]){ // modifies the input, appending an X to it.    …}

To test your function you may find the following main function useful. It uses cin in a different way—this new way is helpful because it allows you to prompt the user for strings that include spaces.

int main(){    char mystr[100];    cout &gt;&gt; “Please enter a string: “;    cin.getline(mystr, 100);     appendX_unsafe(mystr);        cout &gt;&gt; “mystr = ‘” &gt;&gt; mystr &gt;&gt; “‘
”;        char s[100] = “lab three”;    appendX_unsafe(s);    cout &gt;&gt; “s = ‘” &gt;&gt; s &gt;&gt; “‘
”;     return 0;}

The previous function was unsafe because if str was an array of size 7 and contained the string “danger”, then the string “dangerX” would have overrun. Do you see why?

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>Here “danger” = [‘d’, ‘a’, ‘n’, ‘g’, ‘e’, ‘r’, ‘ ’], but “dangerX” = [‘d’, ‘a’, ‘n’, ‘g’, ‘e’, ‘r’, ‘X’, ‘ ’], which requires the memory for 8 characters.</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

Now, extend your previous function by also passing in the number of characters in the array, and checking that you don’t write beyond where you should.

void appendX(char str[], int len){ // modifies the input, only appending an X if it would still be within bounds    …}

Test your code by trying

char t[7] = “danger”; appendX(t,7);

and comparing it with

char t[8] = “danger”;appendX(t,8);




It is usually worth your while to construct test inputs for your code during the process of writing it. Try to get into the habit of doing this.

<h3>Concatenating two strings</h3>

The next step is to write a function that appends one string to the end of another. If the combined length is longer than can be safely stored, append as many characters as can be safely stored, but don’t forget to always ensure you include a terminating ‘ ’.

void catstrings(char first[], int fst_len, char second[]){ // concatenate second to the end of first, while not overrunning first    …}

One smart way to test your code is to make an array which is actually longer than fst_len. If you fill that array with some unusual character, say a ‘~’, you can check whether any part beyond first[fst_len-1] has been overwritten by ensuring that the remainder of the entries are still filled with ‘~’s. To do so, you’ll need to write your own function to print out all elements of the array, since cout always stops at the first ‘ ’. This can help give you confidence that your code is respecting the fst_len limit for operations that change (or write) memory, but it won’t ensure that you don’t try to read memory that you shouldn’t. Careful reasoning is the best way to deal with the latter issue.

Writing the code for the previous question that is correct for all instances requires extreme care. Even though your program may seem to run without any problems, that only provides limited evidence for correctness. Ask one of your peers to try find an input that causes your code to act incorrectly. Then do the same for them. If you can’t find anyone to do it in person, post your code to piazza.

<h3>String comparison</h3>

Write a function that compares two strings, returning true if and only if they are identical.

bool string_eq(char first[], char second[]){ // return true iff two strings are identical, character for character     …}

You may think that just using the line first == second would do the job. <em>Try it!</em>

It’ll compile. If your testing is only cursory, it might even trick you into believing that it’s working correctly. Here’s an example: for some previously defined variable str, if you call string_eq(str, str) as a test, you’ll get true. And string_eq(“hot”, “dog”) will return false, also just as you’d expect. But, in fact, strings (and arrays in general) won’t work correctly this way because they are aggregate data types.

Once you get your string_eq working correctly, write a new function string_eq_nocase which compares two strings but ignores differences in the case of letters (so that “bungle” and “BUNgle” are identical).

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td><a href="http://robotics.cs.tamu.edu/dshell/cs121/code/501_fourc.cpp">&lt;=”” a=””&gt;Here</a> is code that includes a function lower_to_upper that returns the uppercase twin of any lowercase letter.</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

bool string_eq_nocase(char first[], char second[]){ // a case insensitive check for string equality    …}

<h3>Stripping multiple spaces</h3>

Some typesetting conventions, especially those from the middle of the last century (which were influenced by the fixed width characters of typewriters), involved sometimes putting two succeeding spaces after certain punctuation symbols.

Write a function that takes a string (as an array of characters) as input and modifies it so that, wherever it has multiple consecutive spaces, these become a single space. For example “Some␣*crazy*␣␣␣␣s␣␣p␣␣␣␣a␣c␣␣␣i␣␣n␣g␣␣!!” becomes “Some␣*crazy*␣s␣p␣a␣c␣i␣n␣g␣!!”, where here I’ve used ␣ to denote the space character ‘ ‘.

void strip_dup_spaces(char str[]){ // removes all consecutive spaces from str    …}

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>Some good test cases might start with multiple spaces, have some words in the middle, but also end with multiple spaces.</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<h3>Substring search</h3>

Write a function that looks for one string as a substring within another. Here we consider “engine” as a substring of the following string (a pithy quotation of Rick Cook):

“Programming today is a race between software engineers striving to build bigger and better idiot-proof programs, and the Universe trying to produce bigger and better idiots. So far, the Universe is winning.”

For this question, we want the substring to appear in sequence and without requiring deletions so, although “engines” can be found within the quotation (via “<u>engine</u>er<u>s</u>“, we see that it is a subsequence), we don’t consider it a substring.

bool contains_sub_str(char haystack[], char needle[]){ // return true iff needle appears within haystack    …}

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>Be careful about overrunning the end of either string. Also don’t forget test cases that have a needle that is longer than the haystack.</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

Writing an efficient contains_sub_str function is surprisingly difficult. Of course, we mainly care about correctness, so our task is comparatively easy. (If you’re curious, <a href="https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm">this</a> and <a href="https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string_search_algorithm">also this</a> Wikipedia page will give you an entry into the world of string-matching algorithms; if this spurs your interest, talk with me about it.)

<h3>Substring deletion</h3>

Now combine the experience gained in writing strip_dup_spaces and contains_sub_str functions, to write a function that deletes the first occurrence of a substring, if that substring appears in the input.

bool del_first_occur(char input[], char cut[]){ // if cut appears in input, remove it and return true;   // otherwise leave input as is and return false    …}

<h2>Shuffling a Deck of Cards</h2>

Your previous effort in helping the local casino with their craps table was a big success. It was so helpful that you’re now their preferred consultant, their go-to numerical ninja. This time it is to investigate some suspicious payouts at the Texas Hold ’Em table. The management suspect a crooked croupier, <a href="https://en.wikipedia.org/wiki/Croupier_(film)">Clive</a>, who is deft at manipulating cards. They suspect that, cunningly, he is dealing crooked hands because he is able to control the deck. Your job is to get to the bottom of this conundrum.

You don your finest evening gown/black tie and head to the table to play a few hands. With a tiny camera concealed slyly about your person, you’re able to record Clive’s shuffle technique close-up. After casually placing a few bets, you swizzle a few expensive drinks (on the house, of course), then return to your lab.

As you pick over the details of the video with forensic care, you observe that Clive’s technique in shuffling the cards is definitely suspicious. After pondering for a while, it hits you: his shuffles are just too perfect. He cuts the standard deck of 52 cards precisely in half every time. Holding the top half in his right hand, bottom half in his left, he then shuffles them so that they interleave exactly: the cards come down singly, first one from the left hand, then one from the right hand, one from the left, right, left, … flawlessly. He does it every time, perfectly, with the repeatability of a machine.

Then you notice something else. The casino requires at least 5 shuffles per deal. Clive exceeds this by a substantial margin: he shuffles 8 times per deal. It is always 8 times, not 7 on some occasions and 9 on others, but 8, always and exactly 8. Again and again, 8, religiously.

To get to the bottom of the matter, write some code to examine how a sequence of 8 perfect shuffles, as described above, permutes the cards in the deck.

<h2>Pig Latin Printer</h2>

Write a function that is given a string (as a ‘ ’ terminated char array) and prints it out to the console (via cout) in Pig Latin form. You should handle a whole string, translating it word by word. A single word is converted to Pig Latin via following algorithm:

<ol>

 <li>If the string begins with a vowel, simply append <em>“way”</em> to the end of the string. As an example, the Pig Latin for <em>“elephant”</em> is <em>“elephantway”</em>.</li>

</ol>




<ol>

 <li>Otherwise, locate the first vowel, move all the characters before the vowel to the end of the word, and add <em>“ay”</em>. For example, Pig Latin for <em>“tradition”</em> is <em>“aditiontray”</em> as the two characters <em>“tr”</em> appear before the first vowel.</li>

</ol>

(For this question, use the five standard vowels: <em>a</em>, <em>e</em>, <em>i</em>, <em>o</em>, and <em>u</em>.)

Note that you’re not asked to produce a new string, only to output to cout. That makes it easier because you don’t have to make space for the <em>“way”</em> or <em>“ay”</em>. You will likely find it is useful to write and test several separate functions to achieve the final result.

Try to write all the code without needing to copy characters from the input array into another array. In computing, the phrase <a href="https://en.wikipedia.org/wiki/Zero-copy">zero-copy</a> is sometimes used to refer to information processing operations which avoid having the CPU copy data; it is typically used to improve efficiency. In this question, as we don’t know how long the string provided as input will be, we wish to avoid making copies to ensure <em>safety</em>. (Later in the class, we’ll be able to allocate variables of sizes that are determined dynamically, so you’d be able to make an array of the right size—but a solution without needing to allocate new variables is still superior.)

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>Good test cases to try are: the empty string, a string with only a single character (either a vowel, or not), and long strings of vowels.</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<h2>Numerical Expression Cost*</h2>

<h3>Converting to an arbitrary radix</h3>

A couple of exercises back we looked at how to pull off digits in an integer in base 10 and also base 2. The latter was useful for various things, such as the repeated squaring technique for computing exponents efficiently. In this question you need to write a function to convert an integer input into an arbitrary radix or base. Represent the output as an array. This is the declaration for my function which does that:

void convert_to_base(long in, int out_digits[], int out_base){ // The inputs: in is a positive integer  //             out_base is a also positive integer  // Output: the number in, but expressed digit-by-digit in out_digits    …}

You should decide whether you’ll put the least significant digits (the units) at out_digits[0] or whether you’ll put the most significant digit there. I found it computationally convenient to put the least significant digits at lower indices of out_digits, but also wrote a function that prints it in the more usual form of most significant digit first.

This section’s title says “an arbitrary radix” but we’re actually only considering positive integer radices. And, at least for my code, extra work was needed to handle base <a href="https://en.wikipedia.org/wiki/Unary_numeral_system">1</a>. Believe it or not, you can express numbers in terms of bases that are fractions (such as ⅜), real numbers (imagine base √2 ), and there’s no reason why they can’t be negative too! I don’t know of any applications for these strange representations, but if you discover one be sure to tell me.

<h3>Computers that use ternary</h3>

In the past, there have been several serious proposals to build computers which would use ternary, i.e., base 3, as the underlying representation for data. Rather than <em>bits</em> (binary digits) such computers have memory measured in <em>trits</em> (ternary digits). Nikolay P. Brusentsov and Sergei Sobolev, of Moscow State University, developed a computer called <a href="https://en.wikipedia.org/wiki/Setun">Setun</a> that operated successfully with the ternary representation. Between 1958–1965 about fifty such machines were produced and operated. On this side of the Atlantic, the original proposal for the MIT Whirlwind considered base 3 but never explored those ideas in their real implementation, and some decades later researchers at SUNY Buffalo designed the TERNAC which used base 3.

Binary has the advantage of being easy to implement reliably through electronics. So why consider using ternary, then? The answer is that base 3 is more efficient. But what exactly is the efficiency of a particular radix? The typical definition takes the width of a representation multiplied by the radix.If width = number of columns of digits, and we have radix = number of symbols, thencost<sub>radix</sub> = width × radix.By this measure, base 3 is the most efficient of all integer bases.

<h3>Ternary doesn’t always win</h3>

Despite base 3 being more efficient than base 2 for an infinite number of integers, there are some specific ones where binary wins out. For example, to represent 65,535 in binary, you need 16 bits, yielding a representational cost as cost<sub>2</sub>(65535) = 16 × 2 = 32. Whereas a ternary representation requires 11 trits, and hence cost<sub>3</sub>(65535) = 11 × 3 = 33. So we see that cost<sub>3</sub>(65535) &gt; cost<sub>2</sub>(65535).

Use your convert_to_base function to count how many integers are more efficiently represented in binary than ternary.

<table>

 <tbody>

  <tr>

   <td>

    <table>

     <tbody>

      <tr>

       <td><strong>?</strong></td>

       <td>HINT</td>

      </tr>

     </tbody>

    </table></td>

   <td>

    <table>

     <tbody>

      <tr>

       <td>There are exactly 8,487 values for x where cost<sub>3</sub>(x) &gt; cost<sub>2</sub>(x).</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

It turns out that the question of the most efficient representation isn’t just a two horse race. There’s another positive integer base that beats binary and ternary for a small set of numbers. Use your convert_to_base function to find this third base which can beat out base 2 and base 3 in some particular cases.

* This is material outside the scope for lab quizzes.