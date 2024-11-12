---
title: "Building Chess ID"
author: "Daylen Yang"
date: 2016-01-17T22:48:07.399Z
lastmod: 2024-11-11T21:46:59-08:00

description: ""

subtitle: "Featuring: Computer Vision! Deep Learning!"

image: "/posts/2016-01-17_building-chess-id/images/1.png" 
images:
 - "/posts/2016-01-17_building-chess-id/images/1.png"
 - "/posts/2016-01-17_building-chess-id/images/2.jpeg"
 - "/posts/2016-01-17_building-chess-id/images/3.jpeg"
 - "/posts/2016-01-17_building-chess-id/images/4.png"
 - "/posts/2016-01-17_building-chess-id/images/5.gif"
 - "/posts/2016-01-17_building-chess-id/images/6.gif"


aliases:
    - "/building-chess-id-99afa57326cd"

---

#### Featuring: Computer Vision! Deep Learning!

In April 2014, my friends and I attended [Big Hack](https://www.facebook.com/events/565394820235478/), a Cal vs. Stanford hackathon. We were wide-eyed freshmen back then, and we threw around the term “machine learning” as if we were Yann LeCun himself. Naturally, we decided to build something that was completely out of our league: an app that would recognize individual chess pieces in a picture of a chessboard, and then tell you who was winning and what the best move was.

The latter was easy: we would just feed the chess position into [Stockfish](http://stockfishchess.org/), an open source chess engine. The image recognition component was much harder to do than we had anticipated, so we slightly pivoted: now, you would take a picture after every turn, and the app would figure out which squares had changed, and so it would still be able to follow the game. But even this proved too difficult for us, and after 24 hours of hacking, we didn’t have much to demo.
![image](/posts/2016-01-17_building-chess-id/images/1.png#layoutTextWidth)
At least our hackathon project had a really stylish landing page.

Fast forward to December 2015: having just finished taking a machine learning class, I wanted to take another stab at building a chess piece visual recognition system.

The first step to identifying chess pieces from a picture of a board is to detect the board and segment it into 64 little squares. I stumbled on [this blog post](http://codebazaar.blogspot.com/2011/08/chess-board-recognition-project-part-1.html) which recommends using a Canny edge detector and then a Hough line detector, and that’s basically what I did. The Hough line detector will pick up the 9 horizontal and 9 vertical lines that make up an 8x8 chessboard. After Hough, you can calculate all the intersection points, run agglomerative clustering, pick out the corner points, do a perspective shift, and then divide by 8 horizontally and vertically to get your squares.
![image](/posts/2016-01-17_building-chess-id/images/2.jpeg#layoutTextWidth)
The next step is to identify the chess piece on each of the 64 squares. Before I get into that, it’s useful to take a step back and look at the state-of-the-art in image recognition. How good are computers at recognizing everyday images? There’s a competition called the ImageNet Large Scale Visual Recognition Competition (or ILSVRC for short) that quantifies this. Google’s GoogLeNet won ILSVRC in 2014 with a Top-5 error rate of 6.7%, which means that 93.3% of the time, the actual label for the image was among the top 5 labels that the neural network returned. Karpathy built a web app that lets you [try out labeling ILSVRC images](http://cs.stanford.edu/people/karpathy/ilsvrc/). Can you beat GoogLeNet?

So, deep convolutional neural networks like GoogLeNet are pretty good at image recognition. What we can do is take a neural network that’s been trained on the ImageNet dataset and fine-tune it with our own data and labels. This process is called [transfer learning](http://cs231n.github.io/transfer-learning/). For this to work, though, I would need a _ton_ of data: images of chess pieces along with their labels. There was no way around it: I took a bunch of pictures and ended up with 10,000+ images of chess pieces, which I hand-labeled. Since chess pieces are rotation invariant, I augmented the data with rotations.
![image](/posts/2016-01-17_building-chess-id/images/3.jpeg#layoutTextWidth)
A peek at some of the training data

With data in hand, it was time to train. Using the [Caffe](http://caffe.berkeleyvision.org/) deep learning framework and starting with a pre-trained AlexNet (winner, ILSVRC 2012), the fine-tuned neural network was achieving 99% accuracy on the test set in no time. Sweet!

In a research setting, this is where things would end. But I wanted to productionize Chess ID, so I needed to deploy the image recognition model to a server. It turns out that evaluating AlexNet on 64 images is somewhat computationally intensive, and I’d been spoiled by the speed of the MacBook Pro.

![image](/posts/2016-01-17_building-chess-id/images/4.png#layoutTextWidth)


As you can see from the chart, the performance offered by cloud hosts is pretty sad. Rather than pay $480/year for Linode’s 4 GB instance, I threw together a cheap server for around $350. How does it do? It can process a chessboard in 5.75 seconds, which is a hair faster than the Linode 4 GB instance. Not bad for a [Pentium processor](http://ark.intel.com/products/88179/Intel-Pentium-Processor-G4400-3M-Cache-3_30-GHz).

Now let’s see Chess ID in action:

![image](/posts/2016-01-17_building-chess-id/images/5.gif#layoutTextWidth)
(some sequences shortened)


![image](/posts/2016-01-17_building-chess-id/images/6.gif#layoutTextWidth)
_(some sequences shortened)_



I’ve made the data and the final model [available on GitHub](https://github.com/daylen/chess-id), so you can train your own models or deploy your own server.
