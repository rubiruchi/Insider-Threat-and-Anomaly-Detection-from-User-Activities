# Unsupervised Learning of User Activities to Detect Intrusions and Insider Threats using Deep Learning
Anomaly detection in network traffic and event logs using deep learning (w/ Pytorch)


## Domain Background
Cybersecurity monitoring is a domain in which is constantly evolving in response to hackers and pirates iterating on their methods to go un-detected. As such, many of the breaches are detected after-the-fact which causes tremendous damage in terms of data leaks, critical system outages, firm reputation etc. It is imperative that Intrusion Detection Systems (IDS) or any security monitoring system be able to detect any possible attacks, including vectors never seen. Thus, the need for detecting anomalous activity in addition to known signatures is essential for a state of the art security monitoring system.

## Problem Statement
There has been many research in the past in anomaly detection using statistical methods(1), semi-supervised learning(2), neural networks(3), and RNNs(4) to some amount of success, but they do not fully address anomalous user behavioral patterns over time. While in a traditional sense, an anomaly detection can be used to detect spikes in network activities (and be able to detect DDOS attacks quite effectively), it is not effective in detecting a creative hacker who tries to mimic a “normal” user until they are able to breach through completely. Thus, a way of detecting and classifying “normal” and anomalous activity based on each user’s sequence of events (behaviors) is needed.

## Dataset
Obtaining a comprehensive dataset in cybersecurity can be difficult relative to other fields due to the field’s nature of security, many datasets are either internal, and unable to be shared because of privacy issues. Also, as threat vectors continue to evolve, it is often difficult to obtain an up-to-date dataset in the security realm for use of machine learning. Thankfully, the Communications Security Establishment (CSE and the Canadian Institute of Cybersecurity (CIC) has devised a systematic way to generating a diverse and comprehensive benchmark dataset for intrusion detection. Thus, for this project, I will use this dataset available at the Registry of Open Data on AWS – A Realistic Cyber Defense Dataset (CSE-CIC-IDS-2018)5 to train and evaluate the effectiveness of my solution. This dataset contains simulated network traffic log files and packet capture (pcap) from 30 servers and 470 machines amounting to over 500GB of data (approx. 94 million rows from the logs and approx. 450GB of packet capture data) which include annotated seven different attack vectors including DDoS, Heartbleed, Brute-Force and infiltration of the network from inside. 
From this dataset, it will be possible to train a model by establishing a baseline norm of user activities using benign traffic and evaluate the model with a mix of benign and malicious traffic. 

## Solution Statement
In order to detect anomalous activity in the network, each user (or IP) will be tracked based on their sequence of activities. To establish a baseline of a “normal” behavior, it would be necessary to collect and learn what a normal sequence of events look like for a typical user in addition to their access statistics (i.e.. Duration of session, count of requests, etc.). Here, we will borrow ideas from NLP and evaluate the effectiveness of RNN-LSTM as suggested by Tuor et all(6). I will be following closely their methods on anomaly detection but will also be exploring newer methods in sequence-to-sequence predictions such as the transformer model(7) to train the models. 

## Benchmark Model
This project will be exploring the methods as described by Tuor et all6 for unsupervised anomaly detection of user behaviors using LSTM but using a newer dataset CSE-CIC-IDS-2018(5) for training and evaluation. The results from this project will be evaluated and compared with several naïve solutions including Isolation Forest and one-class SVM as the benchmark for anomaly detection and I will explore other algorithms such as the mentioned transformer model(7). 
The models will be trained with benign (normal) activity and will be tested with a mix of benign activity and anomalous activity to evaluate the effectiveness of each model and will be compared to the benchmark model.

## Evaluation Metrics
As the proposed dataset for this project is labeled, it will be possible to use traditional evaluation metrics (accuracy, precision, recall) to quantify the performance of the benchmark model and the solution model(s). However, since the labeled dataset is skewed with mostly benign data points, I will be giving more emphasis on recall and precision values during evaluation.

## Project Design
This project will largely consist of three parts:
1.	Data exploration & feature extraction 
2.	Model training & experimentation 
3.	Evaluation & detection


Figure 1 Anomaly Detection Training and Testing Process

First, the dataset needs to be understood properly to determine what features can be extracted. Essentially, the feature extraction step must extract information such as basic statistics like aggregate counts, session time etc. in addition to the sequence of events or activity of each user. This will allow for the detection algorithm to utilize both user behavioral patterns as well as statistical trends.
Next, each user activity will be transformed to a feature vector which will be fed into a neural network so that the network can be trained to learn the “normal” behavior. The idea here is that once the model is trained to know what the typical normal behavior of a user is, any user which deviates in the accepted pattern will be flagged as an anomaly.
Because the goal of this project is to learn patterns of sequence of events, NLP applications seem to fit in quite nicely if we substitute the words with events. Thus, some of the most prominent algorithms are considered for developing a user behavior-based anomaly detection system – RNN-LSTM8, and transformer model(7).  
The algorithms will be trained with the pre-processed data which will be used to train a baseline “ground truth” of normal expected user behavior. If the user’s behavior deviates from the expected “norm”, it will be marked as an anomaly. Thus, it is possible to train a model without a label, effectively making this an unsupervised learning problem, but it will be able to determine if an activity is normal or not. A test set that contains known hacking activity will be fed into the trained model to evaluate the effectiveness of the models.





## References
1. Carter, K. M., and Streilein, W. W. 2012. Probabilistic reasoning for streaming anomaly detection. In Proc. SSP, 377–380. 
2. Gavai, G.; Sricharan, K.; Gunning, D.; Hanley, J.; Singhal, M.; and Rolleston, R. 2015. Supervised and unsupervised methods to detect insider threat from en- terprise social and online activity data. Journal of Wireless Mobile Networks, Ubiquitous Computing, and Dependable Applications 6(4):47–63. 
3. Ryan, J.; Lin, M.-J.; and Miikkulainen, R. 1998. Intrusion detection with neu- ral networks. Advances in neural information processing systems 943–949. 
4. Debar, H.; Becker, M.; and Siboni, D. 1992. A neural network component for an intrusion detection system. In Proc. IEEE Symposium on Research in Security and Privacy, 240–250. 
5. https://www.unb.ca/cic/datasets/ids-2018.html
6. Tuor, A.; Kaplan, S.; Hutchinson, B.; Nichols, N.; and Robin- son, S. 2017. Deep learning for unsupervised insider threat detection in structured cybersecurity data streams. In Artifi- cial Intelligence for Cybersecurity Workshop at AAAI. 
7. A.Vaswani,N.Shazeer,N.Parmar,J.Uszkoreit,L.Jones,A.N. Gomez, Ł. Kaiser, and I. Polosukhin, “Attention is all you need,” in Advances in Neural Information Processing Systems, 2017, pp. 6000–6010. 
8. Hochreiter, Sepp & Schmidhuber, Jürgen. (1997). Long Short-term Memory. Neural computation. 9. 1735-80. 10.1162/neco.1997.9.8.1735.
