# FLOAT32
Custom IEEE-754 single precision floating-point representation

# Features
- Create a single precision floating point number

# To-Do
- Add
- Subtract
- Multiply
- Divide
- Error Handling for nan and other special numbers

# Initialization
```cpp
    FLOAT32(int whole, int fraction)
    {
        char sign = whole >= 0 ? 0 : 1;

        whole = Absolute(whole);
        fraction = Absolute(fraction);
        char exponent = CalculateExponent(whole, fraction);
        int mantissa = CalculateMantissa(exponent, whole, fraction);

        data |= (sign & 1) << 31;
        data |= (exponent & 0xFF) << 23;
        data |= (mantissa & 0x7FFFFF);
    }
```

# Addition
```cpp
FLOAT32 AddFloats(FLOAT32 a, FLOAT32 b)
{
        //todo
        return FLOAT32();
}
```

# Subtraction
```cpp
FLOAT32 SubtractFloats(FLOAT32 a, FLOAT32 b)
{
        //todo
        return FLOAT32();
}
```

# Multiplication
```cpp
FLOAT32 MultiplyFloats(FLOAT32 a, FLOAT32 b)
{
        //todo
        return FLOAT32();
}
```

# Division
```cpp
FLOAT32 DivideFloats(FLOAT32 a, FLOAT32 b)
{
        //todo
        return FLOAT32();
}
```

# Utilities

### IEEE-754 Exponent
```cpp
int CalculateExponent(int whole, int fraction)
{
    if (whole == 0 && fraction == 0)
        return 0;
    int exponent = 0;
    if (whole == 0)
    {
        int base = Power(10, CountDigits(fraction));
        for (int i = 0; i < 31; i++)
        {
            fraction *= 2;
            if (fraction > base)
            {
                exponent--;
                break;
            }
            exponent--;
        }
        return exponent + 127;
    }
    while (whole > 1)
    {
        whole >>= 1;
        exponent++;
    }
    return exponent + 127;
}
```

### IEEE-754 Mantissa
```cpp
int CalculateMantissa(char exponent, int whole, int fraction)
{
    exponent -= 127;
    int result = 0;
    result |= (whole << (23 - exponent));
    int base = Power(10, CountDigits(fraction));
    for (int i = 23 - exponent - 1; i >= 0; i--)
    {
        fraction *= 2;
        int bit = 0;
        if (fraction >= base)
        {
            bit = 1;
            fraction -= base;
        }
        result |= (bit & 1) << i;
    }
    if (fraction > 0)
    {
        int round_base = (fraction >= (base / 2)) ? 1 : 0;
        result |= round_base;
    }
    return result;
}
```

### Absolute
```cpp
int Absolute(int value)
{
    return value < 0 ? value *= -1 : value;
}
```

### Count Digits
```cpp
int CountDigits(int value) 
{
    if (value == 0)
        return 1;

    int digits = 0;
    value = Absolute(value);

    while (value > 0) 
    {
        value /= 10;
        digits++;
    }

    return digits;
}
```

### Power
```cpp
int Power(int base, int exponent)
{
    int result = 1;
    while (exponent > 0)
    {
        if (exponent & 1)
            result *= base;

        base *= base;
        exponent >>= 1;
    }

    return result;
}
```
