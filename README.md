# Lane-Detection-GANs
Investigating using GANS to help train a lane detection neural network

## Purpose
To investigate whether the usage of a GAN-trained generator can be used as the basis of weights for a semantic segmentation model for lane detection.

## Planned Architectures
1. Check performance of network similar to original network [here](https://github.com/mvirgo/MLND-Capstone) when using square images instead of rectangular, to fit with the norm in training convolutional neural networks.
2. Check performance of the network by adding in batch normalization for all layers, adding skip layers, and trying strides of 2 instead of max pooling.
3. Check performance gained when using an ImageNet pretrained architecture (Inception, ResNet, etc.) as the encoder part of the network, with a similar decoder structure as before for the semantic inference. Check both speed and inference accuracy.
4. Check performance if training the full architecture of an ImageNet pretrained architecture like #3, but without freezing the pretrained layers, and adding that to the original decoder structure.
5. Use a GAN to train a generator to produce fake semantic outputs, with the original real semantic outputs from the dataset as the "real" inputs. 
6. Using the optimal encoder network from steps 1-4, lead into a fully-connected layer that acts similar to the noise vector that the generator made in step 5 would typically start with. Using the optimal encoder plus the generator as decoder, check performance against the original (speed and accuracy). Likely will need to drop final layer of optimal encoder to train (so that it matches to the noise vector for pre-trained decoder), and then also train final layer of decoder. Can try both this method and full re-training with the pre-trained weights to start with.
7. Potentially trade back and forth between a fully convolutional model and the generator portion of a GAN, such that the "embeddings" in the middle of the fully convolutional model are similar to the generator's input noise, and go back and forth until little improvement is seen. 
8. Optimize for inference; again compare for speed and accuracy.

See `adj_preprocess.py` file for pre-processing on the labels for all networks, as well as images for the ImageNet pre-trained networks - this needs to be dropped into the relevant directory for training.

## Results
**In Progress**


**Upcoming Changes** - Based on testing the first four architectures, there actually is surprisingly little difference specific to lane detection. This may be due to the simplicity of the task, where the lane is nearly always in the same location, and it's only the edges of the lane where most changes take place. As such, I might consider re-doing the work on a simplified version of Cityscapes dataset (say on a subset of classes), to potentially get more useful results.

Inference times benchmarked using a GTX 1060.
See `performance.py` file for example evaluation script.

Test dataset is based on annotations of Udacity's challenge video from Advanced Lane Finding.

Test accuracy is based on intersection over union metric.

Architecture | Test Acc | Speed | Parameters
--- | --- | --- | ---
arch-1 | 0.9768 | 5.41 ms | 725,101
arch-2 | 0.9771 | 5.18 ms | 725,101
arch-3 | 0.9518 | 41.47 ms | 27,226,273
arch-4 | 0.9764 | 83.43 ms | 25,441,345

`arch-1` and `arch-4` are also fairly visually appealing, `arch-2` is pretty good as well, while `arch-3` seems to perform poorly when visualized.
