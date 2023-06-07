# Predicting HOMO-LUMO Gap for Organic Solar Cells

This project involves the discovery of promising materials for organic solar cells. The focus is on the HOMO-LUMO gap, a measure of molecular efficiency for solar energy use. The larger this gap, the more efficient the molecule is for solar cells. The challenge is to use machine learning to predict this gap from a molecule description, which is a more efficient approach than traditional density functional theory. Follow along with the notebook that contains in-depth descriptions of the code and the choices made.

## High-Level Description

The data provided includes a small data set (100 molecules with associated HOMO-LUMO gaps) and a larger, related data set (50,000 molecules with LUMO energy levels). Despite the second data set's labels not being directly the desired prediction target, it is considered valuable due to the probable correlation between molecule features predictive of LUMO energy and the HOMO-LUMO gap.

This approach is referred to as transfer learning, where features learned on one task are used for another related task. The project also suggests using unsupervised representation learning methods, like autoencoders, to extract useful feature representations from the larger dataset.

## Technical Description

The code employs a hybrid approach combining an autoencoder with regression models for a predictive task. This approach uses semi-supervised learning, beneficial given the extensive data requirements of deep learning models. Instead of using pre-provided fingerprints, the RDKit library is used to extract MACCS keys and ECFP (2000 bits) from the smiles of each molecule, thereby expanding the dataset.

The autoencoder in this code doesn't compress the input but seeks to learn valuable feature transformation. This feature transformation is then used in conjunction with a predictor model trained on the encoder's output to predict the target variable, LUMO. This dual-task structure encourages the encoder to learn features beneficial for both reconstructing the original data and predicting the target variable.

After training the autoencoder-predictor on the larger pretraining dataset, the encoder, now acting as a feature extractor, is used to transform the actual training data into a latent representation. This representation captures significant patterns relevant to a similar task. Then, two regression models are trained on these encoded features, exploiting the learned patterns for the prediction task. The one with the lowest rmse on the validation data is then chosen to provide the final predictions on the test dataset.
