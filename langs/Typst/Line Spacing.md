

# 1.5 Word 

https://forum.typst.app/t/whats-the-equivalent-of-ms-words-1-5-line-spacing/1057
Is the original Forum post that leads down the rabbit hole 

seems to be the solution to get 1.5 spacing otherwiese use the other format that is the first post
```typst
#set par(leading: 0.75em, spacing: 0.75em)
```


## The Whole rabbit hole

From this Github issue https://github.com/typst/typst/issues/159#issuecomment-1609939896

where the following solution is presented

```typst
#let leading = 1.5em // Your line spacing (1, 1.5, 2, etc.)
#let leading = leading - 0.75em // "Normalization"
// #set block(spacing: leading) // This is for an old version of Typst (<=0.11.1).
#set par(leading: leading, spacing: leading)
```


a comment about this 
> With `leading: 0.25em` parentheses do overlap a little (in some fonts more, in others less). For `0.75em` and `1.25em` the error only increases (~1.56 instead of 1.5, ~2.1 instead of 2.0).

Credit to : https://github.com/Andrew15-5 for finding the solution. 

Then it leads to this github issue https://github.com/typst/typst/issues/1221 which is also referenced there 


Then we get to the latest issue, that typst apparently caclualates baseline distance compleatly counter intuitve 
https://github.com/typst/typst/issues/4224#issuecomment-2423254670

while also being inconsistent 


```typst 
#set text(top-edge: 1em, bottom-edge: 0em)
#set par(leading: 0.17em)
```
https://github.com/typst/typst/issues/4224#issuecomment-2755913480

is also potentially a solution to obtain 1.5 Spacing that is eqivalent to word