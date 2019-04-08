Deep Dive into the Microsoft Professional Program in Artificial Intelligence
=====================
Chris Cameron, Spiria

# Background

Recently I found some downtime at Spiria and decided to do some skills upgrading. I have long had an interest in Artificial Intelligence stemming from University courses in AI, Discrete Search and Optimization and Matrix Calculations, among others. I was happy to find that Microsoft had opened up an extensive internal training program to the general public through a partnership with [edX](https://www.edx.org), so upon finding it I began auditing the program.

The whole program is broken up into 10 courses, which are:

1. Introduction to Artificial Intelligence (AI)
2. Introduction to Python for Data Science
3. Essential Math for Machine Learning: Python Edition
4. Ethics and Law in Data and Analytics
5. Data Science Research Methods: Python Edition
6. Principles of Machine Learning: Python Edition
7. Deep Learning Explained
8. Reinforcement Learning Exlained
9. Develop Applied AI Solutions (three options below)
  - Computer Vision and Image Analysis
  - Speech Recognition Systems
  - Natural Language Processing (NLP)
10. Final Project

# Deep Dive Part 2: Introduction to Artificial Intelligence (AI) - Course Review

In this second entry I'm going to discuss the first course in the program, [Introduction to Artificial Intelligence (AI)](https://www.edx.org/course/introduction-artificial-intelligence-3). In the previous entry I explained how to setup your local environment with Docker and Anaconda3 (specifically the miniconda3 image) in order to run Jupyter notebooks. This course makes heavy use of Jupyter Notebooks in the labs and I was glad to have it running locally when Azure's hosted Jupyter offering became unresponsive.

This course is broken into six modules:

1. Introduction
2. Machine Learning
3. Language and Communication
4. Computer Vision
5. Conversation as a Platform
6. Learning More

Let's examine each of these in detail

# Module 1: Introduction

This module is a set of videos that you can mostly gloss over. The biggest thing you'll get out of this is a link to the [Course Setup Guide](https://aka.ms/edx-dat263x-setup) in the `Preparing for the Labs` section.

_Note: Be sure to check that the link above and other links I provide later have not become stale over time. Be sure to compare to your course's instructions; the materials do change from time-to-time._

Inside the setup guide you'll be asked to [sign up for a 30-day trial](https://aka.ms/edx-dat263x-az) to MS Azure using either your existing account or new Microsoft account. You'll be asked to provide a credit card number but the trial includes $200 of credits which will be plenty enough to complete the course.

You'll then be asked to [download the lab files](https://aka.ms/edx-dat263x-labfiles) which are essentially small data files and Jupyter notebooks (the files with the `.ipynb` extension). In each of the labs you'll be asked to upload these notebooks to the Jupyter server (either your local Jupyter or Azure Notebooks). There's a folder for demos and for labs. Each lab folder is named `Lab01`, `Lab02` etc.

# Module 2: Machine Learning

Like much of this course, definitions will come fast and furious with theoretical knowledge hand-waved away to further study. I was upset about this at first however the later courses go into more depth so just take some notes as you go and remind yourself that even though things seem fast now you'll fill in the gaps later.

## Introduction

The basic introduction here is that Machine Learning (ML) is about taking historical data (**features**) and using it to predict a value. Supervised ML starts with **features** that have known outcomes, or **labels**. Unsupervised ML does not have **labels** and so your ML model will be trained to find similarities between the **features** in order to make its predictions.

## Regression

Next you learn that predicting these values is essentially a Regression problem (e.g., Linear Regression), which you'll learn plenty about in the 3rd course _Essential Math for Machine Learning: Python Edition_. Try to think about your **features** being used as input to a _function_ `f(x_1, x_2, x_3,...) = y` such that each `x_k` is a relevant **feature** and `y` is the value you predict based on those **features**.

If you are doing Supervised ML you have the benefit of being able to compare your predictions with actual known **labels**, and a few techniques for measuring error are introduced including Mean Absolute Error, Relative Absolute Error, and Relative Squared Error.

After the brief intro to regression and error (a.k.a. residual) computations, two ML models are discussed: Classification (for supervised ML) and Clustering (for unsupervised ML).

## Classification

Classification is about creating a function whose output classifies the features. A binary classifier comes out with a `y` value that is either 0 or 1 or thresholded to be 0 or 1. You can then measure how good your classifier is by computing the number of true and false positives and true and false negatives.

Then comes the whirlwind of terms. Confusion Matrix. Accuracy. Precision. Recall. Receiver Operator Characteristic (ROC) chart. Again, if these sorts of measurements are new to you, try not to feel overwhelmed. The majority of this course is spent showing you how to get Azure Machine Learning services to do these things for you; deeper understanding will come in the later courses. Suffice it to say, if algebra and calculus make your knees shake you're going to need to have courage to get through!

## Clustering

As for Clustering, there is a brief introduction to k-means clustering which is basically a way of saying you want to separate your observations into k groups, or clusters. This is done in a spatial sense where you consider if you have `n` **features** then you could plot a point in `n` dimensional space. There are a few ways to do the clustering, but the one they go into detail with is Principal Component Analysis (PCA). With PCA you define ellipses which represent your clusters (k clusters => k ellipses). When you visualize the ellipses you want the major axis of one ellipse to be as orthogonal (perpendicular) to the other as possible.

The PCA section is a bit hand-waved over, and you need to study up on PCA to truly grip the subject. Having seen the PCA technique used to analyze decades of microseismic event (think mini earthquakes) data I can say it is an extremely useful mathematical technique and worth learning.

## Azure Machine Learning Studio

A.K.A. "This course brought to you by Microsoft"

The rest of the Machine Learning module is less about learning ML and more an instructional advertisement for Microsoft Azure and its suite of Machine Learning software. I don't say that to disparage Microsoft: in fact, there's quite an amazing assortment of ML SaaS available here and anyway, I'm auditing the course for free.

However, I was a bit disappointed that in a course called "Introduction to AI" there had not yet been a proper treatise on the subject of Artificial Intelligence. Chalk this up to the practical nature of the courses - everything has been whittled down to get you up-and-running experiments as fast as possible. In fact if you already have a solid foundation in ML, the rest of this course serves as a great introduction to Azure's portfolio of tools you will likely need if you're running your experiments in the cloud, and you can probably stop after this course (though be sure to check out the Law and Ethics course too!)

Anyhow. The first thing you'll do is open up the Azure Dashboard and provision a "Machine Learning Studio Workspace", and within that you will open the "Machine Learning Studio". This is a Studio in the same way Visual Studio is a Studio. This software is really cool, and feels like an executable flow chart. Basically you search for modules that have been pre-built to perform certain tasks like uploading data, cleaning it, normalizing it, etc and drag-and-drop them into a canvas. Then you connect each module to the next. Actually if you've ever used Audio Muxing software in a GUI it's quite similar looking.

Eventually the instructor shows you how to export data from Machine Learning Studio into a Jupyter Notebook which you can either Download or run in Azure Notebooks. Once there, he shows you some basic stuff with grids and scatterplots. Having never used Jupyter before I was really impressed with the way it feels like an interactive research document: it has a very publishable feel but still lets you tinker and experiment. Really cool stuff.

## Creating Some Models

Earlier the instructor taught basics of Regression, Classification, and Clustering. The next sections show you how to do this using Machine Learning Studio.




