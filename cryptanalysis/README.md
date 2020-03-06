# Cryptanalysis Challenges

## Encoding - ASCII

Just decode the given string using the given [ASCII table](http://repository.root-me.org/Cryptographie/EN%20-%20ASCII%20Table.gif). Looking at the given string of letters and numbers, we can determine that the string is in hexadecimal format. You can use the command line tool `xxd` to convert the hexadecimal bytes into ASCII, but the string is not in in the correct format for `xxd`, so I just used a script to format the string into an array two byte strings, prefixed by '0x'. 

```shell
#!/bin/bash
for i in $(echo $1 | fold -w2)
do
   echo "0x$i" | xxd -r
done
```

<img src="images/ch8-1.png">

The password to complete the level is: `2ac376481ae546cd689d5b91275d324e`
