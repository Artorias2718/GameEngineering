# Assignment 5.12: Neural Networks

## Requirements

- Start from the **Genetic Algorithms** assignment
  - Remove the **Command Sequence**... the set of commands each tank AI blindly follows.
  - The positions of the mines should now randomize at the start of every round.
  - The mines should be removed when collected.
- Create a Neural Network
  - The Neural Network should have 1 input layer, at least 1 hidden layer, and 1 output layer.
  - The output layer must have 2 nodes - these produce the values for the left and the right tread. (Use a TanH activation functionLinks to an external site..)
  - The input layer should include:
    - Relative heading to the nearest mine
    - Distance to the nearest mine.
    - *optional* any other information you think might be helpful.
- Use the Genetic Algorithm to adjust the weights
  - Each tank AI must have its own set of weights for the Neural Network.
- Submit both an executable AND a video showing the app performing at what you consider to be "fully trained".

Tips:
  - Use TanH activation functions for hidden layers.
  - Neural Networks work better when the input range matches the output range. For example: we normally express "heading" in degrees with the range of [-180° to 0° to 180°]. Instead, use the range [-1.0 to 0.0 to 1.0].
  - Use a Bias neuron on the input layer and hidden layers.
- [Neural Network basics](https://en.wikibooks.org/wiki/Artificial_Neural_Networks/Neural_Network_Basics)
- [Neural Network tanks tutorial](http://www.ai-junkie.com/ann/evolved/nnt1.html)
- [Online book](http://neuralnetworksanddeeplearning.com/) about **Deep Learning**