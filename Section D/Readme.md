# Take-home Test - RG



# Section D: Asymptotic Computational Complexity - Option 1: Space Complexity

1. The worst-case space complexity of the “doBracketsMatch” function is the use of a stack instead of a numeric counter. Only one value needs to be maintained for each set of brackets and it is possible for there to be more closing than opening brackets which would result in a valid negative value. There is the potential for a stack overflow condition.


2. The worst-case space complexity of the “doBracketsMatch” function if we extended it to support multiple pairs of opening and closing brackets.   (a) Mismatching different types of brackets. (b) Potential stack overflow. 
  

3.   Alternative design changes that would affect the extension. (a) Include validation to report that no brackets were detected. (b) Report the location(s) of mismatched brackets. (c) Set up an array of arrays to handle the support of multiple pairs of brackets. The inner array would contain the symbols for each set of brackets, a numeric counter and a flag variable to indicate that type of bracket has been found. The containing array would allow for multiple pairs of different types of brackets. The extension would need to return the results as an array. 

'____________________________________________________________________________________________________________'
