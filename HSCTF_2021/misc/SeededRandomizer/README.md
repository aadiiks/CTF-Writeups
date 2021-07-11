# Challenge Name: seeded-randomizer

## Description

There are at least two types of randomizers in Java, one that is purely random and one that is seeded (that is pseudorandom). Please fix this code to output the correct flag (note the flag format, and a sample has been provided).

![](https://i.imgur.com/6xLgJii.png)

## Detailed solution

Basically, in java theres 2 ways to randomize number, 1-truly random 2-fake random (can give u the same thing given a specific SEED value) 

![](https://i.imgur.com/GHSVggC.png)
with the SEED random, u can guess the next random numbers, no matter how many times u run it, its the same

Heres how it acts without the seed
![](https://i.imgur.com/tRmCNyh.png)
in short, 
random() returns a random number
random(x) where x is the seed, will return a psudo-random, meaning u can predict/know the pattern cuz its the same everytime

```java
import java.util.Random;

public class SeededRandomizer {

	public static void display(char[] arr) {
		for (char x: arr)
			System.out.print(x);
		System.out.println();
	}

	public static void sample() {
		Random rand = new Random(79808677);

		char[] test = new char[12];
		int[] b = {9, 3, 4, -1, 62, 26, -37, 75, 83, 11, 30, 3};
		for (int i = 0; i < test.length; i++) {
			int n = rand.nextInt(128) + b[i];
			test[i] = (char)n;
		}
		display(test);
	}

	public static void main(String[] args) {
		sample();
		// Instantiate another seeded randomizer below (seed is integer between 0 and 1000, exclusive):
		char[] flag = new char[33];
		int[] c = {13, 35, 15, -18, 88, 68, -72, -51, 73, -10, 63, 
				1, 35, -47, 6, -18, 10, 20, -31, 100, -48, 33, -12, 
				13, -24, 11, 20, -16, -10, -76, -63, -18, 118};

		for (int j = 0; j <= 1000; j++) {
			Random rand = new Random(j);

			for (int i = 0; i < flag.length; i++) {
				int n = rand.nextInt(128) + c[i];
				flag[i] = (char)n;
			}
			display(flag);
		}
	
	}

}
```

1-i copied the highlighted from the sample() function to the main() function
2-i looped 0-1000 cuz thats what the comment said

### Flag
```
flag{s33d3d_r4nd0m1z3rs_4r3_c00l}
```
