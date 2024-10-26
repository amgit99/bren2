
| Property         | Value                        |
| ---------------- | ---------------------------- |
| 📅 Date          | 30-08-2024, 09:34            |
| 🏷️ Tags         | #algorithms, #string         |
| 🔗 Related Notes | [[Brain expanding problems]] |
This beautiful and terrifying algorithm allows one to find all instances of a pattern in a given text. And does so without wasting any comparisons, giving an overall complexity of $O(m+n)$ where $m$ is the size of the text and $n$ is the size of the pattern.

In the naive approach of pattern matching one slides the pattern over the text and checks if there is a match at every position. If the matching fails one needs to slide the sequence by one position and repeat again so as to not miss any patterns. This yields a complexity of $O(m*n)$ this blows out of proportion when the sizes get big. 

Knuth Morris Pratt banks on the idea of shifting the pattern optimally so as to not miss any potential solutions based on the current information at hand. It turns out that all the data required to make that decision of shifting optimally is present in the pattern and solely dependent on it.
For finding this optimal shift we can look at the LPS array.
### Longest Prefix Suffix Array
This array holds the length of the longest proper prefix that is also a proper suffix at every index of the string. The diagram below shows the lps array and the string it was created from. The reason why this array is relevant is to check which was the longest prefix which was also a suffix at the point before the mismatch.
![[kmp1.png]]
Looking at the example below, we can see that the LPS value for index 11 (one place before the mismatch) is 6, this means the suffix of length 6 is same as the prefix of length 6, this would mean shifting the pattern so that it begins from index 6(obtained from LPS index 11 minus LPS value 6) would guarantee a match of length 6 from the start. ![[kmp2.png]]
<span style="color:#e1db3d">The significance of the suffix part</span> is that, we have already performed the computation of matching the pattern so far *(the matching grows on right end, revealing suffixes)* now, if we know a prefix *(something that begins from the start)* that is equal to a suffix that ends at the point before mismatch we can use that information to align the string and save up on matching. Since the suffix until the point of mismatch is matching and the prefix is same as the suffix, we don't need to rematch that much section (equal to size of the prefix which is also the suffix)
![[kmp3.png]]This algorithm guarantees that we are not missing any other occurrences because we choose the longest prefix that is also the suffix, which means we are shifting the string at the first occurrence of a suffix (hence the longest). The condition that it must be a valid LPS is also important, if the prefix doesn't match the suffix we can't reuse the matching computation for the prefix that we did up to the point of mismatch for the suffix.
And choosing a shorter prefix is just plain stupid, as one may miss out on a pattern that starts where the longest suffix starts.

### Finding the values of the LPS array in Linear Time


(\[\[)(\w*)(\s*)(\w*)_(\]\])
