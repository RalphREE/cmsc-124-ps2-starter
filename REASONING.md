# Reasoning Before Execution

Write the rule behind each case before you commit your predictions. Replace
every placeholder line below with one to three sentences that name the rule
producing that case's value, type, length, and dimensions. One rule is enough.
After you run and find a miss, add a short correction entry under that case in a
later commit.

## P01
Type promotion: an atomic vector holds one type, so c() climbs the ladder
logical, integer, double, character to the highest type present. 3.5 is a
double, so TRUE becomes 1 and 2L becomes 2, giving a double [1,2,3.5].
c() keeps all 3 elements and adds no dimensions.

## P02
Character is the highest type on the ladder, so the string "2" promotes every
other element to character: 1L becomes "1" and TRUE becomes "TRUE". The
3 elements stay in order and a plain vector has no dimensions.

## P03
Arithmetic is element-wise, and the shorter vector is recycled to match the
longer one, so c(1L, 2L) acts as 1,2,1,2 and gives [11,22,31,42]. Integer
plus integer stays integer, the result has the longer length of 4, and there
are no dimensions.

## P04
A positive index selects positions in the order listed, so c(3L, 1L) returns
the third element and then the first, and each value keeps its name. Indexing
does not change the type, so the result is an integer of length 2 with no
dimensions.

## P05
A negative index removes that position, so -2L drops the 7 and keeps 4 and 9
in order. Indexing does not change the type, so the result is an integer of
length 2 with no dimensions.

## P06
x > 10L is NA for the missing element, but TRUE | NA is TRUE, so the mask is
FALSE, TRUE, TRUE, FALSE and logical indexing keeps the 2nd and 3rd elements.
The kept NA is NA_integer_, so [NA,12] is an integer of length 2 with no
dimensions.

## P07
A factor stores integer codes plus level labels, so typeof() reports integer
even though the labels print as text. The explicit levels keeps the order low,
high instead of sorting, the 3 input values become 3 codes, and a factor has no
dim attribute.

## P08
A list keeps unlike types in separate named elements without coercing them, so
scores stays integer and label stays character. typeof() is list, the
length counts the 2 top-level elements, and a list has no dimensions.

## P09
A data frame is a list of equal-length columns, so typeof() says list and
length() counts the 2 columns, not the rows. It also carries rows by columns
as its dimensions: 2 values in each of 2 columns gives 2x2.

## P10
Unlike a data frame, a matrix is a plain integer vector 1..6 stored by columns
with a dim attached, and c(10L, 20L) recycles down that storage order, adding
10 to the 1st, 3rd, and 5th values and 20 to the 2nd, 4th, and 6th. Integer plus
integer stays integer, the length counts all 6 values, and the 2x3 dim from
nrow = 2L is kept.

## P11
Arithmetic with NA yields NA, and mean() keeps the missing value unless told to
remove it. The NA is NA_real_ and the other values are doubles, so the result
is a length-one double [NA] with no dimensions.

## P12
na.rm = TRUE removes the NA and mean() averages the rest, (8 + 10) / 2 = 9.
mean() returns a single double, so the length is 1 with no dimensions.

## P13
1:3 creates an integer vector, and x[2] <- 2.5 is a mixed-mode assignment:
R promotes the whole vector to double so the 2.5 fits. The result is
[1,2.5,3], still 3 elements with no dimensions.

## P14
ifelse() decides per position, setting the NA to 0 and doubling everything
else to 14, 24, 8. The mask adjusted > 15 then caps the 24 at 15, giving
[14,0,15,8]; the values are doubles, the length stays 4, and nothing adds a dim.

## P15
An array is a plain vector with a dim attached, just like a matrix, so its type
is the type of the values: 1:8 is integer. The values stay in storage order,
1 to 8, down the columns; the length counts all 2 x 2 x 2 = 8 values, and the
dim is the 2x2x2 given.

## P16
R indexing starts at 1, so index 0 selects nothing. The result is an empty
vector that keeps its type, so it is integer with length 0 and no dimensions.
