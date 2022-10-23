# Overview
The goal here was to develop and train two different models, and deploy them on GCP using a canary deployment achieved by kubernetes and istio. 
The first model is an 11-layer custom implementation of a resnet that was trained from scratch on CIFAR-10, and the second model uses transfer learning
and has a pretrained 50-layer resnet as a feature extractor. A few Dense layers are added to the end and trained to use the features extracted by the 
pre-trained layers to make predictions on the CIFAR-10 dataset. The deployment is relatively simple, its just a single kubernetes cluster, and an istio
virtual service that routes 70 percent of requests to the 11-layer resnet and the remaining 30 percent to the 50 layer resnet. The only point of the canary 
deployment is to demonstrate that I know how to create a canary deployment. 
## Data
Data is just CIFAR-10, which is quite popular. I use minimal data augmentation by doing random horizontal and vertical flips on the original data set.
## Training
Training was extremely resource intensive, so I trained the 11-layer resnet for approximately 15 epochs before deployment, and the 50 layer resnet for about 
4. The point of this project was not to get some arbitrarily high accuracy, so I decided not to continue training.
## Results
If you open and run the Post request demo notebook, you will see the results of making 20 concurrent requests to the service. The example image is of a cat,
and you will see that while both models correctly classify the image, the 50 layer resnet does so with significantly greater confidence.
