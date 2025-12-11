# aoc.xlsx
Solving AOC2025 in Excel

## Day 1

Question 1 asks how many times a combination lock numbered 0-99 stops at 0, given a series of R/L turns as input. Splitting the input is straightforward with `LEFT` and `RIGHT`. To find where the dial stops after each turn, you take the previous position and add the offset from the latest turn, modulo 100. The answer is just a `COUNTIF` to determine how many of the entries of that column are 0.

Question 2 asks for how many times the dial crosses 0, not just stops at it. We can still find this mathematically: if we're turning right, just take the quotient of the previous position plus the adjustment all divided by 100. For example, if we're starting at 80 and turning right by 350, we would cross 0 four times, and `QUOTIENT(80+350, 100)=4`. Turning negatively is a bit more complicated since there's a special case if we're starting at 0. To make things clearer when using quotient, we'll subtract 100 from our start number so that we get the same properties on the quotient as we did for the positive side. For example, starting at 20 and turning right by 240 would cross 0 three times, and `QUOTIENT(-100+20-240, 100)=3`. The special case is starting at 0 because we don't want to count starting at 0 as a crossing, so we include a small adjustment with an `IF` condition.

## Day 2
Here, invalid numbers were ones where the first half and second half were equal. Given a range as input, we can figure out what the first and last invalid numbers in the range are. On the lower end, if the first half of the number is bigger than the second half, then the first half repeated twice is in the range. Otherwise, the first half plus one repeated twice is in the range. For example, if the range is 2113-5000, the lowest possible invalid number is 2121 since 21 is bigger than 13. If the range was 2132-5000, the lowst invalid number is 2222. We can use the same logic, inverted, on the top end. Once we have the low and high bounds, we can find the sum of all of them by using the sum from 1 to n formula `n * (n + 1) / 2`.

In question two, we had to generalize this process to any number of groupings. The logic for figuring out the lower bound is a bit more complicated but basically involves splitting the range bounds into first part and rest of the parts (e.g. for 123456 split 3 ways, the first part would be 12 and the second would be 3456), and then figuring out if the first part repeated is bigger or smaller than the end part. Since 1212 is less than 3456, 131313 would be the first invalid number for a range starting with 123456.

We can do this for each possible number of groupings from 1 to 10, but there are some numbers which are invalid in multiple ways. For example, 25252525 is invalid if you look at it as two groups (2525 repeated twice) or four (25 repeated four times). My approach was hacky, but worked: only look at groupings that were prime numbers (2, 3, 5, 7) since if a number is invalid in four or eight groups, it'll also be invalid in two (XYXYXYXY is 4 groups repeated twice, but also two groups of XYXY repeated once) so we aren't double counting. We just have to subtract off any groups whose number is a products of those primes (e.g. 6 and 10) since there are numbers which are invalid for both 2 and 3 groups. It was hacky but worked!

## Day 3

TBD, did at home

## Day 4
For this one, I basically built a sheet where each generation is its own grid and references the one above it to check if it should remove the roll of toilet paper. The logic here is straightforward, the tricky part was designing a sheet in a way that made it efficient to copy/paste it since it ended up requiring nearly 90 generations to complete. I had a much more labor intensive process to start which used a new sheet for each generation, but using a single sheet lets you copy the formulas for multiple generations at once which lets you go exponentially faster.

## Day 5
The first part was simple: I just built a grid with the ranges as rows and ingredient IDs as columns, and then in the table, I checked if the ingredient ID was in that range. Then, I checked if the ingredient ID matched any ranges and summed those up to get the answer.

For the second part, you had to figure out the size of all the ranges, accounting for the fact that they could overlap. The approach here was to sort the ranges in ascending order. If a range is fully contained by another one, it means its end will be less than or equal to the max end that we've seen so far, so we shouldn't include it in our count. If a range is partially overlapping, that means it starts before the end of a range we've seen so far. In this case, we just move the start of the range up to 1 more than that biggest end. Then, we count the size of all the ranges!
