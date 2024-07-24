* I realized that instead of passing in the 2D (28,28) examples to the ML algorithm. I have to flatten the dimensions before sending it in. I remember reading about the intuition for this somewhere. But now with my implementation in the first layer. Each pixel will have a weight associated to it. 
	* I also read in the blog that I need to divide the pixels by 255 to scale it to be values between 0-1. because the pixel scale is 255.
	* He converted the output values to one hot encoding. This is something I am familiar with but why do it? [[Deep Work Machine Learning Exploration]] 07/12/24
* 07/13/24: For the loss function I need to have some kind of percentage or something for the output neurons. So I can be like you are off by this much. Or maybe not?
	* Scaling: I don't think we need to scale the input values to be between 0-1. Its currently 0-255. That is alright. Why do it?
	* Loss Function: So I have output neurons that match the number of neurons we got. And the max output neuron will be the chosen number(expected results between 0-9) and we have neurons 0-9. And it seems like the result can be any number. Unless I slap a loss function that will convert the neurons outputs to probabilities. loss function could be $each\_item / total\_score$. And the $max\_index(probs)$ gets chosen. 
	* I need to put some kind of a constraints on this thing for common issues. Because I have 5 test inputs and I noticed that sometimes all of the outputs are 0.
* 07/14/24: 
	* Loss Function: So I got probs of what is predicted and I will choose the max probs for each training example as the prediction for that training example. So now what do I say the loss is? In a ideal world the neurons would have 0 probability for all the wrong outputs and 100% probability for the correct output. So the loss would be how are off from the ideal it is?? 
		* Example: predicted_probs: [0.1, 0.2, 0.7, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0] -> predicted 2. Correct answer is three though. So losses: $(actual - predicted)^2$. Mean squared error(MSE). That should work.
			* predicted_probs: [0.1, 0.2, 0.7, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
			* actual: [0,0,0,1,0,0,0,0,0,0]
			* $(actual-predicted_probs)^2$: $[0.1^2,0.2^2,0.7^2,1^2,0^2,0^2,0^2,0^2,0^2,0^2 ]$
			* loss for one example: $[0.1^2,0.2^2,0.7^2,1^2,0^2,0^2,0^2,0^2,0^2,0^2 ]$
			* losses will be an array of arrays containing the above for each example
			* Then take the (sum_of_each_example/num_classes) and sum that across all the examples. Then divide by num_of_examples. To end up with a single value we will call data_loss.
			* Then we will call .backwards() on data_loss
		* we are trying to drive all of the losses to zero. How does back propagation tie into the loss function to help with this?
- 07/19/24
	- To drive the loss to zero in which direction should I update my parameters? Should I subtract or add the gradient for each parameter?
		- Does the gradient need to be multiplied by anything (learning_rate is just a constant value we add to control the sizes of the updates so its simple to understand and I am not adding that in yet.)
		- What is the gradient? --> It is the partial derivative of the parameter with respect to the cost function.
			- It is how much to adjust the parameter to reduce the cost function to zero. but HOWWWW?????? How does the gradient know that this is the amount to adjust by to reduce the cost function? I am confused on the amount and which direction it should take. And if the adjustment actually does reduce the cost. 3 questions.
			- The gradient gives the slope. Which in this case is the partial derivative. How is that supposed to inform me of how **much** to go in a certain direction? It just gives the slope. Why would the partial derivative of a parameter with respect to the loss be subtracted or added to minimize the loss to 0?
- 07/20/24
	- The gradient is calculated started with the loss function which gives the error. Which is what we are trying to minimize. So the gradient we get for the parameter is with reference to the error of the slope of the respective change in loss/ respective change in parameter value.
	- With partial derivatives we are keeping all of the variables constant except for one which we are "changing". So the derivative is the (respective change in loss/respective change in parameter value we are changing) with the other parameters held constant.
	- So as far as I know this is correct. And we are just subtracting this value because we know this to be the positive gradient of the loss with respect to (w.r.t) a parameter. What this means is what was said in the previous bullet point. So we are subtracting the parameter by this slope value because... its just the value that might be going towards the optimal point of the lowest point in the loss result.
		- The question still is how do we know that the gradient we calculated is the "how much" to move towards the global minimum in the cost function. I think this is the how much because the slope is the slope at this **certain section** of the cost function graph. And the slope can change past a single slope movement downwards. We are calculating the slope at the point in the cost function that the loss output is at. So the safest bet is to move at most 1 * slope. Anything more like 2 * slope might overshoot the minima because the slope could have changed in that **region**
	- I am kind of bored of this today. I do not feel like exploring it as much. I do not know why. But I am going to put in at least an hour more of work then I need to go get some dinner or make some dinner.
	- Okay so the parameters are not getting their gradients like they are supposed to. But I feel like I have done everything right. So this is going to be hard to fix. Where is the issue?!!! There is also the problem of all of the predicted probabilities coming as zero. That should not be happening. 
		- So there are too many parameters for me to debug the network effectively. I think... So I need to design a toy problem to make it easier to fix. Problems: 1. Why are the gradients not flowing to the parameters. 2. Why are all of the probabilities coming back as 0?
- 07/22/24
	- So it seems like there are gradients flowing back all the way to the first layer but the number of gradients start to decrease after the last layer. Why is that???
	- So I fixed it it was a programming error I believe and not an issue in my neural network code implementation. 
	- TODO: I want to manually back propagate from my loss function just to prove to myself that it will actually update the parameters in a meaningful way.
	- But first finish solving MNIST with the MLP
07/24/24 **Not getting the mean part of mean squared error loss**
- So it seems like the loss function I have in place does not make the predictions get better even when the loss is going down. How is that possible??
	- I need better predictions and I need to influence that by changing the loss function somehow.
- So I get the losses using mean squared error. What is the mean part of this? Its the sum then divide by the number of training examples right
```python 
# Calculate total loss

    # 1. take (sum_of_each_example/num_classes)

    # 2. then data_loss = sum across all the examples / num_of_examples

    # 3. data_loss.backward()
```
* I have this psuedo code. I think the step where I take the average loss between classes is unnecessary. Why do I even take the average anyways? What is that accomplishing why is there a mean in the mean squared error? What is the mean accomplishing?
	* With step one we are spreading out the loss across all of the examples. So if its one example that is causing the most problems we are distributing that error across all of the examples and they are being adjusted equally. So we need to take out the /num_classes\
	* Step 2 justification... I can actually apply the same logic as above to why we should remove dividing by num_of_examples but to calculate a mean you have to do the sum()/num_of_examples somewhere!  
	* How long should it take to overfit a small dataset of MNIST anyways? My execution runs so slow... its ridiculous.