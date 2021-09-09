# Take-home Test - RG

'____________________________________________________________________________________________________________'
## Section A: Code Review - Option 4: Ruby

1. The quick and simple answer to the student's question is to explicitly assign the value to the array. e.g. array[array.index(n)] = "*#{n}*"

2. However it appears that the student is approaching the programming requirement from the wrong direction. 

3. There are an infinite number of incorrect spellings. The correct way to code a Spell Checker is to only compare all values against valid spellings and then flag just those that are incorrect. The @words array should have the following values; @words = ['cat', 'dog', 'rabbit', 'hamster', 'budgie', 'parrot']. 

4. Modify the "If" statement with an "else" in order to append asterisks to only mispellings that do not match the correct values in the @words array. (Noted that the test could have been reversed instead of using the else option. Either option would accomplish the same result.)

5. Modified code example below;
````Ruby
 class Spelling_Checker
    def initialize()
        @words = ['cat', 'dog', 'rabbit', 'hamster', 'budgie', 'parrot']
    end
    def spellChecker(string)
        array = string.split(" ")
        array.each{ |n|
            if @words.include? n
            else
                array[array.index(n)] = "*#{n}*"
            end
        }.join(" ")end end 
checker = Spelling_Checker.new
output = checker.spellChecker("cat ct dog pig hamster parot")
p output
````



6. The output from the above modified code will be; "cat \*ct\* dog \*pig\* hamster \*parot\*"

7. Note that \*pig\* is flagged because it is not yet included in the @words array.

8. Disclaimer: I have never seen any Ruby code prior to this exercise but it was relatively easy to look up the relevant commands based upon my knowledge of other languages. Debugging, coding logic and understanding of requirements are universal to all languages.


'____________________________________________________________________________________________________________'
