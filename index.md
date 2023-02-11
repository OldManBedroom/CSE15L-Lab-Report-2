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

For this screenshot, the method **handleRequest()** is called with the provided url.

The relevent arguments are **Server.start(port, new Handler());** **server.createContext("/", new ServerHttpHandler(handler));** **String ret = handler.handleRequest(exchange.getRequestURI());**

Once the **start()** mehtod is called, the **creatContext()** method is called to create a new handler.

This handler is will call **handleRequest()** method

The relevent values of fields include **str=""**, **url=null**, **parameters[]=null**.

Once the request is made, the value of **url** is changed to **http://localhost:7777/add-message?s=Hello**.

The value of **parameters[]** is changed to **{s, Hello}**. 

The value of **str** is changed to **"Hello"**.

Screenshot2
-----

![image](screenshot2.png)


For this screenshot, the method **handleRequest()** is called with the provided url.

The relevent arguments are **Server.start(port, new Handler());** **server.createContext("/", new ServerHttpHandler(handler));** **String ret = handler.handleRequest(exchange.getRequestURI());**

Once the **start()** mehtod is called, the **creatContext()** method is called to create a new handler.

The relevent values of fields include **str="Hello"**, **url=null**, **parameters[]=null**.

Once the request is made, the value of **url** is changed to **http://localhost:7777/add-message?s=How20%are20%you**.

The **20%** signs in this url mean **space** in url-encode.

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

Here is the Symptom of running the two tests provided above.

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
The bug for this method is that the original array is not saved. Once half of the array is reversed, the first half of this array will just be like the second half of the array in reversed order, so when you try to reverse the second half of the array, the elements' values will not change. For example, if the original array is **[1,2,3,4,5,6]**, the reversed array will be **[6,5,4,4,5,6]**, not **[6,5,4,3,2,1]**. To fix this bug, I created a new array to save the reversed array so the original array won't be changed. during the reversing, Each value of the new array is assigned with the value of the original array in reversed order. After the reverse, the new array will be assigned to the original array. Now the original array is in reversed order.


Part3
====

The most important thing I learned in lab3 is that sometimes buggy programs can also past the test. Therefore, it is crucial to design different tests and inputs for a program. Consider special cases while testing. For example, what if the input is null or what if the input is a array with only one element. Testing these special inputs and fixing bugs caused by them will make the program m reliable.























