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

## Section B: Domain Speciality

Given the programming language choices in the other sections and the limitations on the choice in Section B I was only left with the option of using Java in order to meet the specific requirements for Section B. Java is not my language of choice.  While I have previously developed a Geocoding Android/ASP.NET application that encompassed all of the requirements for web development and data CRUD functionality via API JSON, it was all programmed using C#. I can request permission to share that code with you if that suits your requirements. After doing some research and making several attempts I realized that I do not have the resources within 10 days in order to replicate my prior accomplishments using Java. 

As a former project manager when I encountered a situation of this nature I would take the following actions. (1) Arrange a meeting with stakeholders and the development team. (2) At the meeting explain the resource constraints. (3) Provide alternatives as a way to resolve the resource constraint. (4) Ask the stakeholders to clarify their priorities so as to better focus the efforts of the development team. (5) Collaborate with stakeholders and the development team to meet the primary goals by including out-of-the-box alternative solutions. (6) Reach an agreement on how to proceed.

As a Cogrammer faced with a programmer that I am mentoring in this situation I would explain to them the steps above and request that they either take those actions themselves or get in touch with their project manager to take those steps on their behalf. 

As the programmer in this situation I would appreciate the opportunity to take the above steps with the hiring team to ensure that your goals for my completion of this exercise will be met. 

I am confident in my knowledge and abilities to accomplish the tasks in this exercise as this is something that I have done on prior projects using other languages with longer timelines. Though I am unable to meet the requirements as currently specified, I feel that I am a good candidate for the code review and mentoring position, and look forward to working with you to ensure you are able to accurately evaluate my suitability. 

'____________________________________________________________________________________________________________'

## Section C: Problem-Solving - Option 2: String Calculator

