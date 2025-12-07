# Class 4 Example  
**Author:** Rachel (A16859649)  
**Date:** 2025-10-09  

---

## Plotting sin(x) in R

```r
x <- 1:50
plot(x, sin(x))
```

This produces a scatterplot of the sine function over the range 1 to 50.

---

## Line Plot Version

```r
plot(x, sin(x), type = "l", lwd = 3, col = "purple")
```

This creates a purple line plot with thicker lines, showing the shape of the sine wave more clearly.
