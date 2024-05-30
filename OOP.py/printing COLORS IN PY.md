Many Python programmers depend on printing out statements on console, as a GUI isn’t always necessary. The issue sometimes is that a black and white console doesn’t help much when one has several console windows opened and running different functions. For example:

![](https://miro.medium.com/v2/resize:fit:463/1*iCAEujpg9J-Lo3_ut0haow.png)

Honestly, you are not going to see what’s wrong (or right) easily when everything is just white on a black background.

So here it comes a library to help us out, fortunately! It’s called [**Termcolor**](https://pypi.org/project/termcolor/). So let’s install it via **_python -m pip install termcolor_** command.

## How to use it

It’s pretty simple. As simples as Python.

from termcolor import coloredprint(colored('Hello', 'red'), colored('Medium', 'yellow'), colored('world', 'green'))

## Before and after

![](https://miro.medium.com/v2/resize:fit:366/1*vb8kXbobDa_m-5CVuNYURw.png)

Regular printing vs Termcolor printing

## Using try and except

from termcolor import coloredtry:  
    int('Hello World')  
    print(colored('Success!', 'green'))  
except Exception as E:  
    print(colored(str(E), 'red'))

![](https://miro.medium.com/v2/resize:fit:353/1*-ae8bIrNssJEQKGM0fC3LA.png)

Printing an exception in red

from termcolor import coloredtry:  
    int('10')  
    print(colored('Success!', 'green'))  
except Exception as E:  
    print(colored(str(E), 'red'))

![](https://miro.medium.com/v2/resize:fit:318/1*Pa4dg54XBkkbAehXZhnVmw.png)

Printing a success try in green

## Printing an exception in color and bold

from termcolor import colored, cprint  
import systry:  
    int('Hello World')  
    print(colored('Success!', 'green'))  
except Exception as E:  
    cprint(str(E), 'red', attrs=['bold'], file=sys.stderr)

![](https://miro.medium.com/v2/resize:fit:347/1*jdi0vLBsfJrS74mGKFvk6Q.png)

Printing an exception in red and bold

## Printing try and exception in red and green backgrounds

from termcolor import colored, cprinttry:  
    int('Hello World')  
    print(colored('Success', 'green', attrs=['reverse', 'blink']))  
except Exception as E:  
    print(colored(str(E), 'red', attrs=['reverse', 'blink']))

![](https://miro.medium.com/v2/resize:fit:340/1*1Qf56s1cw99TMbpE5ukyGg.png)

`Printing an exception in red background

from termcolor import colored, cprinttry:  
    int('10')  
    print(colored('Success', 'green', attrs=['reverse', 'blink']))  
except Exception as E:  
    print(colored(str(E), 'red', attrs=['reverse', 'blink']))

![](https://miro.medium.com/v2/resize:fit:271/1*Kkbxwp2jikJV5ZR7eaWmrQ.png)

Printing a success try in green background