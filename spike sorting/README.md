# spike sorting
## Definition and motivation of spike sorting
During extracellular recording, a single electrode often picks up electrical signals from many nearby neurons. As a result, the recorded data typically contains multiple spike shapes, each corresponding to a different neuron. Since these neurons may have distinct functions, it’s important to separate and analyze them individually. ***Spike sorting*** refers to the process of using algorithms to automatically identify and distinguish spikes from different neurons in data recorded by one electrode.

## Key considerations for spike sorting
Even though spike shapes from different neurons can look quite similar, several key factors help distinguish them: 
1. Distance to the electrode tip: Neurons that are closer to the electrode produce spikes with larger amplitudes, while those further away appear smaller.
2. Recording location on the neuron: Even at the same distance, the spike waveform can vary depending on which part of the neuron the electrode is closest to—for example, the dendrite, axon, or soma.
3. Membrane properties: Different neuron types, or different segments of the same neuron, have varying types and densities of ion channels. This diversity affects the spike waveform’s shape and timing.

## Spike sorting pipeline
1. First, we can segment the continuous electrical signal (data) to extract individual spikes, also referred to as “snippets,” for further analysis. Spike detection is typically performed by setting a voltage threshold—when the signal crosses this threshold, it is identified as a spike. Then extracted a short segment, such as a 1 ms window centered around this event, as the snippet for further analysis. The threshold is usually set as:
thresholds = data_mean - 3.3 * data_std;
**Note**: We usually set this threshold to a negative value. This is because, in extracellular recordings, neural spikes generally manifest as prominent negative deflections. Therefore, setting a negative threshold allows for reliable detection of these spike events.

2. After collecting all the snippets from a single electrode, each snippet consists of several dozen sampled points, let's take 50 points as an example. Each snippet can thus be viewed as a single data point in a 50-dimensional space. In this high-dimensional space, spikes originating from the same neuron tend to cluster tightly together, which in practice makes it difficult to differentiate them using standard clustering techniques. To address the challenge of high dimensionality, dimensionality reduction methods such as **Principal Component Analysis (PCA)** are commonly employed. PCA identifies directions of greatest variance within the dataset and projects the original high-dimensional data onto a smaller number of principal components. By reconstructing the data using only the first few principal components, much of the noise and redundant information can be removed, thereby making subsequent analyses such as clustering more effective.

3. In the final step, we can plot the selected principal components in the principal component space, enabling visualization and classification of the snippets. Furthermore, by applying algorithms such as k-means to the principal components, it can automatically group the snippets into clusters corresponding to different types of neurons.