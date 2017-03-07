# My Codewars Kata

TLWR; Look at my code styles. Cheaters are lame.

These code snipets are my solutions to some of the code puzzles I encountered at http://www.codewars.com.
They are here for readers to get a sense of how I approach some puzzle-type coding problems. If you're looking for solutions to the kata, don't bother. It's like training for karate by getting your neighbor to practice all the moves.

That said if you're trying to get a better sense of challenge parameters you can follow the title links to each puzzle for a full description of the problem. 

###[Euchre Score](https://www.codewars.com/kata/586eca3b35396db82e000481)

```python
def update_score(current_score, called_trump, alone, tricks):
    """Update the score based on the rules of Euchre."""
    
    tricks = tricks.count(called_trump)
    idx = called_trump - 1
    
    if tricks <= 2:
        idx = 1 - idx
        current_score[idx] += 2
    
    elif 3 <= tricks <= 4:
        current_score[idx] += 1
    
    elif tricks == 5:
        points = 2
        if alone:
            points *= 2
        current_score[idx] += points
    
    return current_score    
```

###[Weight for weight](https://www.codewars.com/kata/55c6126177c9441a570000cc)

```python
from operator import itemgetter

def order_weight(strng):
    """Return ordered weights defined by an obfuscation of strng."""

    strng = sorted(strng.split(' '))
    weights = [ (num, sum(( int(n) for n in num))) for num in strng ]
    weights = ' '.join([ n for n, _ in sorted(weights, key=itemgetter(1)) ])
    
    return weights
```

###[Validate Sudoku with size `NxN`](https://www.codewars.com/kata/540afbe2dc9f615d5e000425)

```python
import math

class Sudoku(object):
    """A Sudoku checker implimented as a class."""

    def __init__(self, board):
        self.board = board
        self.row = len(board[0])
        self.col = len(board)
        self.unit = math.sqrt(self.row)
        self.valid = list(range(1, self.row+1))
        self.is_valid()
    
    def valid_check(self, row):
        return sorted(row) == self.valid
        
    def valid_rows(self):
        board = self.board
        return all([ self.valid_check(row) for row in board ])
      
    def valid_cols(self):
        board = zip(*self.board)
        return all([ self.valid_check(col) for col in board ])
    
    def valid_squares(self):
        jstep = int(self.unit)
        jlim = (jstep-1) * jstep + 1
        istep = self.row
        ilim = self.row * self.col
        board = sum(self.board, [])
        board = [ board[i+j:i+j+jstep] for j in range(0,jlim,jstep) for i in range(0, ilim, istep) ]
        board = sum(board, [])
        board = zip(*[iter(board)]*istep)
        return all([ self.valid_check(square) for square in board ])
    
    def valid_board(self):
        ints = all([ type(i) == type(1) for i in sum(self.board,[]) ])
        dim = self.row == self.col
        unit = self.unit.is_integer()
        return all((ints, dim, unit))
        
    
    def is_valid(self):
        if self.valid_board():
            return all((self.valid_rows(), 
                        self.valid_cols(), 
                        self.valid_squares()))
        return False
```    

###[Shortest Word](https://www.codewars.com/kata/57cebe1dc6fdc20c57000ac9)

```python
def find_short(s):
    """Return the length of the shortest word in a string of words."""

    return min(map(len, s.split(' ')))
```

###[IP validation](https://www.codewars.com/kata/515decfd9dcfc23bb6000006)

```python
def is_valid_IP(string):
    """Verify an IP address is valid."""

    valid = [ s.isdigit() 
              and not s[0] == '0'
              and not s == '256'
              for s in string.split('.') ]
              
    if  all(valid)      \
    and len(valid) == 4 \
    and len(string): 

        return True 
    
    return False  
```

###[Area of an annulus](https://www.codewars.com/kata/5896616336c4bad1c50000d7)

```python
def annulus_area(r):
    """Return the annulus area."""
    
    pi = 3.1415926535

    return round(pi*r**2/4,2)
```

###[Find the missing term in an Arithmetic Progression](https://www.codewars.com/kata/52de553ebb55d1fca3000371)

```python
def find_missing(seq):
    """Return the missing term in an Arithmetic progression."""

    diff = seq[1] - seq[0]
    
    for i in range(len(seq)):
        next_diff = seq[i+1] - seq[i]
    
        if  next_diff != diff:
            break
    
        diff = next_diff
    
    return seq[i] + diff
```

