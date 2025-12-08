
#  ** REGULAR EXPRESSIONS & DETAILED EXPLANATIONS**

---

# **I. BASIC CHARACTER MATCHING **

### **1. `.`**

Matches: **Any single character** except newline.  
Explanation: A wildcard; stands for one arbitrary character.

### **2. `\d`**

Matches: **Any digit **.  
Equivalent to `[0-9]`.

### **3. `\D`**

Matches: **Any non-digit**.

### **4. `\w`**

Matches: **Word characters** → letters, digits, underscore  
Equivalent to `[A-Za-z0-9_]`.

### **5. `\W`**

Matches: **Any non-word character**.

### **6. `\s`**

Matches: **Whitespace** (space, tab, newline).

### **7. `\S`**

Matches: **Non-whitespace**.

### **8. `[abc]`**

Matches: **One character that is either a, b, or c**.

### **9. `[^abc]`**

Matches: **One character except a, b, or c**.  
Caret (`^`) negates the character set.

### **10. `[a-z]`**

Matches: **Lowercase letters a through z**.

### **11. `[A-Z]`**

Matches: **Uppercase A through Z**.

### **12. `[0-9]`**

Matches: **Any digit**.

### **13. `[a-zA-Z]`**

Matches: **Any alphabetic character**.

### **14. `[^0-9]`**

Matches: **Any non-digit character**.

### **15. `[a-zA-Z0-9_]`**

Matches: **Any alphanumeric character + underscore**.

---

# **II. QUANTIFIERS **

### **16. `a*`**

Matches: **0 or more** occurrences of "a".  
Greedy: takes as many as possible.

### **17. `a+`**

Matches: **1 or more** occurrences of "a".

### **18. `a?`**

Matches: **0 or 1** occurrence of "a".  
Usually used for _optional_ characters.

### **19. `a{3}`**

Matches: **exactly 3** occurrences.

### **20. `a{3,}`**

Matches: **3 or more** occurrences.

### **21. `a{3,5}`**

Matches: **between 3 and 5** occurrences.

### **22. `[0-9]+`**

Matches: **One or more digits** (an integer).

### **23. `\w{4,8}`**

Matches: **Word with length between 4 and 8**.

### **24. `\s*`**

Matches: **any amount of whitespace—including none**.

### **25. `\s+`**

Matches: **one or more whitespace characters**.

### **26. `(abc)+`**

Repeats the **entire group** `abc`.

### **27. `(ab)*`**

Repeats “ab” **zero or more times**.

### **28. `a*?`**

_Non-greedy_: **minimal match**.

### **29. `a+?`**

Matches **one or more**, but minimal.

### **30. `.*?`**

Matches as few characters as possible—commonly used in parsing HTML.

---

# **III. ANCHORS **

### **31. `^`**

Matches: **Start of string or line**.

### **32. `$`**

Matches: **End of string or line**.

### **33. `\b`**

Word boundary:  
Matches between a **word char and non-word char**.

### **34. `\B`**

Non-boundary:  
No separation between word/non-word.

### **35. `^abc`**

Matches: “abc” **only at the beginning**.

### **36. `abc$`**

Matches: “abc” **only at the end**.

### **37. `^\d+$`**

Matches: **entire string is only digits**.

### **38. `^\s*$`**

Matches: **string that is empty or only spaces**.

### **39. `\bcat\b`**

Matches “cat” as a **whole word**.

### **40. `(?m)^`**

Multiline start anchor.

---

# **IV. GROUPING & ALTERNATION **

### **41. `(abc)`**

Capturing group – treated as one unit.

### **42. `(?:abc)`**

Non-capturing group (does not store match).

### **43. `a|b`**

Matches **a OR b**.

### **44. `(foo|bar)`**

Matches either "foo" or "bar".

### **45. `(gr(e|a)y)`**

Matches: “grey” or “gray”.

### **46. `(https?|ftp)`**

Matches: `http`, `https`, or `ftp`.

### **47. `(19|20)\d{2}`**

Matches: **years 1900–2099**.

### **48. `(a(bc))`**

Nested groups – captures “bc” as group 2, “abc” as group 1.

### **49. `((\d{3})-(\d{4}))`**

Captures a pattern and its sub-components.

### **50. `(?:\w+\.)+`**

