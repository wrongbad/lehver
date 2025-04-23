# LehVer

A versioning system based on [Lehmer Code](https://en.wikipedia.org/wiki/Lehmer_code)

Versions are correctly ordered by any unicode-aware string sort

### Symbols

🔴🔵🟠🟡🟢🟣🟤🟥🟦🟧🟨🟩🟪🟫  
14 symbols = 6227020800 possible versions  
Symbols are sorted by ascending unicode values

### Algorithm

- Start with a 0 based version counter (0, 1, 2, ...)
- Convert the number to [factorial number system](https://en.wikipedia.org/wiki/Factorial_number_system)
- Reading the factoradic digits from left to right:
  - Select that symbol from the list, removing it from the list
- Add the remaining first symbol to the front of the version string

### Example

- Version # 4
- Factoradic 200
- 2 🔴🔵🟠🟡🟢🟣🟤... -> 🟠
- 0 🔴🔵🟡🟢🟣🟤... -> 🔴
- 0 🔵🟡🟢🟣🟤... -> 🔵
- (add to front) 🟡🟢🟣🟤... -> 🟡
- Ver: 🟡🟠🔴🔵

### Reference Implementation

```py
# python
import sys
ver = int(sys.argv[1])
sym = '🔴🔵🟠🟡🟢🟣🟤🟥🟦🟧🟨🟩🟪🟫'

# compute factorials
fac = [1]
for i in range(1, len(sym)):
    if fac[0] * i <= ver:
        fac = [fac[0] * i, *fac]

out = ''
for base in fac:
    # get factoradic digit
    d = ver // base
    ver -= d * base
    # get symbol for digit
    out += sym[d]
    # remove symbol from list
    sym = sym[:d] + sym[d+1:]

# add next symbol to the front (for sort order)
out = sym[0]+out

print(out)
```