###[Human readable duration format](https://www.codewars.com/kata/52742f58faf5485cae000b9a)

```python
def format_duration(s):
    """Return seconds, s, in human readable format."""

    d = [1, 60 ,60, 24, 365]
    ts = []

    while d:
        u, s = divmod(s, reduce(lambda x, y: x*y, d))
        ts.append(u), d.pop()

    us = 'year', 'day', 'hour', 'minute', 'second'
    us = [ u + 's' if t != 1 else u for u, t in zip(us, ts) ] 
    ts = [ '{} {}'.format(t, u) for t, u in zip(ts, us) if t ]

    return ' and '.join(ts).replace(' and',',',len(ts)-2) if ts else 'now'
```

###[How many stairs will Suzuki climb in 20 years?](https://www.codewars.com/kata/56fc55cd1f5a93d68a001d4e)

```python
def stairs_in_20(stairs):
    """How many stairs does a monk climb in 20 years."""
    
    return sum(sum(stairs, []))*20
```

###[Snail](https://www.codewars.com/kata/521c2db8ddc89b9b7a0000c1)

```python
def snail(ar):
    """Return the flattened array unravelled like a curled snake."""
    
    snail = []
    while ar:
        snail.extend(ar.pop(0))
        ar = list(reversed(list(zip(*ar))))
    
    return snail
```


###[Did I Finish my Sudoku?](https://www.codewars.com/kata/53db96041f1a7d32dc0004d2)

```python
import numpy as np

def done_or_not(board=[]):
    """Return True if a sudoku solution is valid."""
    
    if not board:
        
        return 'Finished!'

    board = np.array(board)
    ranges = range(0,7,3), range(9), range(1,10)
    boards = [
        [ list(np.ravel(board[j:j+3, i:i+3])) for i in ranges[0] for j in ranges[0] ],
        [ list(np.ravel(board[i:i+1, :9])) for i in ranges[1] ],
        [ list(np.ravel(board[:9, i:i+1])) for i in ranges[1] ], ]

    for board in boards:
        for section in board:
            for n in ranges[2]:
                if section.count(n) != 1:

                    return 'Try again!'

    return 'Finished!'
```

###[Human Readable Time](https://www.codewars.com/kata/52685f7382004e774f0001f7)

```python
def make_readable(seconds):
    """Return seconds in digital clock format."""

	time = []
	for d in (3600, 60, 1):
		t, seconds = divmod(seconds, d)
		time.append('{}'.format(t).zfill(2))
	
    return ':'.join(time)
```

###[Domino Reaction](https://www.codewars.com/kata/584cfac5bd160694640000ae)

```python
def domino_reaction(s):
    """A domino model. Adjacent, vertical dominoes will fall."""
    
    result = ''
    for i, d in enumerate(s):
        if d in '/ ':
            result += s[i:]
            break
        
        result += '/'

    return result
```

###[Is a number prime?](https://www.codewars.com/kata/5262119038c0985a5b00029f)

```python
def is_prime(num):
    """Return true if num is prime.""" 

    if num < 2:
        return False

    for n in range(num-1, 1, -1):

        if not num % n:

            return False

  return True
```

###[Sum of Digits / Digital Root](https://www.codewars.com/kata/541c8630095125aba6000c00)

```python

# Learned solution. Very nice.
def digital_root(n):
    """Return the sum of digits in n until a single digit."""

    return n % 9 or n and 9

# My solution.
def digital_root(n):
    """Return the sum of digits in n until a single digit."""
    
    if not len(str(n))-1:
        
        return n
    
    return digital_root( sum([ int(i) for i in str(n) ]) )
```      

###[Checking Groups](https://www.codewars.com/kata/54b80308488cb6cd31000161)

```python
def group_check(s):
    """Return True if all brackets, ({{, are complimented in order."""
    
    if not s:
        
        return True
        
    inner, outer, stack = '({[', ')}]', [s[0]]
    
    for c in s[1:]:
        
        if c in inner:
            stack.append(c)
            continue
        
        if stack:
            if inner.index(stack[-1]) == outer.index(c):
              stack.pop()
              continue
        
        return False
    
    if stack:
       
       return False
    
    return True
```

