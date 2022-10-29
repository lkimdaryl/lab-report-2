# Lab Report 2

The code below is a simple search engine that allows you to add strings and search for strings that matches your query.  


```
import java.io.IOException;
import java.net.URI;
import java.util.*;

class Handler implements URLHandler {

    List<String>  words = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("This is the homepage");
        } else if (url.getPath().equals("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                words.add(parameters[1]);
                return parameters[1] + " added";
            }
        } else {
            if (url.getPath().contains("/search")) {
                String[] param = url.getQuery().split("=");
                if (param[0].equals("s")) {
                    List<String> wordsFound = new ArrayList<>();
                    String res = "words found: \n";
                    for (int i=0; i<words.size();i++ ){
                        if (words.get(i).contains(param[1])){
                            wordsFound.add(words.get(i));
                        }
                    }

                    for (int j=0; j<wordsFound.size();j++){
                        res = res + wordsFound.get(j) + "\n";
                    }
                    return res;
                }
            }
        }
        return "404 Not Found!";
    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
When compiling and running the code above, it will prompt us to visit "http://localhost:(some port number)" and the web browser will display the following.

![Image](imgs\homepage.JPG)

The image above is the result of the code:
```
 if (url.getPath().equals("/")) {
            return String.format("This is the homepage");
```

Adding just a "/" right after the port number will yield the same result.

On the other hand, adding a "/add?s= (some strings)" will yield the following image:

![](imgs\add-apple.JPG)

In this case, I added the string "apple".

This was the result of the following codes:
```
} else if (url.getPath().equals("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                words.add(parameters[1]);
                return parameters[1] + " added";
            }
```
It checks to see if we added "/add" in our path and if so, it will create an array of strings where we can store our querries. It then checks if the first element of that array is "s", and if so, it stores the second element and display on the web browser that it has been added.

I then added a bunch more strings to store for later. When I finished, I added "/search?s=app" to my path instead of the "/add?s= (some strings)" and the following image displayed.

![](imgs\search.JPG)

In this case, I was searching for any words I added that has the substring "app" and those three words appeared.

_____
## Part 2
We were asked to provide the following from this week's lab.
1. The failure-inducing input (the code of the test)

2. The symptom (the failing test output)

3. The bug (the code fix needed)

4. Then, explain the connection between the symptom and the bug. Why does the bug cause that particular symptom for that particular input?

1st Code:

```
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
        
    return arr;
  }
```

1. failure inducing input:
```
int[] input1 = {1,2,3};
```
2. Symptom:

![](./imgs/ArrayTestInducingFailure.JPG)

3. bug:
```
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
        
    return arr;
```
4. The code is accessing the wrong element for each iteration of the for loop and therefore will return the wrong result.


2nd code:
```
class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) { 
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
```
1. failure inducing input
```
		List<String> myList = new ArrayList<>();
		myList.add("a");
		myList.add("b");
		myList.add("k");
		myList.add("ap");
		myList.add("apple");
		myList.add("pale");
```
2. Symptom:

![](./imgs/ListTestFailure.JPG)

3. bug:
```
result.add(0, s);
```
4. This line of code adds string s at the beginning of the list and shifts the rest of the element to  the right which gives the reverse order of the of the original list. 

