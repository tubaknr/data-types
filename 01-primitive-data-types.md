# Primitive Data Types
1. string
2. number
3. bigint
4. boolean
5. undefined
6. symbol
7. null

------
## 1. String
### Explanation
A sequence of characters used to represent text. 

### Example
(```const stringTypeVariable = "This is sequence of characters."```)

### Important Key Points
- uses UTF-16
- 2 byte = 16 bit.
- emojis are 4 bytes. 
- ```stringTypeVariable.length``` = # of UTF-16 code units = # of characters
- the space it occupied in memory = # of characters * 2
- 1 code unit = 16 bit = 2 byte.