1. In order to save time I chose to make use of the Microsoft SignalR tutorial as the project base that would handle the standard UI and IO tasks for the solution. (https://docs.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-5.0&tabs=visual-studio) This is in accordance with the Best Practice of reusing working code while it does not impact the stipulation regarding the use of "under the hood" libraries to solve the problem.

2. I opted to use Javascript and the places where I inserted my own code is bracketed with comments e.g. RG Code Starts..., RG Code Ends....

3. The Javascript code that I wrote to handle the String Calculator is in the Chat.js file. (Minor modifications were made to the SignalR UI to alter the label and hide the unneeded text box.)

4. My Javascript code is as follows;

````Javascript

 "use strict";

var connection = new signalR.HubConnectionBuilder().withUrl("/chatHub").build();

//Disable send button until connection is established
document.getElementById("sendButton").disabled = true;

connection.on("ReceiveMessage", function (user, message) {
    var li = document.createElement("li");
    document.getElementById("messagesList").appendChild(li);
    // We can assign user-supplied strings to an element's textContent because it
    // is not interpreted as markup. If you're assigning in any other way, you 
    // should be aware of possible script injection concerns.
    // RG Code modification start....
    li.textContent = ` result: ${message} `;
    // RG Code modification end....
});

connection.start().then(function () {
    document.getElementById("sendButton").disabled = false;
}).catch(function (err) {
    return console.error(err.toString());
});

document.getElementById("sendButton").addEventListener("click", function (event) {
    var user = document.getElementById("userInput").value;
    var message = document.getElementById("messageInput").value;
    // RG Code Inserted here....

    try {
        const strArry = user.split(" ");
        if (strArry.length > 1) {
            switch (strArry[0].toUpperCase()) {
                case "value".toUpperCase():
                    message = strArry[1];
                    break;
                case "+":
                    message = plusSign(strArry);
                    break;
                case "-":
                    message = minusSign(strArry);
                    break;
                case "*":
                    message = multiplySign(strArry);
                    break;
                case "/":
                    message = divideSign(strArry);
                    break;
                case "factorial".toUpperCase():
                    message = factorial(Number(strArry[1])).toString();
                    break;
                case "prime".toUpperCase():
                    message = calcHiPrimeNbr(Number(strArry[1])).toString();
                    break;
                case "fibonacci".toUpperCase():
                    message = higesthFibonacciNumber(Number(strArry[1])).toString();
                    break;
                default:
                    message = user + ' ! no operator detected'
            }
        } else {
            message = user + ' ! only single string detected'
        }
    }
    catch (err) {
        message = " ! Error encountered: " + err;
        message = " ! Please correct the input and try again.";
    }
    // RG Code Ends here....
    connection.invoke("SendMessage", user, message).catch(function (err) {
        return console.error(err.toString());
    });
    event.preventDefault();

    // RG Code Starts here....

    function cleanupResult(wrkFloat) {
        var num = parseFloat(wrkFloat);
        num = +(Math.round(num + "e+3") + "e-3");
        return num.toString();
    }

    function plusSign(strArry) {
        // Plus sign
        var wrkFloat = parseFloat("0.0"); //0.0
        // Plus sign
        // Loop through all the elements in the array 
        for (var i in strArry) {
            if (i > 0) {
                if (Number.isNaN(strArry[i])) {
                    //ignore non numeric values
                } else {
                    wrkFloat = wrkFloat + Number(strArry[i]);
                }
            }
        }
        return cleanupResult(wrkFloat);
    }

    function minusSign(strArry) {
        // Minus sign
        var wrkFloat = parseFloat("0.0"); //0.0
        wrkFloat = Number(strArry[1]);
        // Loop through all the elements in the array 
        for (var i in strArry) {
            if (i > 1) {
                if (Number.isNaN(strArry[i])) {
                    //ignore non numeric values
                } else {
                    wrkFloat = wrkFloat - Number(strArry[i]);
                }
            }
        }
        return cleanupResult(wrkFloat);
    }

    function multiplySign(strArry) {
        // Plus sign
        var wrkFloat = parseFloat("0.0"); //0.0
        wrkFloat = Number(strArry[1]);
        // Plus sign
        // Loop through all the elements in the array 
        for (var i in strArry) {
            if (i > 1) {

                if (Number.isNaN(strArry[i])) {
                    //ignore non numeric values
                } else {
                    wrkFloat = wrkFloat * Number(strArry[i]);
                }
            }
        }
        return cleanupResult(wrkFloat);
    }

    function divideSign(strArry) {
        // Plus sign
        var wrkFloat = parseFloat("0.0"); //0.0
        wrkFloat = Number(strArry[1]);
        // Plus sign
        // Loop through all the elements in the array 
        for (var i in strArry) {
            if (i > 1) {

                if (Number.isNaN(strArry[i])) {
                    //ignore non numeric values
                } else {
                    wrkFloat = wrkFloat / Number(strArry[i]);
                }
            }
        }
        return cleanupResult(wrkFloat);
    }

    function factorial(n) {
        let rslt = 1;
        if (n == 0 || n == 1) {
            return rslt;
        } else {
            for (var i = n; i >= 1; i--) {
                rslt = rslt * i;
            }
            return rslt;
        }
    }

    function calcHiPrimeNbr(nbr) {
        var pFlg;
        var n;
        var i;
        var x;
        var hPrm;
        pFlg = 1;
        // document.Write( "Prime Numbers are : " );
        for (n = 1;
            (n <= nbr);
            n++) {
            x = (n - 1);
                for (i = 2;
                    (i <= x);
                    i++) {
                        if (((n % i) ==
                            0)) {
                            pFlg = 0;
                            break;
                        } else {
                            pFlg = 1;
                        }
                }
            if ((pFlg == 1)) {
                //document.Write(n) ;
                hPrm = n;
            }
        }
        return hPrm;
    }

    function fibArry(nbr) {
        //set up fibonnaci numbers in array
        let fib = [0, 1];
        let rsltData = [];
        for (let i = 2; i <= nbr; i++) {
            fib[i] = fib[i - 1] + fib[i - 2];
            rsltData.push(fib[i]);
        }
        return rsltData;
    }

    function higesthFibonacciNumber(nbr) {
        //find highest fibonacci number in array below value provided       
        var rslt = 0;
        var i = 0;
        var fibData = fibArry(nbr);
        while (i < nbr) {
            if (fibData[i - 1] < nbr) {
                rslt = fibData[i - 1];
            }
            i++;
        }
        return rslt;
    }
    // RG Code Ends here....     });


````



5. The project is named SignalRChat and can be compiled and executed using Visual Studio 19.

6. String values can be entered in the Input box. Pressing the Calculate button invokes the code. The Results of each calculation are listed below. All error messages are prefixed with an exclamation mark. 

7. When I reveiwed my code as against the requirments for this option I realised that I had overlooked the nested parentheses requirment. This would involve setting up a looping function to execute the nested calculations in order to obtain the result. 

'____________________________________________________________________________________________________________'


# Section D: Asymptotic Computational Complexity - Option 1: Space Complexity

1. The worst-case space complexity of the “doBracketsMatch” function is the use of a stack instead of a numeric counter. Only one value needs to be maintained for each set of brackets and it is possible for there to be more closing than opening brackets which would result in a valid negative value. There is the potential for a stack overflow condition.


2. The worst-case space complexity of the “doBracketsMatch” function if we extended it to support multiple pairs of opening and closing brackets.   (a) Mismatching different types of brackets. (b) Potential stack overflow. 
  

3.   Alternative design changes that would affect the extension. (a) Include validation to report that no brackets were detected. (b) Report the location(s) of mismatched brackets. (c) Set up an array of arrays to handle the support of multiple pairs of brackets. The inner array would contain the symbols for each set of brackets, a numeric counter and a flag variable to indicate that type of bracket has been found. The containing array would allow for multiple pairs of different types of brackets. The extension would need to return the results as an array. 

'____________________________________________________________________________________________________________'
