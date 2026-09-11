# Stock-FX-Tracking-and-Prediction
This repository presents the methodology and results of a stock and FX prediction project.<br>
*A case study in time-series modeling for financial markets*

## Overview

This project builds models to track and predict stock prices and exchange rates. The focus is on capturing trends and evaluating prediction accuracy on unseen data.

Part of implementation details and code are publicly shared; this site presents the methodology, results, key figures, and code .

## Data & Setup

- **Assets:** [e.g. stock 2317, USD/TWD exchange rate]
- **Period:** [e.g. daily trade transactions: 2019-2020 of 2317, 2010 - 2020 of USD/TWD Exchange Rate]
- **Split:** train/validation/prediction with chronological ordering

## Modeling Approach

The models use historical prices and derived features to forecast future values. The evaluation emphasizes:

- Ability to track major up/down moves in historical data
- Ability to data preprocessing, build a recurrent neural network (RNN), Train the model, exchange rate/stock forecasting, Plot charts


## Results: Tracking

![Stock price tracking: predicted vs actual over time](stock_tracking.png)

**Stock price tracking.** The model follows the main trends in historical stock prices.

![Exchange rate tracking: predicted vs actual over time](fx_tracking.png)

**Exchange rate tracking.** The model captures the broad movements in the FX rate.

## Results: Prediction vs. Actual

![Stock prediction vs actual on test set](stock_prediction_vs_actual.png)

**Stock prediction vs. actual (test set).** The scatter shows how closely predicted prices align with realized prices on unseen data.

![fx prediction vs actual on test set](fx_prediction_vs_actual.png)

**FX prediction vs. actual (test set).** The model achieves reasonable alignment with actual exchange rates, with some under/over-prediction at extremes.



## Coding  
```
# 1. Reshape data for RNN:
reshaped_data = np.array(data).astype('float64')
x= reshaped_data[:,:-1]
y = reshaped_data[:,-1]

# 2. Split into train and test set attention : use chronological split to prevent data leakage form  future to past 
split = int(len(x) * 0.8)
x_train, x_test = x[:split], x[split:]
y_train, y_test = y[:split], y[split:]

# 3. Build RNN model and compile with Adam optimizer and MSE lose for regression task  
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import,SimpleRNN, Dropout,Dense

model = Sequential()
model.add(SimpleRNN(input_shape=(),units=256, unroll=False))
model.add(Dropout(0.2))
model.add(Dense(units=1))
model.compile(optimizer='adam', loss='mse')

# 4. Train the model
fit on training data with test set used for validation monitoring
model.fit(x_train, y_train, epochs=50, batch_size=32, 
          validation_data=(x_test, y_test), verbose=1)

# 5. Predict and inverse-transform to original scale 
pred_scaled = model.predict(x_test)
pred = scaler.inverse_transform(pred_scaled)     
actual = scaler.inverse_transform(y_test.reshape(-1,1))

# 6. Visualize actual vs predicted exchange rates 
plt.figure(figsize=(12,5))
plt.plot(actual, label='actual exchange rate')
plt.plot(pred,label='prediction exchange rate')
plt.legend()
plt.title('USD/TWD')
plt.show()
```

This is a demonstration site for a portfolio project.
