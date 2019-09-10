This Jupyter notebook uses a discrete mixing model to perform unsupervised clustering of profiles from an online freelance labor market called UpWork. Workers provide a list of their skills, and the learned clusters correspond to particular "types" of workers -- at a broad level, coders, web developers, designers, writers, and office workers. The dataset is described in more details in Anderson, Katharine A., (2018). “Skill Networks and Measures of Complex Human Capital” Proceedings of the National Academy of Science, 201706597.

This small project arose out of conversations with Dr. Anderson. She and a collaborator wanted to include skill data in some reduced form in a model of the labor market. They used $k$-means for clustering, and had trouble with a "giant" cluster that included about half of the workers, and did not have a clean qualitative description. I figured a mixing model could do better.

The discrete mixing model is similar to a Gaussian mixing model, in that the underlying model for workers first assigns them a type, which we cannot directly observe, then assigns observable features based on that type. In the Gaussian model each type corresponds to a multivariate normal distribution. In this discrete model, each type corresponds to a vector of probabilities for having each skill. The skill assignments are then treated as independent Bernoulli trials (conditional on type).

I couldn't find a nice Python library for fitting this sort of model, so I implemented the expectation-maximization algorithm myself. The model takes in a number of clusters, then returns a vector of skill probabilities for each cluster. In the following we print the most probable skills, and see that they make sense qualitatively. In fitting the model we also generate an estimate of the type assignment for each worker, which can have support in multiple types. We look at those below and find that the discrete mixing model successfully classifies most workers, with only a small fraction ambiguously classified. As one might expect, the greatest degeneracy is between the coders and web-developers, and between writers and office workers.

The primary run below has five clusters, since this provided a good comparison with previous work that used $k$-means. A run with $k=10$ shows that the clusters are still qualitatively sound for higher $k$. We have not done the cross-validation required to determine on optimal $k$, since our goal was to produce a manageable number of clusters, rather than the finest possible partitioning of the dataset.
