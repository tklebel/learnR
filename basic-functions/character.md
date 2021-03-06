

# Character functions

Previously we introduced character vector that is used to deal with text data. In contrast with many other programming languages, character vector is not a vector of a bunch of characters like `a, b, c, ...` but a vector of strings.

R also provides a variety of built-in functions to deal with character vector. Many of them also perform vectorized operations so that it can process numerous strings at the same time.

In this section, we will know how to print, combine, and transform texts in character vectors.

## Printing texts

Perhaps the most basic thing we can do with texts is to view them. R provides several ways to view texts in the console.

The simplest way is directly type the text

```r
> "Hello"
```

```
[1] "Hello"
```

or the name,


```r
> str1 <- "Hello"
> str1
```

```
[1] "Hello"
```

or use `print()` to explicitly print it.


```r
> print(str1)
```

```
[1] "Hello"
```

then the character vector is printed out with indices.

In some cases, we want the texts to appear as a message rather than a character vector with indices, we can call `cat()` or `message()`.


```r
> cat("Hello")
```

```
Hello
```

We can construct the the message in a flexible way.


```r
> name <- "Ken"
> userof <- "R"
> cat("Hello,",name,"- a user of",userof)
```

```
Hello, Ken - a user of R
```

The space character is by default used as the separator between the arguments in the output, or we can change it by specifying `sep=` argument.

An alternative function is `message()` which is often used in serious situations like error or important event. And the output text has more conspicuous appearance. It distincts from `cat()` in that it does not use separator.


```r
> message("Hello, ",name," - a user of ",user)
```

```
Error: object 'user' not found
```

Therefore we show the same text, we need to write the separators manually.

## Combining texts

In practice, we often need to combine several texts to build a new one. `paste()` function works by combining several character vectors together. This function also use space as the default separator.


```r
> paste("Hello","world")
```

```
[1] "Hello world"
```

```r
> paste("Hello","world",sep="-")
```

```
[1] "Hello-world"
```

If we don't want the separator, we can set `sep=""` or alternatively call `paste0()`.


```r
> paste0("Hello","world")
```

```
[1] "Helloworld"
```

The previous examples show what the functions do with single-element character vectors. What about multi-element ones?


```r
> paste(c("A","B"),c("C","D"))
```

```
[1] "A C" "B D"
```

We can see that `paste()` works element-wisely, that is, `paste("A","C")` first, then `paste("B","D")`, and finally the results are collected to build a character vector of length 2.

If we want the results to be put together in one string, we can specify how these two elements are connected by setting `collapse=`.


```r
> paste(c("A","B"),c("C","D"),collapse = ", ")
```

```
[1] "A C, B D"
```

If we want to print such a string in multilines, we can set `collapse` to be `\n` (newline) and call `cat()`:


```r
> cat(paste(c("A","B"),c("C","D"),collapse = "\n"))
```

```
A C
B D
```

The same thing also works with `paste0()`.

## Transforming texts

It is easy to perform basic transformations on texts.

### Changing cases

`tolower()` change the texts to lower capital letters while `toupper()` does the opposite.


```r
> tolower("Hello")
```

```
[1] "hello"
```

```r
> toupper("Hello")
```

```
[1] "HELLO"
```

This is particularly useful when a function accepts character input. For example, we define a one-line function which returns `x+y` when `type` is `"a"` or `"A"` and `x*y` when `type` is otherwise. The best way to do it is convert `type` to lower or upper case no matter what the original value is.


```r
> calc <- function(x,y,type) if(tolower(type)=="a") x+y else x*y
> c(calc(2,3,"a"),calc(2,3,"A"),calc(2,3,"q"))
```

```
[1] 5 5 6
```

This makes the function more robust to similar and non-distinctive inputs.

### Counting characters

Another useful function is `nchar()` which simply counts the number of characters.


```r
> nchar("Hello")
```

```
[1] 5
```

```r
> nchar(c("Hello","R","User"))
```

```
[1] 5 1 4
```

### Substring

In previous chapters, we know how to subset vectors and lists. We can also subset the texts in a character vector by calling `substr()`.


```r
> ch1 <- "Hello"
> substr(ch1,1,3)
```

```
[1] "Hel"
```

```r
> substr(ch1,1,3) <- "abc"
> ch1
```

```
[1] "abclo"
```

`substr()` also has a counterpart function with assignment, which allows us to replace a substring of a text.

### Splitting texts

Some texts have regular separator such as comma. To extract the information in them, we need to split the texts and make each part accessible. `strsplit()` is able to split texts by specific separators in a character vector.


```r
> strsplit("a,bb,ccc",split = ",")
```

```
[[1]]
[1] "a"   "bb"  "ccc"
```

```r
> strsplit(c("Ken,24,Finance","James,25,Economics"),split = ",")
```

```
[[1]]
[1] "Ken"     "24"      "Finance"

[[2]]
[1] "James"     "25"        "Economics"
```

`strsplit()` returns a list of character vectors containing splitted parts because it works element-wisely. In fact, the function is more powerful than is shown. It also supports *regular expressions*, a very powerful framework to process text data. We will cover this topic later.

## Formatting texts

Concatenating texts with `paste()` is sometimes not a good idea because the text is broken into pieces and becomes harder to read as the format gets longer.

### sprintf

In this case `sprintf()` solves the problem and provides a syntax for us to format the text parts. For example,


```r
> sprintf("name: %s, age: %d","Ken",24)
```

```
[1] "name: Ken, age: 24"
```

The functions requires a *format* text in which we write the text with some parts represented by place holders like `%s` and `%d`. Following the format text, we specify additional arguments as the values to replace those place holders.

In the above example, `%s` stands for *string* and `%d` for *digits* (integers). More than representing different types of strings, the format is able to incorporate a uniform formatting syntax. The following table shows the most commonly used syntax.

| Format | Output |
|--------|--------|
| `sprintf("%s","A")` | A |
| `sprintf("%d",10)` | 10 |
| `sprintf("%04d",10)` | 0010 |
| `sprintf("%f",pi)` | 3.141593 |
| `sprintf("%.2f",pi)` | 3.14 |
| `sprintf("%1.0f",pi)` | 3 |
| `sprintf("%8.2f",pi)` |     3.14 |
| `sprintf("%08.2f",pi)` | 00003.14 |
| `sprintf("%+f",pi)` | +3.141593 |
| `sprintf("%e",pi)` | 3.141593e+00 |
| `sprintf("%E",pi)` | 3.141593E+00 |

`%` in the format text is a special character and will be interpreted as the initial character of a place holder. What if we really mean `%` in the string? To avoid such interpretation, we need to use `%%` to represent a single `%`. The following code is an example.


```r
> sprintf("The ratio is %d%%",10)
```

```
[1] "The ratio is 10%"
```

### rprintf package

I created a package named `rprintf`, an advanced version of `sprintf()`. It is a set of functions that make it easier to build formatted texts in R. In addition to C-style string formatting provided by `sprintf()`, this package also provides name-based and number-based formatting. 


```r
> library(rprintf)
> rprintf("My name is $name, and I'm $age years old.",name="Ken",age=25)
```

```
[1] "My name is Ken, and I'm 25 years old."
```

For more details, visit its [website](http://renkun.me/rprintf),  its page on [CRAN](http://cran.r-project.org/web/packages/rprintf/), or view the project on [GitHub](https://github.com/renkun-ken/rprintf).

## Regular expressions

