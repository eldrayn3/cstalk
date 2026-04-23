import pandas as pd
import numpy as np
import statsmodels.api as sm
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
from statsmodels.graphics.regressionplots import plot_partregress_grid

df = pd.read_csv('Fuel_Cost.csv')
df.head()
#მონაცემების გამოსახვა ბიბლიოეთეკით

df.isnull().sum() #არასათანადო მონაცემების შემოწმება
#ნაპოვნი არა გამოტოვებული მნიშვნელობები

#ცხრილიდან ვიღებთ co2emisons
X = df.drop(columns=["CO2EMISSIONS"])
y = df["CO2EMISSIONS"]
#ვაგებთ მოდელს
X_const = sm.add_constant(X)
model_full = sm.OLS(y, X_const).fit()
model_full.summary()

#Partial Regression Plots ვაგებთ
fig = plt.figure(figsize=(12, 8))
plot_partregress_grid(model_full, fig=fig)
plt.tight_layout()
plt.show()

#ვარჩევთ მნიშვნელოვან ცვლადებს ამეებს ძლიერი გავლენა აქვთ
selected_features = ["ENGINESIZE", "CYLINDERS", "FUELCONSUMPTION_COMB_MPG"]
X_selected = df[selected_features]

#train test გაყოფა
X_train, X_test, y_train, y_test = train_test_split(
    X_selected, y, test_size=0.15, random_state=42
)

#საბოლოო მოდელის აგება
X_train_const = sm.add_constant(X_train)
X_test_const = sm.add_constant(X_test)
model_final = sm.OLS(y_train, X_train_const).fit()
model_final.summary

#პროგნოზი
y_pred = model_final.predict(X_test_const)

#შეფასება
rmse = np.sqrt(mean_squared_error(y_test, y_pred))
r2 = r2_score(y_test, y_pred)
print("\nModel Evaluation:")
print(f"RMSE: {rmse:.2f}")
print(f"R^2: {r2:.4f}")
#21.58 prediction error გვიჩვენებს ამ მოდელის სიზუსტეზე ეს მოდელი კარგია და ხსნის კანონზომიერებას

#Actual vs Predicted model comparison
plt.figure(figsize=(8, 6))
plt.scatter(y_test, y_pred, alpha=0.6)
min_val = min(y_test.min(), y_pred.min())
max_val = max(y_test.max(), y_pred.max())
plt.plot([min_val, max_val], [min_val, max_val], linestyle='--')
plt.xlabel("Actual CO2")
plt.ylabel("Predicted CO2")
plt.title("Actual vs Predicted CO2")
plt.grid()
plt.show()

import seaborn as sns
import matplotlib.pyplot as plt
corr_matrix = df.corr()
plt.figure(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, fmt=".2f", linewidths=0.5)
plt.title("Correlation Heatmap")
plt.show()

--------------------------------------------------

import pandas as pd
ship =pd.read_csv("https://raw.githubusercontent.com/datasciencedojo/datasets/refs/heads/master/titanic.csv")
ship['Age'] =ship['Age'].fillna(ship['Age'].mean())
ship['Sex']  =ship['Sex'].map({ "male":0,"female":1})
ship['family']  =ship['Parch'] +ship['SibSp']
ship.drop(['PassengerId','Name','Ticket','Cabin','Embarked','Parch','SibSp'],axis=1,inplace=True)

ship.head()

y =ship['Survived'].values
X =ship.drop('Survived',axis=1).values
from sklearn.model_selection import train_test_split
X_train,X_test,y_train,y_test =train_test_split(X,y,test_size=0.2,random_state=1)
from sklearn.tree import DecisionTreeClassifier
model =DecisionTreeClassifier()
model.fit(X_train,y_train)

positive_probabilities =model.predict_proba(X_test)[:,1]
from sklearn.metrics import roc_curve

fpr,tpr,thresholds =roc_curve(y_test,positive_probabilities)
import matplotlib.pyplot as plt
plt.plot(fpr,tpr)
