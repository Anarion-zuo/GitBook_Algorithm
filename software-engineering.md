# Software Engineering

## Bit Hack

### Swap numbers

Exchange 2 integer number without a temporary storage.

```
x = x ^ y
y = x ^ y
x = x ^ y
```

This is due to the own inverse property of the operation xor.

### Find Max and Min

To decide the larger or smaller one between 2 integers, we have a naive way of writing it.

```
max(a, b):
	if a > b:
		return a;
	else:
		return b;
```

Due to the pipeline problem of the processor, this may prove slower than the bit controlling version.

```
r = y ^ ((x ^ y) & -(x < y))
```

