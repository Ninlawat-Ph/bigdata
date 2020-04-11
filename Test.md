# Spark testing

#### Get dataset :

Goto path: /opt/spark

wget 'https://raw.githubusercontent.com/lakshay-arora/PySpark/master/spark_streaming/datasets/twitter_sentiments.csv'

#### Create file python sentiment_analysis_streaming.py

https://github.com/lakshay-arora/PySpark/blob/master/spark_streaming/twitter_sentiment_analysis.py

you use command: vi sentiment_analysis_streaming.py pass code python in this

#### Command to test

Open two session 

- first session

nc –lk 4321


python3 /opt/spark/sentiment_analysis_streaming.py localhost 4321

