Part1
=======

StringServer Code
-----

Here is the code for my StringServer:

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    
    String str = "";

    public String handleRequest(URI url) {

        if(url.getPath().contains("/add-message")){
            
            String[] parameters = url.getQuery().split("=");
            if(parameters[0].equals("s")){
                if(str!=""){
                    str = str + "\n" + parameters[1];
                    return String.format(str);
                }
                str = str + parameters[1];
                return String.format(str);
            }
            return "404 Not Found";
        }
        return "404 Not Found";
    }
}

class StringServer {
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

Screenshot1
-----------

![image](screenshot1.png)

In this Screenshot, the method **handleRequest()** is called with the url shown in the screenshot.

The relevent argument should be **Server.start(port, new Handler())**.

The relevent values of fields include **str=""**, **url=null**, **parameters[]=null**.

Once the request is made, the value of **url** is changed to **http://localhost:7777/add-message?s=Hello**.

The value of **parameters[]** is changed to **{s, Hello}**. 

The value of **str** is changed to **"Hello"**.

Screenshot2
-----

![image](screenshot2.png)


In this Screenshot, the method **handleRequest()** is called with the url shown in the screenshot.

The relevent argument should be **Server.start(port, new Handler())**.

The relevent values of fields include **str="Hello"**, **url=null**, **parameters[]=null**.

Once the request is made, the value of **url** is changed to **http://localhost:7777/add-message?s=How20%are20%you**.

The value of **parameters[]** is changed to **{s, How are you}**. 

The value of **str** is changed to **"Hello\nHow are you"**.

Part2
====

I choose the bug in reverseInPlace method.

Here is a failure-inducing input for this buggy program:

```
public void testReversedInPlace2(){
    int[] input2 = {1,2,3};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{3,2,1}, input2);
}
```

Here is an input that doesn't induce a failure:

```
public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
}
```

Here is the Sympton of running the two tests provided above.

The second test passed but the first test didn't.

<img width="610" alt="image" src="https://user-images.githubusercontent.com/122486933/215355161-a2165291-1807-4291-80d6-02292221c6e7.png">

Here is the code of reverseInPlace method before the bug is fixed:

```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
        arr[i] = arr[arr.length - i - 1];
    }
}
```

Here is the code of reverseInPlace method after the bug is fixed:

```
static void reverseInPlace(int[] arr) {
    int[] arrNew = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
        arrNew[i] = arr[arr.length - i - 1];
    }
    for(int i=0; i<arr.length; i++){
        arr[i]=arrNew[i];
    }
}    
```

The bug for this method is that the reversing operation is done on the input array, which makes the array the same after the reverse is done. In the fixed method, the reverse is done on a new array that carries the same values as the input array and assign the new array to the input array after the reverse.

Part3
====

The most important thing I learned in lab3 is that sometimes buggy programs can also past the test. Therefore, it is crucial to design different tests and inputs for a program. Consider special cases while testing. For example, what if the input is null or what if the input is a array with only one element. Testing these special inputs and fixing bugs caused by them will make the program moew reliable.























