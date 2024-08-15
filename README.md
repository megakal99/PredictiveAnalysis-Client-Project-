# PredictiveAnalysis-Client-Project- MevStrategy - Data-Driven Decision-Making

PredictiveAnalyse is designed to predict gas fees and block numbers for transactions in the Ethereum blockchain, specifically targeting the MEV (Miner Extractable Value) sandwich attack strategy. This project utilizes advanced deep learning models to forecast gas fees and optimize transaction placement, aiming to gain strategic advantages by predicting and leveraging transaction ordering and gas fees for successful MEV sandwich attacks.

### Overview:

To predict Front-running and Back-running transactions' gas fees, we used the following deep learning models:

    * LSTM (Long Short-Term Memory) for forecasting GasBaseFee for upcoming blocks.
    * MLP (Multi-Layer Perceptron) for predicting the maximum priority gas fees.

#### LSTM Model:

For training the LSTM model, we utilized a dataset with over 40,000 observations. Each observation contained block data such as BlockTimeStamp, GasBaseFees, GasUsed, and BlockSize. A lag of 1 timestamp was applied for optimal model performance, with Mean Squared Error (MSE) as the loss function.

Results:
Data	MSE (Gwei)	MAE (Gwei)	MAX Error (Gwei)	75% Percentile Error (Gwei)	95% Percentile Error (Gwei)
Train	0.0000575	0.00506	0.079949	0.002332	0.006
Test	0.0000571	0.00503	0.110665	0.002435	0.0061

Conclusion: 95% of predicted BaseGasFee values differ from the actual values by less than 0.0061 Gwei. In the worst-case scenario, the error is 0.11 Gwei. Accuracy decreases slightly for blocks beyond the next one, with errors for the second to fifth blocks ranging from 1.60 Gwei to 2.42 Gwei, with rare maximum errors up to 16.93 Gwei.

#### MLP Model:

The MLP model predicts maximum priority gas fees for Front-running and Back-running transactions using approximately one million observations for training and over 321,000 for testing.

Results:

Front-Running Transactions:
Data	MSE (Gwei)	MAE (Gwei)	MAX Error (Gwei)	95% Percentile Error (Gwei)	99% Percentile Error (Gwei)
Train	77.47	1.45	300.29	0.96	14.96
Test	74.76	1.44	300.58	0.96	14.71

Conclusion: 99% of prediction errors are less than 14.71 Gwei, with 95% of errors being less than 0.96 Gwei. Outliers occasionally exceed 200 Gwei, occurring between 1% and 2% of the time.

Back-Running Transactions:
Data	MSE (Gwei)	MAE (Gwei)	MAX Error (Gwei)	95% Percentile Error (Gwei)	99% Percentile Error (Gwei)
Train	63.64	2.78	297.42	-0.71	12.44
Test	62.78	2.77	297.24	-0.73	12.30

Conclusion: 99% of prediction errors are less than 12.44 Gwei, with 95% of errors being less than -0.73 Gwei. Rare errors occur between 1% and 2% of the time.

#### Block Number Prediction:

My approach to predicting the block number in which a specific transaction in the mempool will be registered is straightforward. I estimate the confirmation time of the targeted transaction and leverage the fact that, in the majority of cases, Ethereum blocks are created approximately every 12 seconds. By dividing the confirmation time of the transaction by 12 and adding the result to the number of the most recent block, I obtain the final prediction of the estimated block number where the targeted transaction will be registered.

This approach is simple and reliable because it relies on the Etherscan API (the most famous and reliable Block Explorer and Analytics Platform for Ethereum), to estimate the confirmation time of the targeted transaction. Additionally, it is based on the common understanding that Ethereum blocks are created every 12 seconds. Moreover, this approach is robust and less complex compared to using sophisticated modeling techniques that require extensive data collection, which can be time-consuming.

### Note:

The project is private. In the repository, you can find screenshots displaying the predictive analysis using the API created by FastAPI (Deployed in VPS).
