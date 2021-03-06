# Flare-On 8 Challenge 1

In this challenge we are asked to reverse engineer JS. The downloaded file is a html document. Opening up the file in a text editor shows that the script is contained in the `<script>` tags. There are two functions in the the script on called `dataEntered()` and the other called `checkCreds()`. DataEntered() is used to enable the submit button on the login for when a username and password combination is entered. checkCreds() is called when the button is clicked. 

The main code block within checkCreds() runs only if the username field's value is Admin and the password field's value is goldenticket (after it has been decoded using base64). The flag is generated by bitwise xor of the password and a key that is stored in the variable *encoded_key*

## JS Code Snippet
*Only what is useful is included here*

```
var encoded_key = "P1xNFigYIh0BGAofD1o5RSlXeRU2JiQQSSgCRAJdOw=="
function checkCreds() {
	if (username.value == "Admin" && atob(password.value) == "goldenticket") 
	{
		var key = atob(encoded_key);
		var flag = "";
		for (let i = 0; i < key.length; i++)
		{
			flag += String.fromCharCode(key.charCodeAt(i) ^ password.value.charCodeAt(i % password.value.length))
		}
		document.getElementById("banner").style.display = "none";
		document.getElementById("formdiv").style.display = "none";
		document.getElementById("message").style.display = "none";
		document.getElementById("final_flag").innerText = flag;
		document.getElementById("winner").style.display = "block";
	}
	else
	{
		document.getElementById("message").style.display = "block";
	}
}
```


```python
import base64
```


```python
returned_value = "goldenticket"
```


```python
print(ord(returned_value[0 % len(returned_value)]))
```

    103



```python
string = returned_value.encode()
```


```python
key = base64.b64encode(string)
```


```python
print(key)
```

    b'Z29sZGVudGlja2V0'



```python
val = key.decode()
print(key.decode())
```

    Z29sZGVudGlja2V0


This is the password that we can enter into the login form to get the flag. 

Entering this password will take us to another page that will generate the flag.

![GT flag](./GT.PNG)

## Python Equivalent

However we can also get the flag by using the code below:


```python
encoded_key = "P1xNFigYIh0BGAofD1o5RSlXeRU2JiQQSSgCRAJdOw=="
keys = bytearray(base64.b64decode(encoded_key))
print(keys)
```

    bytearray(b'?\\M\x16(\x18"\x1d\x01\x18\n\x1f\x0fZ9E)Wy\x156&$\x10I(\x02D\x02];')



```python
flag = ""
for i in range(0, len(keys)):
    flag = flag + chr(keys[i] ^ ord(val[i % len(val)]))
    
print(flag)
```

    enter_the_funhouse@flare-on.com


The flag is `enter_the_funhouse@flare-on.com`
