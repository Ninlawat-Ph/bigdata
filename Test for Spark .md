# Spark Testing

#### Get dataset :

Goto path: /opt/spark

wget 'https://raw.githubusercontent.com/lakshay-arora/PySpark/master/spark_streaming/datasets/twitter_sentiments.csv'

#### Create file python sentiment_analysis_streaming.py

https://github.com/lakshay-arora/PySpark/blob/master/spark_streaming/twitter_sentiment_analysis.py

you use command: vi sentiment_analysis_streaming.py pass code python in this

#### Command to test

Open two session 

- First session

       run command:    nc –lk 4321

- Second session (Before you run command need to tunnels prot what you want test)

       run command:    python3 /opt/spark/sentiment_analysis_streaming.py localhost 4321
       
       
    https://drive.google.com/drive/u/1/folders/1TPapBvOKN0yezMfuYaBpHpnjwaS66LdY

