# Cracking challenges

## ELF x86 - 0 protection

We get an ELF 32-bit binary. If we run it, we get a password prompt asking us to put in a password. If we enter something, it tells us that password is incorrect. 

<img src="images/ch1-1.png">

Firstly, we can run `ltrace` to see how the binary is checking our input. Running the command, we see the binary parse our input and allocate memory. However, we also see it comparing our input to a string: 

```C
strcmp("password", "123456789")
```

<img src="images/ch1-2.png"> 

If we satisfy this condition to be true by entering `123456789` as the password, the binary tells us that we solved the challenge. 

<img src="images/ch1-3.png">

The password to complete the level is `123456789`

## ELF x86 - Basic

We get an ELF 32-bit binary. If we run it, we get a username prompt asking us to input our username. Entering a random username will likely result in the binary returning "Bad username". 

<img src="images/ch2-1.png">

Running `ltrace` gives us an error, so what we can do is run this binary in [radare2](https://github.com/radareorg/radare2). You can read the [documentation](https://readthedocs.org/projects/radare2s-website/downloads/pdf/latest/) on how to use it, but in this challenge we're just going to be visiting the grahpical view of the disassembled binary. This can be done by analyzing the binary with `aaa`

<img src="images/ch2-2.png">

then going into hex view with `V`, navigating down towards the `main` function with the arrow keys

<img src="images/ch2-3.png">

and then going into the graphical view with Shift+V once more. 

<img src="images/ch2-4.png">

Once here, we go down a bit with the arrow keys and notice two strings being defined (`var_ch` as "john", and `var_10h` as "the ripper"). We notice a few things in this function. 

Firstly, we notice the calls to the `puts` function being made, showing the strings we saw when running the binary, asking us for our username. Followed by that, the `getString` function is called to get whatever string we provide, and put it into `s1`. 

<img src="images/ch2-5.png">

Furthermore, we see the `var_ch` variable being moved to `s2`. 

<img src="images/ch2-6.png">

After these two values (`s1`, and `s2`) are defined, they are used as parameters in the function call `strcmp`. Based on the result of this function (either true or false), the "Bad username" string is printed and the program exits, or a "password: " string is printed, presumabely asking for our password. 

<img src="images/ch2-7.png">

Based on this string comparison, and the one done later down in the program when asking us for our password, we can conclude that the username and password for this binary are: `john:theripper`.

Using these values, we get the flag. 

<img src="images/ch2-8.png">

The password to complete the level is `987654321`