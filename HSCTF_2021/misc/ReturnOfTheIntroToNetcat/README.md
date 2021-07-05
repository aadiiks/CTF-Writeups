# Challenge Name: Return of the Intro to Netcat

## Description

hey, netcat seems fun! (with a twist)
```bash
nc return-of-the-intro-to-netcat.hsc.tf 133
```

## Detailed solution

Start by using netcat 

```bash

┌──(kali㉿kali)-[~]
└─$ nc return-of-the-intro-to-netcat.hsc.tf 1337
== proof-of-work: enabled ==
please solve a pow first
You can run the solver with:
    python3 <(curl -sSL https://goo.gle/kctf-pow) solve s.AACF.AACi6vTn3MLtWFW9DhpBI7T6
===================

Solution?

```

Opening another terminal to execute the python command 

Open another terminal
  
```bash  

┌──(kali㉿kali)-[~]
└─$ python3 <(curl -sSL https://goo.gle/kctf-pow) solve s.AACF.AACi6vTn3MLtWFW9DhpBI7T6                

Solution:
s.AAAjsU4q6lrgYTpQZNRzhbSRmtUg4TLliDvAAyIXYC9Vut/OZJtnL94WYclfCH2hFltBsPLTmnwSfgDlET4/u0PM4W6s8v6iMxChjn4I3xnqhGL6JJ8kAd1iFA3NZkVvDM96difAYmpwRqwLGqIsSpDCc74Sb0+9uwYS/F9/elR9wcLldbHtb6ySpigp211Mm/nAm+qNM5mYB16WJzVmOQ06

```  

Now back to netcat and put the solution 

```bash  

┌──(kali㉿kali)-[~]
└─$ nc return-of-the-intro-to-netcat.hsc.tf 1337
== proof-of-work: enabled ==
please solve a pow first
You can run the solver with:
    python3 <(curl -sSL https://goo.gle/kctf-pow) solve s.AACF.AACi6vTn3MLtWFW9DhpBI7T6
===================

Solution? s.AAAjsU4q6lrgYTpQZNRzhbSRmtUg4TLliDvAAyIXYC9Vut/OZJtnL94WYclfCH2hFltBsPLTmnwSfgDlET4/u0PM4W6s8v6iMxChjn4I3xnqhGL6JJ8kAd1iFA3NZkVvDM96difAYmpwRqwLGqIsSpDCc74Sb0+9uwYS/F9/elR9wcLldbHtb6ySpigp211Mm/nAm+qNM5mYB16WJzVmOQ06
Correct
You got it! Here's what you're looking for: flag{the_cat_says_meow}

```  


### Flag

```
flag{the_cat_says_meow}
```
