# steganography detection with machine learning

*scalable functions for detecting malicious Least Significant Bit (LSB) insertion steganography with machine learning*

code available [here](https://github.com/disesdi/steganography_detection_with_machine_learning/blob/fe2f5a4f45838245913199a797176a71caf2513a/steganography_detection_with_machine_learning.ipynb)

## background

*steganography* refers to encoding information in a non-secret image or file, typically via statistical means. steganography is frequently used to obscure the transmission of malicious code, where the *hidden text* is often encrypted before being encoded in the *cover text*. 

> *watermarking* is an example of a non-malicious use of steganography. 

when used to obscure the presence of data, steganography becomes a means of *security through obscurity*, which, when combined with cryptographic methods, can make the encoded data highly difficult to access/analyze by outside parties. this renders it a particularly effective tool for malicious purposes.

steganography is also increasingly accessible to would-be users; open source tools such as [OpenStego](https://www.openstego.com/), or others like [Image Steganography](https://incoherency.co.uk/image-steganography/) democratize steganography applications considerably--which in turn leads to greater ease of use by malicious actors.

the increased threat surface exposed by proliferation of steganographic tools & techniques makes detection a security imperative. because of its often statistical nature, steganography detection is a natural area of application for machine learning.

## least significant bits (LSB)

Least Significant Bit insertion involves replacing the least significant bit of image pixels with the concealed data bits. bits can be replaced naively, or random noise can be introduced in order to more effectively evade detection.

## tools & data

this project makes use of `Aletheia`, an open source tool for steganography analysis.

a variety of tools for data extraction & analysis are available in the `Aletheia` github repo: https://github.com/emmanueltsukerman/aletheia

although `Aletheia` includes pre-built models, for this project i've chosen to build and train my own.

two datasets are used in training, representing the two approached to LSB insertion described above. they are `bossbase.fea` and `bossbase_lsb.fea`.

finally, due to the size of the datasets & processing contraints model training is performed in [Google Colab](https://colab.research.google.com/).

## methods & models

data is split into 70% training and 30% testing data using the `train_test_split` module from `sklearn`.

i chose to test two models, implemented with `sklearn`: 

* Random Forests: `sklearn.ensemble.RandomForestClassifier`

* Gaussian Naive Bayes: `sklearn.naive_bayes.GaussianNB`

since steganography detection might be performed at scale, i defined a suite of functions to train, score, and report on classifier performance: 

* `fit_clf` takes a list of instantiated classifiers, & fits to the training data

* `score_clf` takes a list of fitted classifiers, retrieves & prints their scores

* `fit_score_clfs` takes a list of instantiated classifiers, & calls `fit_clf` and `score_clf`, respectively.
