# Estimate pi 

## Using the Wallis formula

### Background

The [Wallis Formula](https://en.wikipedia.org/wiki/Wallis_product) can be
used to approximate the value of the constant 𝛑. 
The formula is like so 

![Wallis](wallis.png)

Some more background is available at https://encyclopediaofmath.org/wiki/Wallis_formula

The estimates of PI increase as the number of terms in the equation
increase. When doing this using a computer, you will have a loop that
will run `n` times. The larger the value of `n`, the more terms there
will be and higher the accuracy of the estimate. 

### Exercise

There is a file provided called `wallis.c` which has a prototype of
a function `wallis_pi`. It will take the number of iterations and
return the estimate of pi using the wallis formula.

Implement this function using the above formula.

## Using a monte carlo simulation

### Background

Imagine a circle of radius 1. The area will be 𝛑*1*1 which is 𝛑. 
Now imagine a square that circumscribes this circle. The edge of the
square will be of length 2. Hence, the area of the square will
be 4. This is illustrated in the diagram below.

![Circle](circle.png)

The ratio of the areas of the two shapes will be 

    area of circle          𝛑
    --------------   =    -----
    area of square          4


Hence the value of 𝛑 will be 4 times the ratio of the areas of the two
figures.

                     4 x area of circle
           𝛑     =   ------------------
                       area of square

The ratio of the areas can be estimated using a monte carlo 
simulation. 

### Monte carlo simulation

Imagine throwing a dart randomly onto the square. It will either be inside or
outside the circle. If we throw lots of darts, the ratio of the darts
inside the circle to the total number of darts will start to
approximate the ratio of the areas of the two figures. 

We can use this to get the ratio and then multiply that by 4 to get
the value of pi.

[Here](https://www.youtube.com/watch?v=ELetCV_wX_c) is a video of the
process to understand how it works

### Exercise
There is a function prototype called `mc_pi` in the file
`monte_carlo.c` which should accept the number of iterations (the
number of darts thrown) as input and return the estimate of 𝛑.

### Implementation hints
You can use the provided `frandom` function to generate and return a
random number (to simulate dart throws). It will return numbers
between 0 and 1 (i.e. all in the first quadrant of the square). 

Use the `frandom` twice per iteration to function to find a random
point `(x,y)` and then check whether it lies inside the circle by
calculating the distance between the point and `(0,0)`. 

## Testing

You can run `make test_mc` and `make test_wallis` to check if your
implementations are correct before you push the code.
#include <assert.h>
#include <stdlib.h>
#include <stdio.h>
#include <math.h>

float wallis_pi(int);

int main(void) {
  float pi;
  for (int i=0; i<5; i++) {
    pi = wallis_pi(i);
    if (!(fabs(pi - M_PI) > 0.15)) {
      printf("Estimate with just %d iterations is %f which is too accurate.\n", i, pi);
      abort();
    }
  }

  for (int i=500; i<3000; i++) {
    pi = wallis_pi(i);
    if (!(fabs(pi - M_PI) < 0.01)) {
      printf("Estimate with even %d iterations is %f which is not accurate enough.\n", i, pi);
      abort();
    }
  }
}

float wallis_pi(int n)
 {
    float s=1.0;
    for (int i=1; i<=n; i++)
    {
        s=s*(4*i*i)/((4*i*i)-1);
    }
   return (2*s); 
 }
