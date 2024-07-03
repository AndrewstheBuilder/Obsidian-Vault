* What is variance, covariance, standard deviation? Give intuitions for the purpose of their existence and usage.
* Why do we have to square the differences for the variance calculation?
  - Example a. $\frac{|4| + |4| + |−4| + |−4|}{4} = \frac{4 + 4 + 4 + 4}{4} = 4$
  - Example b. $\frac{|7| + |1| + |−6| + |−2|}{4} = \frac{7 + 1 + 6 + 2}{4} = 4$
  see how both of those are the same value even though the bottom example has higher changes. So what has to happen is that we have to square the differences between the (value - mean) to exaggerate larger differences over smaller ones. And that will give us a higher variance for the bottom example vs. the top one.
	- Example a. $\frac{4^2 + 4^2 + 4^2 + 4^2}{4} = 16$
	- Example b. $\frac{7^2 + 1^2 + 6^2 + 2^2}{4} = 47.5$