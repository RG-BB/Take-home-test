# Take-home Test - RG

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
                    message = highesthFibonacciNumber(Number(strArry[1])).toString();
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

    function highesthFibonacciNumber(nbr) {
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

7. When I reveiwed my code as against the requirments for this option I realised that I had overlooked the nested parentheses requirement. This would involve setting up a looping function to execute the nested calculations in order to obtain the result. Further last minute testing established that I have overlooked the contiguous white space requirement. The use of the Trim and Replace string functions are missing from my code.  

