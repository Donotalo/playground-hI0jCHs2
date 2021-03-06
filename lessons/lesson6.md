The `sizeof` operator takes a parameter and returns the size of the parameter in bytes. Following example prints the size of an array with 3 elements, each is a 32 bit integer:

```C
#define ARRAY_SIZE 3

int32_t arr[ARRAY_SIZE] = { 0 };

size_t size = sizeof(arr);
printf("Array size %d bytes\n", size);
```

The return type of `sizeof` operator is `size_t` which is an integer. The example above will convey the message that the array size is 12 bytes (3 integers, 4 bytes each).

Now consider the following program:

```C runnable
#include <stdio.h>
#include <stddef.h>
#include <stdint.h>

#define ARRAY_SIZE 3

void print_array_size(const int32_t parr[])
{
	size_t size = sizeof(parr);
	printf("Size of parameter is %d bytes\n", size);
}

int main()
{
	int32_t arr[ARRAY_SIZE] = { 22, 33, 44 };

	size_t size = sizeof(arr);
	printf("Actual array size is %d bytes\n", size);

	print_array_size(arr);

	return 0;
}
```

You'll notice that the sizes are different in `main()` and `print_array_size()` even though _it appears that_ the same 12 byte array `arr` is passed to the `print_array_size()` function. Based on compiler settings, you could also see warning for using `sizeof` operator on parameter `parr` because `parr` is declared as array.

What happens is when an array is passed to a function as parameter, it is _converted_ to a pointer to the first element of the array. In the example above, `parr` is indeed a pointer to `arr[0]` even though array syntax (`[]`) is used.

Hence, `sizeof(parr)` returns the size of a pointer on the platform. The code above will work the same way if the function prototype is declared as follows:

```C
void print_array_size(const int32_t* parr)
```

An array can be passed to a function through pointer. Using the parameter which is a pointer to the array the array elements can be modified:

```C runnable
#include <stdio.h>
#include <ctype.h>

void string_to_upper(char *pstr)
{
	while (pstr != NULL &&  /* pstr itself cannot be NULL, a NULL pointer cannot be dereferenced */
	       *pstr != '\0')   /* Continue only if this is not the end of the string */
	{
		*pstr = toupper(*pstr); /* Converts original array content */
		pstr++; /* Points to next character */
	}
}

int main()
{
	char str[] = "a small string";
	string_to_upper(str);
	printf(str);

	return 0;
}
```

In the above example the character array or C string `str` is passed to `string_to_upper()` function using parameter `pstr` which becomes pointer the the first character of the array. Using `pstr` the whole array is modified. Then `str` is printed in `main()` and the modifications made in `string_to_upper()` is observed.

