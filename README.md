class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        # Handle overflow edge case
        INT_MAX = 2**31 - 1
        INT_MIN = -2**31
        
        # divisor = 0 is undefined – you may raise exception or handle as appropriate
        if divisor == 0:
            raise ValueError("Divisor cannot be zero")
        
        # If dividend is INT_MIN and divisor is -1, result would overflow 32-bit signed int
        if dividend == INT_MIN and divisor == -1:
            return INT_MAX
        
        # Determine sign of result
        negative = (dividend < 0) ^ (divisor < 0)
        
        # Work with absolute values to simplify logic
        dvd = abs(dividend)
        dvs = abs(divisor)
        
        # The quotient
        result = 0
        
        # Use bit-shifts to subtract large chunks (faster than one-by-one subtraction)
        for shift in range(31, -1, -1):
            if (dvs << shift) <= dvd:
                dvd -= (dvs << shift)
                result += (1 << shift)
        
        # Apply sign
        if negative:
            result = -result
        
        # Clamp to 32-bit signed int range
        if result < INT_MIN:
            return INT_MIN
        if result > INT_MAX:
            return INT_MAX
        
        return result
        
