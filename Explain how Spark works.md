## วิธีใช้โมเดลการเรียนรู้ของเครื่องเพื่อทำนายการสตรีมข้อมูลโดยใช้ PySpark
โดยอ้างอิงมาจาก https://www.analyticsvidhya.com/blog/2019/12/streaming-data-pyspark-machine-learning-model/

#### Overview 
-	Streaming data เป็นหลักการที่น่าตื่นเต้นของ machine learning
-	เรียนรู้วิธีการใช้ machine learning (logistic regression) เพื่อทำนาย Streaming data โดยใช้ PySpark
-	ครอบคลุมพื้นฐานของ Streaming data และ Spark Streaming

#### Introduction
- ทุกวินาทีมีการส่ง Tweets มากกว่า 8,500 Tweets รูปภาพ มากกว่า 900 รูปภาพถูกอัพโหลดบน Instagram มีการโทร Skype มากกว่า 4,200 ครั้ง มีการโทรค้นหาโดย google มากกว่า 78,000 ครั้งและมีการส่ง email มากกว่า 2 ล้านฉบับ
- เราจะรวบรวมข้อมูลในระดับนี้ได้อย่างไร เราจะรู้ได้อย่างไรว่า machine learning pipeline  ยังคงประมวลผลลลัพธ์ทันที่มีการสร้างและรวบรวมข้อมูล สิ่งเหล่านี้เป็นงานสำคัญในอุตสาหกกรมที่ท้าทายที่ต้องเผชิญ และ เหตุใดแนวคิด Streaming data จึงได้รับความสนใจมากขึ้นระหว่างองค์กร
- ดังนั้นในบทความนี้เราจะได้เรียนรู้ว่าข้อมูลแบบสตรีมคืออะไรเข้าใจพื้นฐานของการสตรีมแบบ Spark แล้วทำงานกับชุดข้อมูลที่เกี่ยวข้องกับอุตสาหกรรมเพื่อนำข้อมูลแบบสตรีมมิ่งไปใช้โดยใช้ Spark

#### What is Streaming Data?
- Streaming data ไม่มีจุดเริ่มต้นหรือจุดสิ้นสุดแบบต่อเนื่อง ข้อมูลนี้สร้างขึ้นทุกๆ วินาที จากข้อมูลหลายแหล่งและจำเป็นต้องประมวลผลและวิเคราะห์โดยเร็วที่สุด ข้อมูลสตรีมจำนวนมากต้องได้รับการประมวลผลแบบ real-time เช่นการค้นหาของ Google 
#### Fundamentals of Spark Streaming
- Spark Streaming เป็นส่วนเสริมของ Core Spark API ที่ช่วยให้สามารถประมวลผลสตรีมข้อมูลล่าสุดที่ปรับขนาดได้และไม่ผิดพลาดมาทำความเข้าใจกับองค์ประกอบต่าง ๆ ของ Spark Streaming
1. Discretized Streams
 ขั้นตอนแรกของการสร้างแอพพลิเคชั่นสตรีมมิ่งคือการกำหนดระยะเวลาแบทช์สำหรับ  แหล่งข้อมูลที่เรารวบรวมข้อมูล หากระยะเวลาแบทช์คือ 2 วินาทีข้อมูลจะถูกเก็บรวบรวมทุก  2 วินาทีและเก็บไว้ใน RDD และสายโซ่ของซีรีย์ต่อเนื่องของ RDD เหล่านี้คือ DStream  ซึ่งไม่เปลี่ยนรูปและสามารถใช้เป็นชุดข้อมูลแบบ distribution โดย Spark ในระหว่างขั้นตอนการประมวลผลข้อมูลล่วงหน้าเรา จำเป็นต้อง  transform variables, including converting categorical ones into numeric, creating        bins ลบค่าผิดปกติ Spark เก็บข้อมูลประวัติการ transformations ที่เรากำหนดไว้ในข้อมูลใดๆ ดังนั้นเมื่อใดก็ตามที่มีความผิดพลาดเกิดขึ้นมันสามารถย้อนเส้นทางของ         transformations และการสร้างผลลัพธ์ที่คำนวณใหม่อีกครั้ง


