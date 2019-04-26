# scmz
single cell model zoo bainstorm repo






Single Cell Model Zoo:

    - what we want: 
        - smth like scVI (https://github.com/YosefLab/scVI)
            => a trainable neural-network-ish model for single cell data

        - we want to train these models on large available datasets.

        - Once the models are trained, we want to apply these models to datasets which where unseen an training time. 

    - Questions / Issues:

        - when training such a model we use a particular modality with a certain number of features (=expressions). When we want to use this model for a new dataset we (usually?) have a different feature set (differnt expressions). To make the new dataset applicable to the pretrained model we need to project the model to the feature space we used for trainig.This means we take the itersection of the featues (expressions which are in both datsets) and set the missing features to zero.
        This might lead to poor performance.

        - Models like scVI are very scable (according to the paper
        1mio cells can be trained in ~1 hour).
        This means there is only little benefit in using a pretrained model compared to traning a model from the scratch.

        - Accuary: It is questionable that a pretrained model has benefits compared to training from the scratch.
        The hope is, that when we train on a very big dataset, we can get (biological) insights on the small dataset which would be invisible without the pretraining on the large dataset.
        Iff the feature set of the dataset which was used for training
        is identical with the featue set used at test time I think a boost in performance is reasonable. Iff the feature sets only have a small intersection I believe it is questionable if there are benefits compared to training from the scratch.

    - Ideas:

        - We could merge multiple datasets with potentially different features/expressions into a single dataset.
        All expressions which are missing in one dataset but are contained in an other dataset are set to zero in the dataset where they are missing
        (to put it differntly, we replace missing expressions/features with zeros)
        After merging all exressions should be non-zero for at least some cells.
        In this way we created a datset which has support for a lot of expressions.
        In the extrem we would combine all datassets we can find, and create a model which uses all 20,000 genes.
        The main challange will be to train a neural network which is able to handle 20k features.
