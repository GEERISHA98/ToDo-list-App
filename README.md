# Instructions for Running the Code:
Since all my code is contained within a single Jupyter notebook, running it is quite straightforward. 
Please follow these steps to ensure everything operates smoothly:

# Input your Kaggle username and API key to enable the dataset download.
Enter your Weights & Biases (wandb) API key for experiment tracking.
Execute all the notebook cells. For best performance, use a Google Cloud Engine (GCE) instance equipped with an NVIDIA T4 GPU and high-memory (n1-highmem-4) settings to replicate my computational environment.

# Model Architecture Details:
- I explored several architectural designs, including inverse pyramid, funnel, diamond, and cylinder shapes.
- Through experimentation, I found that GELU activation function outperformed others like ReLU, ELU and LeakyReLU in terms of results.
- A dropout range of 0.1 to 0.5 was tested, with 0.2 yielding the highest accuracy. Furthermore, reducing dropout towards the network's end enhanced performance significantly.
- The funnel architecture emerged as the most effective, described below:

Total parameters: 19.16113M.
It starts with a linear layer of 2048 neurons, followed by batch normalization, GeLU activation, and a 0.1 dropout.
Four subsequent linear layers each contain 2048 neurons, batch normalization, GeLU activation, and a 0.2 dropout.
The next layers include one with 1024 neurons and another identical layer, both followed by batch normalization, GeLU activation, and dropouts of 0.1 and 0.05, respectively.
A penultimate layer with 512 neurons follows, incorporating batch normalization, GeLU activation, and a 0.02 dropout, leading to the output layer.

# Hyperparameters:

Epochs: 40
Dropout rates explored: 0.1, 0.2, 0.02
Batch size: 1024. 512 and 2048 btach sizes were also tested but showed almost nil impact on accuracy.
Initial learning rate: 0.001
Optimizer Choice:
Out of SGD, RMSProp, Adam, and AdamW tested, Adam delivered the best outcomes with minor differences. 

# Learning Rate Scheduler:
While experimenting with an Exponential LR scheduler (gamma range: 0.1-0.3), switching to ReduceLROnPlateau in combination with the GeLU activation offered a significant advantage, aiding in surpassing the middle accuracy threshold. The effective parameters for my model were a patience of 3, mode set to "min," and a reduction factor of 0.1. I noted that a higher patience of 5 or 7 delayed LR adjustments too much, missing minor yet crucial accuracy variations.

# Data Processing and Loading:

Implementing cepstral normalization enhanced model accuracy by approximately 8-9%.
I utilized the default dataloaders from the provided starter notebook without modifying any attributes for the training, validation, and testing sets.

## NOTE: 

# In order to access my final run wandb log, use the eblwo API:
import wandb
run = wandb.init()
artifact = run.use_artifact('geerishj/hw1p2/run-an53jkth-history:v0', type='wandb-history')
artifact_dir = artifact.download()

# Wandb Report 
Access the wandb report on val accuracy, train accuracy, val loss, train loss and lr using this link: https://api.wandb.ai/links/geerishj/lw1ptmlw 

