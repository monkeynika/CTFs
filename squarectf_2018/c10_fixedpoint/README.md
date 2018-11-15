# Square CTF 2018 - C10: fixed point

## Description

```
If you are a Charvis then this is a simple application of Xkrith’s First and Sixth Theorems.  If you are not a Charvis then you should familiarize yourself with Xkrith’s works.  A primer can be found in Room 100034B.
```
Note: For the humans out there, this system is built on an oscillator. It’ll disable itself when you find the oscillator’s fixed point.

## Preparation

The challenge was to find the ```x```, and f(x) should be equal x ( ```f(x) == x```).

We have fixed_point.html, where f(x) was declared:

```
function f(x) {
  if ((x.substr(0, 2) == '🚀') && (x.slice(-2) == '🚀')) {
    return x.slice(2, -2);
  }
  if (x.substr(0, 2) == '👽') {
    return '🚀' + f(x.slice(2));
  }
  if (x.substr(0, 2) == '📡') {
    return f(x.slice(2)).match(/..|/g).reverse().join("");
  }
  if (x.substr(0, 2) == '🌗') {
    return f(x.slice(2)).repeat(5);
  }
  if (x.substr(0, 2) == '🌓') {
    var t = f(x.slice(2));
    return t.substr(0, t.length/2);
  }

  return "";
}
```

So.. At first sight, it is not difficult to determine that ```x``` - it is a string, that contains emoji symbols of rockets, unos, moons, go on. And ```f(x)``` - it is a function that interpreting input one by one symbol.

Let's to dive in it. 

1) ```🚀x🚀```

Rockets.

Rockets are key players of this game. Because when ```f(x)``` gets into ```if(rockets)```, function just print ```x``` ( input in rockets, WITHOUT rockets).
When another emojis pass substring after one of their to another emoji, rockets just print it, and the current recursion spins in the opposite direction!

I call rockets as BRACKETS. Because they do nothing, they don't call ```f(x)``` again. And they disappears.. It is the hardest part of this game because rockets disappears. 

For example, when you print , you will see:

```🚀1122334455🚀``` -> ```1122334455```

P.S. I use plain numbers (by 2 symbols) to test this challenge, it is easy to see what happens

2) Another emojis - are functions. And I use next formula, I use any count of function, that are following each other, and one BRACKETS:

FuncFuncFuncFuncFunc......🚀some_emojis_that_just_will_be_print🚀

Where Func - one of 👽, 📡, 🌗, 🌓

3) ```📡```

This emoji just reverse string, after this one.

```📡🚀1122334455🚀``` -> ```5544332211```

4) ```🌗```

White moon gets string after this and multiples it by 5.

```🌗🚀11🚀``` -> ```1111111111```

5) ```🌓```

Black moon gets string after this and divides it by 2.

```🌓🚀11223344🚀``` -> ```1122```

6) ```👽```

Uno - is solution for rockets. Because uno creates one rocket and continue to send substring to ```f(x)``` again


## Solution

This challenge is solved from the end. Because a output of any function - it is a input for another one.

So, let's start with BRACKETS(rockets)!!!

```🚀11223344🚀``` -> ```11223344```

Poorly... :) Okay, let's add uno to this.

```👽🚀11223344🚀``` -> ```🚀11223344```

Good, but output contains the rocket at first position, and it is unpossible with my rules. O, I can reverse it! 

```📡👽🚀11223344🚀``` -> ```44332211🚀```

Good output, I have rocket at the last position, but my string into rockets are reversed. Okay, solve it in future.

Also I have only one rocket it output, but moons can help me to multiple rocket. At first, multiple it with 1 white moon. And then divide with 2 moons.

```🌓🌓🌗📡👽🚀11223344🚀``` - > ```44332211🚀44```

Let's increment count of white and black moons, until you will see the following output .

```🌓🌓🌓🌓🌓🌓🌗🌗🌗📡👽🚀11223344🚀``` - > ```44332211🚀44332211� ```

Fine! What I look here? I look that my input string in BRACKETS is reflected two times ( but it is reversed now), and I see one moon, and another one.. almost.

So, let's clear bad rocket at the end and reverse input at the same time.

```📡🌓🌓🌓🌓🌓🌓🌗🌗🌗📡👽🚀11223344🚀``` -> ```11223344🚀11223344```

Will you see it? It is almost the end. You can paste  ```📡🌓🌓🌓🌓🌓🌓🌗🌗🌗📡👽```  instead of ```11223344``` and you see :). But it is only a one rocket, where is another one.. last one..

Okay, what I know.. I know that with emoji-functions, I get ```11223344``` * 2 and one rocket. I have 4 symbols ( 11,22,33,44) * 2 and another one symbol (rocket). 

Can I do that I will have two rockets, but only (4 * 2 - 1 ) symbols? Exactly! I did a mistake ( but really not), in my previous steps. Just replace 👽 and 📡 symbols in the middle.

```📡🌓🌓🌓🌓🌓🌓🌗🌗🌗👽📡🚀11223344🚀``` -> ```223344🚀11223344🚀```


And it is the end :))) In input I have unnecessary symbol ```11```.

Replace ```223344``` with 📡🌓🌓🌓🌓🌓🌓🌗🌗🌗👽📡

```📡🌓🌓🌓🌓🌓🌓🌗🌗🌗👽📡🚀11📡🌓🌓🌓🌓🌓🌓🌗🌗🌗👽📡🚀``` -> Good!!!

## Finally

Finally , this solution is not unique :) Maybe, you have another one? :)