###[Find the missing letter](https://www.codewars.com/kata/5839edaa6754d6fec10000a2)

```python
def find_missing_letter(cs):
    """Return missing letter in letters incremented alphabetically.""" 
    
    for c1, c2 in [ cs[x:x+2] for x in range(len(cs)) ]:

        if not ord(c2) - ord(c1) == 1:

            return chr(ord(c1)+1)
```        

###[Format a string of names like 'Bart, Lisa & Maggie'.](https://www.codewars.com/kata/53368a47e38700bd8300030d)

```python
def namelist(names):
    """Return a list of names as string formatted gramatically."""
   
    n = names

    return ' & '.join([ x['name'] for x in n]).replace(' & ', ', ', len(n) - 2)
```

###[Chess Fun 1: Chess Board Cell Color](https://www.codewars.com/kata/5894134c8afa3618c9000146)

```python
def chess_board_cell_color(cell1, cell2):
    """Return True if two chessboard cells are the same color."""

    return not sum([ ord(x) for x in cell1 + cell2 ]) % 2
```

###[Your order, please](https://www.codewars.com/kata/55c45be3b2079eccff00010f)

```python
import re

def order(sentence):
    """Order the sentence string according to embedded numbers."""

    key = lambda word: re.findall(r'\d', word)[0]
    sentence = sentence.split()
  
    return ' '.join(sorted(sentence, key=key))
```


###[Bit Counting](https://www.codewars.com/kata/526571aae218b8ee490006f4)

```python
def countBits(n):
    """Return the number of 1s in the binary representation of n."""
    
    return bin(n).count('1')
```

###[Vasya - Clerk](https://www.codewars.com/kata/555615a77ebc7c2c8a0000b8)

```python
def tickets(people):
    """Confirm that a clerk can fulfill all transaction change."""

    bank = []

    for person in people:

        if person == 25:
            bank.append(person)
            continue

        if person == 50:
            if 25 in bank:
                bank.remove(25)
                bank.append(50)
                continue

            else:

                return "NO"

        if person == 100:

            if bank.count(25) >= 1 and bank.count(50) >= 1:
                bank.remove(25)
                bank.remove(50)
                bank.append(100)

            elif bank.count(25) >= 3:
                bank.remove(25)
                bank.remove(25)
                bank.remove(25)

            else:

                return "NO"

    return "YES"
```

###[Find the odd int](https://www.codewars.com/kata/54da5a58ea159efa38000836)


```python
from collections import Counter

def find_it(seq):
    """Find the number in seq that appears an odd number of times."""

    count = Counter(seq)
    
    for idx, value in enumerate(count.values()):

        if value % 2:
    
            return list(count.keys())[idx]

    return None
```

###[Two to One](https://www.codewars.com/kata/5656b6906de340bd1b0000ac)

```python
def longest(s1, s2):
    """Return the longest string from unique characters in  s1, s2."""

    return ''.join(sorted(set(s1+s2)))
```

###[Regex validate PIN code](https://www.codewars.com/kata/55f8a9c06c018a0d6e000132)

```python
def validate_pin(pin):
    """Return True if pin has only numbers and is 4 or 6 digits."""

    return all([ digit.isdigit() for digit in pin ]) and len(pin) in (4,6)
```

###[Sum of two lowest positive integers](https://www.codewars.com/kata/558fc85d8fd1938afb000014)

```python
def sum_two_smallest_numbers(numbers):
    """Return the sum of the two lowest positive integers in numbers."""
    
    numbers = [ number for number in numbers if number > 0 ]
    
    return sum([ numbers.pop(numbers.index(min(numbers))) 
        for _ in range(2) ])
```


###[Alien Accent](https://www.codewars.com/kata/5874657211d7d6176a00012f)

```python
def convert(st):
    """Translate 'a's to 'o's and 'o's to 'u's because Aliens."""
    
    table = str.maketrans({'a':'o', 'o':'u'})
    
    return st.translate(table)
```


###[Equal Sides Of An Array](https://www.codewars.com/kata/5679aa472b8f57fb8c000047)

```python
def find_even_index(arr):
    """Return i where the sum of arr left and right of i are equal."""

    for idx, _ in enumerate(arr):
        left = sum(arr[:idx])
        right = sum(arr[idx+1:])

        if left == right:
            return idx

        if left == 1 and right == 1:
            return 1

    return -1
```