![Push up to github](https://cdn.analyticsvidhya.com/wp-content/uploads/2019/12/Screenshot-from-2019-12-04-16-27-27.png)

2. Caching นี่คือวิธีหนึ่งในการจัดการกับความท้าทายนี้ เราสามารถเก็บผลลัพธ์ที่เราคำนวณ (แคช) ชั่วคราวเพื่อรักษาผลลัพธ์ของการแปลงที่กำหนดไว้ในข้อมูลด้วยวิธีนี้เราไม่ต้องคำนวณการแปลงเหล่านั้นซ้ำแล้วซ้ำอีกเมื่อเกิดความผิดพลาดใด ๆ DStreams ช่วยให้เราสามารถเก็บข้อมูลสตรีมในหน่วยความจำ สิ่งนี้มีประโยชน์เมื่อเราต้องการคำนวณการดำเนินการหลายอย่างในข้อมูลเดียวกัน

3. Checkpointing การแคชมีประโยชน์อย่างยิ่งเมื่อเราใช้อย่างถูกต้อง แต่ต้องการหน่วยความจำจำนวนมาก และไม่ใช่ทุกคนที่มีเครื่องหลายร้อยเครื่องที่มี RAM ขนาด 128 GB เพื่อแคชทุกอย่าง Checkpointing คืออีกเทคนิคหนึ่งในการเก็บผลลัพธ์ของข้อมูลเฟรมที่ถูก transformation แล้วมันช่วยประหยัดสถานะของแอพพลิเคชั่นที่รันเป็นระยะ ๆ บนที่เก็บ reliable ข้อมูลคล้าย HDFS อย่างไรก็ตามมันช้าลงและมีความยืดหยุ่นน้อยกว่าแคช   เราสามารถใช้ Checkpointing เมื่อเรามีการสตรีมข้อมูล ผลลัพธ์การเปลี่ยนแปลงขึ้นอยู่กับผลลัพธ์การแปลงก่อนหน้าและจำเป็นต้องเก็บรักษาไว้เพื่อใช้งาน นอกจากนี้เรายังตรวจสอบข้อมูลเมตาดาต้าเช่นเดียวกับการกำหนดค่าที่ใช้ในการสร้างข้อมูลสตรีมมิ่งและผลลัพธ์ของชุดการดำเนินการ DStream

4. Shared Variables in Streaming Data มีบางครั้งที่เราจำเป็นต้องกำหนดฟังก์ชั่นที่จะต้องดำเนินการในหลาย ๆ cluster ตัวแปรที่ใช้ในฟังก์ชั่นนี้จะถูกคัดลอกไปยัง แต่ละเครื่อง(cluster) ที่นี่แต่ละ cluster มีตัวดำเนินการแตกต่างกันและเราต้องการบางสิ่งที่สามารถให้เชื่อมความสัมพันธ์ระหว่างตัวแปรเหล่านี้ตัวอย่างเช่นสมมติว่าแอปพลิเคชัน Spark ของเราทำงานใน 100 cluster ที่แตกต่างกันซึ่งจับภาพ Instagram ที่โพสต์โดยผู้คนจากประเทศต่างๆ เราต้องนับแท็กเฉพาะที่กล่าวถึงใน โพสต์ ตอนนี้ executors ของแต่ละคลัสเตอร์จะคำนวณผลลัพธ์ของข้อมูลที่มีอยู่ในคลัสเตอร์นั้น ๆ แต่เราต้องการบางสิ่งที่ช่วยให้กลุ่มเหล่านี้สื่อสารกันเพื่อที่เราจะได้รับผลสรุปรวม ใน Spark เรามีตัวแปรร่วมซึ่งทำให้เราสามารถแก้ไขปัญหานี้ได้

5. Accumulator Variable ตัวจัดการบนแต่ละคลัสเตอร์จะส่งข้อมูลกลับไปยังกระบวนการของไดรเวอร์เพื่ออัพเดทค่าของตัวแปรตัวสะสม โดยจะมีตัวแปรที่ใช้สำหรับรวบรวมข้อมูล executors เรียกตัวแปรนี้ว่า Accumulator
  
6. Broadcast Variable อนุญาตให้โปรแกรมเมอร์เก็บตัวแปรแคชแบบอ่านอย่างเดียวในแต่ละเครื่อง โดยปกติ Spark จะกระจาย Broadcast Variable โดยอัตโนมัติโดยใช้อัลกอริธึมการ Broadcast ที่มีประสิทธิภาพ แต่เรายังสามารถกำหนดได้หากเรามีงานที่ต้องใช้ข้อมูลเดียวกันหลายขั้นตอน
  
#### Understanding the Problem Statement
 - งานคือการจัดหมวดหมู่ Tweets แบ่งแยกเชื้อชาติหรือกีดกันทางเพศจากทวีตอื่น ๆ เราจะใช้ข้อมูลตัวอย่างการฝึกอบรมของ Tweets Label '1' แสดงว่าทวีตนั้นเป็นชนชั้น    เหยียด   ผิว / เพศหญิงและ Lable '0'

#### Setting up the Project Workflow
1. Model Building:  เราจะสร้าง Logistic Regression Model pipeline เพื่อจำแนกว่าทวีตมีคำพูดแสดงความเกลียดชังหรือไม่ ที่นี่เรามุ่งเน้นที่จะไม่สร้างรูปแบบการจัดประเภทที่แม่นยำมาก แต่เพื่อดูวิธีการใช้แบบจำลองใด ๆ และส่งคืนผลข้อมูลการสตรีม

2. Initialize Spark Streaming Context: เมื่อสร้างแบบจำลองแล้วเราจำเป็นต้องกำหนดชื่อโฮสต์และหมายเลขพอร์ตจากตำแหน่งที่เรารับข้อมูลสตรีม

3. Stream Data: ต่อไปเราจะเพิ่มทวีตจากเซิร์ฟเวอร์ netcat จากพอร์ตที่กำหนดและ Spark Streaming API จะได้รับข้อมูลหลังจากระยะเวลาที่กำหนด

4. Predict and Return Results: เมื่อเราได้รับข้อความ Tweets เราจะส่งข้อมูลไปยังขั้นตอนการเรียนรู้ของเครื่องที่เราสร้างขึ้นและส่งคืนความรู้สึกที่คาดการณ์จากแบบจำลอง

![Push up to github](https://cdn.analyticsvidhya.com/wp-content/uploads/2019/12/overview.png)

#### Defining the Stages of our Machine Learning Pipeline
- ตอนนี้เรามีข้อมูลใน Spark dataframe แล้วเราต้องกำหนด stage ต่าง ๆ ที่เราต้องการแปลงข้อมูลจากนั้นใช้มันเพื่อรับ label ที่ถูกทำนายจากโมเดลของเรา
ใน stage แรกเราจะใช้ RegexTokenizer เพื่อแปลงข้อความ Tweets เป็นรายการของคำ จากนั้นเราจะลบคำหยุดออกจาก word list และ word vectors ในขั้นตอนสุดท้ายเราจะใช้คำว่าเวกเตอร์เหล่านี้เพื่อสร้างแบบจำลองการถดถอยโลจิสติกส์(logistic regression model)และรับความรู้สึกที่ทำนายไว้

![Push up to github](https://cdn.analyticsvidhya.com/wp-content/uploads/2019/12/pipeline_streaming.png)

       # define stage 1: tokenize the tweet text    
       stage_1 = RegexTokenizer(inputCol= 'tweet' , outputCol= 'tokens', pattern= '\\W')
       # define stage 2: remove the stop words
       stage_2 = StopWordsRemover(inputCol= 'tokens', outputCol= 'filtered_words')
       # define stage 3: create a word vector of the size 100
       stage_3 = Word2Vec(inputCol= 'filtered_words', outputCol= 'vector', vectorSize= 100)
       # define stage 4: Logistic Regression Model
       model = LogisticRegression(featuresCol= 'vector', labelCol= 'label')

#### Setup our Machine Learning Pipeline
- เพิ่มขั้นตอนในวัตถุ Pipeline  แล้วเราจะทำการแปลงตามลำดับ ติดตั้ง Pipeline  กับชุดข้อมูลการฝึกอบรมและตอนนี้เมื่อใดก็ตามที่เรามี Tweets ใหม่เราเพียงแค่ต้องส่งผ่านวัตถุ Tweets และแปลงข้อมูลเพื่อรับการคาดการณ์
     
       #setup the pipeline
       pipeline = Pipeline(stages= [stage_1, stage_2, stage_3, model])

       #fit the pipeline model with the training data
       pipelineFit = pipeline.fit(my_data)
      
#### Stream Data and Return Results

- สมมติว่าเราได้รับความคิดเห็นนับร้อยต่อวินาทีและเราปิดกั้นผู้ใช้ที่โพสต์ความคิดเห็นที่มีคำพูดแสดงความเกลียดชัง ดังนั้นเมื่อใดก็ตามที่เราได้รับข้อความใหม่เราจะส่งสิ่งนั้นเข้าไปใน Pipeline  และรับความรู้สึกที่คาดการณ์ไว้ เราจะกำหนดฟังก์ชั่น get_prediction ซึ่งจะลบประโยคว่างและสร้างดาต้าเฟรมที่แต่ละแถวมีทวีต ดังนั้นเริ่มต้นบริบท Spark Streaming และกำหนดระยะเวลาแบทช์ 3 วินาที ซึ่งหมายความว่าเราจะทำการคาดการณ์ข้อมูลที่เราได้รับทุก 3 วินาที

# define a function to compute sentiments of the received tweets
       def get_prediction(tweet_text):
	       try:
             # filter the tweets whose length is greater than 0
		         tweet_text = tweet_text.filter(lambda x: len(x) > 0)
             # create a dataframe with column name 'tweet' and each row will contain the tweet
		         rowRdd = tweet_text.map(lambda w: Row(tweet=w))
             # create a spark dataframe
		         wordsDataFrame = spark.createDataFrame(rowRdd)
             # transform the data using the pipeline and get the predicted sentiment
		         pipelineFit.transform(wordsDataFrame).select('tweet','prediction').show()
	       except : 
		      print('No data')
    
             # initialize the streaming context 
      ssc = StreamingContext(sc, batchDuration= 3)

             # Create a DStream that will connect to hostname:port, like localhost:9991
      lines = ssc.socketTextStream(sys.argv[1], int(sys.argv[2]))

             # split the tweet text by a keyword 'TWEET_APP' so that we can identify which set of words is from a single tweet
      words = lines.flatMap(lambda line : line.split('TWEET_APP'))

             # get the predicted sentiments for the tweets received
      words.foreachRDD(get_prediction)

             # Start the computation
      ssc.start()             

             # Wait for the computation to terminate
      ssc.awaitTermination()  
      
   รันโปรแกรมในเทอร์มินัลเดียวและใช้ Netcat (เครื่องมือยูทิลิตี้ที่สามารถใช้ในการส่งข้อมูลไปยังชื่อโฮสต์และหมายเลขพอร์ตที่กำหนด) คุณสามารถเริ่มการเชื่อมต่อ TCP โดยใช้คำสั่งนี้
   
   	nc -lk port_number
    
      
      