Matches multiple words ending with `.` (like “[www.”](http://www.xn--ivg/)).

### **51. `(\d{1,3}\.){3}\d{1,3}`**

Matches the 4 dotted blocks of an IP.

### **52. `(foo)?bar`**

Matches “bar” or “foobar”.

### **53. `a(b|c)+d`**

“a”, then b or c repeated, then “d”.

### **54. `(jan|feb|mar)`**

Case of non-capturing alternative options.

### **55. `(ab){2,4}`**

Repeats the whole “ab” group 2–4 times.

---

# **V. LOOKAHEADS / LOOKBEHINDS **

### **56. `a(?=b)`**

Positive lookahead:  
Matches **a only if followed by b**.

### **57. `a(?!b)`**

Negative lookahead:  
Matches **a only if NOT followed by b**.

### **58. `(?<=a)b`**

Positive lookbehind:  
Matches **b only if preceded by a**.

### **59. `(?<!a)b`**

Negative lookbehind:  
Matches **b only if NOT preceded by a**.

### **60. `\d+(?= dollars)`**

Matches numbers **before the word "dollars"**.

### **61. `(?<=\$)\d+`**

Matches digits **after a $ sign**.

### **62. `foo(?=bar)`**

Matches “foo” only if followed by “bar”.

### **63. `foo(?!bar)`**

“foo” not followed by “bar”.

### **64. `(?=.{8,})`**

Ensures entire string has **at least 8 chars**.

Used in password validation.

### **65. `^(?=.*\d)`**

Asserts: contains **a digit**.

### **66. `^(?=.*[A-Z])`**

Asserts: contains **uppercase letter**.

### **67. `^(?=.*[a-z])`**

Asserts: contains **lowercase letter**.

### **68. `^(?=.*[!@#$%^&*])`**

Asserts: contains **special symbol**.

### **69. `^(?=.*\d)(?=.*[A-Z]).+$`**

Combined password rule:  
Must contain digit + uppercase.

### **70. `(?<!\d),(?=\d)`**

Matches comma **between digits** (e.g., thousands separators).

---

# **VI. EMAIL, URL, AND NETWORK **

### **71. `\w+@\w+\.\w+`**

Very simple email pattern.

### **72. `[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[A-Za-z]{2,}`**

Better email validator.

### **73. `https?://[^\s/$.?#].[^\s]*`**

Matches URLs starting with http/https.

### **74. `(ftp|http|https):\/\/\S+`**

General URL.

### **75. `www\.\w+\.\w+`**

Simple www URL.

### **76. `(\d{1,3}\.){3}\d{1,3}`**

IPv4 format.

### **77. `(([0-9A-Fa-f]{1,4}:){7}[0-9A-Fa-f]{1,4})`**

IPv6 format.

### **78. `([0-9a-fA-F]{2}:){5}[0-9a-fA-F]{2}`**

MAC address.

### **79. `^[0-9]{5}(?:-[0-9]{4})?$`**

US ZIP code.

### **80. `\d{3}-\d{2}-\d{4}`**

US Social Security Number (format only).

### **81. `\(\d{3}\)\s?\d{3}-\d{4}`**

Phone number format: (123)456-7890.

### **82. `\d{3}-\d{3}-\d{4}`**

Phone number 123-456-7890.

### **83. `\+\d{1,3}\s?\d+`**

International phone number.

### **84. `[A-Fa-f0-9]{32}`**

MD5 hash.

### **85. `[A-Fa-f0-9]{40}`**

SHA-1 hash.

---

# **VII. TEXT PROCESSING & STRING VALIDATION **

### **86. `^[A-Za-z]+$`**

Letters only.

### **87. `^[A-Za-z0-9]+$`**

Alphanumeric only.

### **88. `^[\w\s]+$`**

Letters + digits + spaces.

### **89. `^[A-Z][a-z]+$`**

Capitalized word (e.g., “London”).

### **90. `[.!?]$`**

Sentence ending punctuation.

### **91. `^[^@]+@[^@]+$`**

String with **one @ symbol**.

### **92. `".*?"`**

Double-quoted string (non-greedy).

### **93. `'.*?'`**

Single-quoted string.

### **94. `\([^)]+\)`**

Text within parentheses.

### **95. `\[[^\]]+\]`**

Text inside square brackets.

### **96. `<[^>]+>`**

HTML tag.

### **97. `<!--[\s\S]*?-->`**

HTML comment.

### **98. `&[a-zA-Z]+;`**

HTML entity (e.g., `&nbsp;`).

### **99. `\d{4}-\d{2}-\d{2}`**

Date in **YYYY-MM-DD** format.

### **100. `(0?[1-9]|1[0-2])/(0?[1-9]|[12]\d|3[01])/\d{4}`**

Date in **MM/DD/YYYY** format, with validation of numbers.

---

# ✔️ If you want, I can also provide:

- A **PDF version** of the full list
    
- Code examples in **Python, JavaScript, C#, Java, or Bash**
    
- A **flash-card study version**
    
- A **cheat sheet graphic**
    
- Practice exercises with answers
    

Would you like that?