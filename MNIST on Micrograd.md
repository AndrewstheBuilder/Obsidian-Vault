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