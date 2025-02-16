---
layout: post
title: How to design ReLu using bitwise operations?
date: 2024-11-26 14:00:00
description: Optimize ReLu from a hardware perspective
tags:
categories:
thumbnail: assets/img/design_relu/pipeline.jpg
---

We can simply design ReLu in C as below:
```c++
int relu(int x) {
	if (x < 0) 
	   return 0;
	else
	   return x;
}
```

However, from a hardware perspective, this type of coding can be very inefficient.
Imagine that this type of code is compiled and implemented in hardware via an ISA.
Each instruction implemented in an assembler has a structure that takes several clocks to complete instead of just one clock. This structure also allows multiple instructions to be executed simultaneously to increase efficiency.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/design_relu/pipeline.jpg" title="pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ARM 3-stage pipeline operation
</div>

In the pipeline above, we can see that the decode process of the STR instruction and another ADD instruction are performed at the same time that the ADD instruction is entered and the execute process is being performed. With this type of pipeline, we can expect low CPI (Clock Cycles per Instruction).
 
However, this pipeline is difficult to apply to code that has branches that move memory locations, such as the previous code. Therefore, it is advantageous to write hardware-optimized code that uses as few branching statements as possible. So how do we create ReLu code that is branch-free?
```c++
void arm_relu_q7(int8_t *data, uint16_t size)
{
...
    /* Run the following code for M cores with DSP extension */

    uint16_t i = size >> 2;
    int8_t *input = data;
    int8_t *output = data;
    int32_t in;
    int32_t buf;
    int32_t mask;

    while (i)
    {
        in = arm_nn_read_s8x4_ia((const int8_t **)&input);

        /* extract the first bit */
        buf = (int32_t)ROR((uint32_t)in & 0x80808080, 7);

        /* if MSB=1, mask will be 0xFF, 0x0 otherwise */
        mask = QSUB8(0x00000000, buf);

        arm_nn_write_s8x4_ia(&output, in & (~mask));

        i--;
    }

    i = size & 0x3;
    while (i)
    {
        if (*input < 0)
        {
            *input = 0;
        }
        input++;
        i--;
    }
    
 ...
```

The above code is part of the arm_relu_q7 function in the CMSIS-NN Library provided by ARM.
In a nutshell, it takes 8 bits of data, 4 by 4, from the arm_nn_read_s8x4_ia function.
It then performs an and operation with the value 0x80808080 and shifts the resulting value 7 bits to the right. 0x80 is 0b10000000, so when the input is negative, it will become 0b00000001 after shifting 7 bits, and when the input is positive, it will become 0b00000000.
After the bit shift, we subtract the value of the shifted bit from 0x00000000. We can calculate the mask like 0b00000000 minus 0b00000001 to get 0b11111111 when the input is negative, and 0b00000000 minus 0b00000000 to get 0b00000000 when the input is positive.
Finally, if we do the AND operation on the initial input and the mask with the NOT operation, we get ReLu function that returns 0 if the input is negative or returns same value if the input is positive.
 
By using bitwise operations in this way, we can write hardware-optimized code without branching.