# Genetic-CarRacing-v2
Solving Gym's car racing environment with genetic algorithms, neural networks, and auto-encoders.

# Encoding
- Compressing the image observation into a 1D vector
- Allows for faster evolutionary training 
- Simplifies genetic model structure

## 1. Data Collection
- Random actions
- Save every nth image
  ### Image Procesing
  - Convert to grayscale
  - Remove grid patterns to reduce complexity
  <br>

  | Original Image | Processed Image |
  | :----------------: | :----------------: |
  | ![Original Image 1](media/original1.png) | ![Processed Image 1](media/processed1.png) |
  | ![Original Image 2](media/original2.png) | ![Processed Image 2](media/processed2.png) |




  ### Stacking Frames
  - Stack 3 consecutive frames
  - Determine velocity & acceleration
  
  <br>
  
  | Slow | Fast |
  | :----------------: | :----------------: |
  | ![Slow Stacked](media/slow_stacked.png) <br>Thin outline indicates little difference <br> between frames| ![Fast Stacked](media/fast_stacked.png) <br>Wide outline indicates larger difference <br> Vehicle has greater velocity magnitude|

## 2. Model Architecture
- Compresses 96x96x3 image input into vector of 32 values
- 99.8843% reduction
- Later feed output of encoder into agent model
### Auto-encoder
![Autoencoder](media/autoencoder.png)

  
## 3. Training
- Utilizing Google Colab's Tesla T4 GPU
<br>

![Loss Graph](media/loss_chart.png)

# Evolutionary Algorithm
Producing the best agent through genetic selection

## Agent Model
  - Impemented in vanilla NumPy for speed and flexibility (reduces uneccesary overhead)
  - Potentially replace with pyTorch for larger models 
  - Shown below is the model archtecture for each individual

![Agent Model](media/agent_model.png)



## Reporoduction
  - The most successful agents of each generation make up the base population for the next generation
  - Each one also produces x number of "mutated" offspring with random values added to each weight
  - The table below indicates how many offspring each individual produces to maintain the population size of 30


Rank   | Mutant Offspring
| :------------: | :-------------: |
|1      |        10        |
|2      |        5         |
|3      |        4         |
|4      |        3         |
|5      |        2         |
|6      |        1         |
|Base      |        5         |
|Total Agents      |        30         |




## Training
![Max Reward Graph](media/reward_chart.png)
(Max Reward)

## Results

| <center>**0 Generations**</center> | <center>**20 Generations**</center> |
| :------------: | :-------------: |
| ![0 Generations](media/runs/test1.gif) | ![20 Generations](media/runs/test20.gif) |
| <center>**60 Generations**</center> | <center>**110 Generations**</center> |
| ![60 Generations](media/runs/test50.gif) | ![1300 Generations](media/runs/test130.gif) |



## Looking Back
  Yeah I know, this agent would be pulled over for DUI. In every simulation, the car learned the same winding behavior indicating that this behavior is not a local optima but rather provides some sort of strategic advantage. Of course it, with the nature of black box nueral networks, would require further testing to determine the exact cause. I'm under the impression that removing the tiling patterns on both the grass and the track elimated some indicators to determine the velocity of the car. On a perfectly straight track, each frame is identical to the next, providing no information regarding the velocity of the agent. By twisting through the track, the overlaying of images produces varience to determine the magnitude of direction of the car. Obviously, it is possible that the winding pattern was simply easier for the model to learn rather than continous actions. Outputing "left" for 5 iterations and "right" for the next 5 could merely be the result of large weight values coupled with the final sigmoid activation function.
  
  ### Potential Improvements:
  - Maintain image complexity 
  - Integrate the encoder into the agent model and make the weights mutable
  - Implement mutations which change the architacture of the agent model rather than soley weight values (and record total reward/elapsed time to encourage smaller, faster models)


  

