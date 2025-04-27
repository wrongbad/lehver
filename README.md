# ChromaVer

- Bright colors are more fun and uplifting than cold hard digits
- No time wasted arguing the boundary of "Major" and "Minor" changes
- Versions are correctly ordered by any unicode-aware string sort
- [Lehmer Code](https://en.wikipedia.org/wiki/Lehmer_code) and [Factorial Number System](https://en.wikipedia.org/wiki/Factorial_number_system) are used to encode numbers
- Symbols are introduced gradually as versions increase, for easy learning

### Symbols

- 🔴🔵🟠🟡🟢🟣🟤🟥🟦🟧🟨🟩🟪🟫  
- 14 symbols -> 13! = 6227020800 possible versions  
- Symbols are sorted by ascending unicode values  
- Adding higher unicode symbols is backward-compatible

### Algorithm

- Start with a 0 based version counter (0, 1, 2, ...)
- Convert the number to factorial number system
- Reading the factoradic digits from left to right:
  - Select that symbol from the list, removing it from the list
- Add the next symbol from the list to the front of the version string

### Example

- Version Index `4`
- Factoradic `200`
- 2 🔴🔵🟠🟡🟢🟣🟤... -> 🟠
- 0 🔴🔵🟡🟢🟣🟤... -> 🔴
- 0 🔵🟡🟢🟣🟤... -> 🔵
- (add to front) 🟡🟢🟣🟤... -> 🟡
- Version `🟡🟠🔴🔵`

### Reference Implementation

```py
# python
import sys
sym = '🔴🔵🟠🟡🟢🟣🟤🟥🟦🟧🟨🟩🟪🟫'
ver = int(sys.argv[1])

# compute factorials
fac = [1]
for i in range(1, len(sym)):
    if fac[-1] * i <= ver:
        fac += [fac[-1] * i]

out = ''
for base in reversed(fac):
    # get factoradic digit
    d = ver // base
    ver -= d * base
    # get symbol for digit
    out += sym[d]
    # remove symbol from list
    sym = sym[:d] + sym[d+1:]

# add next symbol to the front (for sort order)
out = sym[0] + out

print(out)
```
