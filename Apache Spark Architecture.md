# Apache Spark Architecture – Spark Cluster Architecture Explained
ขอบคุณข้อมูลจาก https://www.edureka.co/blog/spark-architecture/



Apache Spark เป็น open-source cluster computing framework สำหรับ real-time data processing. ประสิทธิภาพจะเร็วขึ้น 100 เท่าในหน่วยความจำและเร็วกว่าดิสก์ 10 เท่าเมื่อเปรียบเทียบกับ Hadoop  มีลักษณะเป็น in-memory cluster computing และ data parallelism and fault tolerance

## Spark its Features


   ![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/09/Picture5-2-768x408.png)

- Speed
  
  Spark ทำงานเร็วกว่า Hadoop MapReduce ถึง 100 เท่าสำหรับการประมวลผลข้อมูลขนาดใหญ่ นอกจากนี้ยังสามารถทำความเร็วตลอดการควบคุมการแบ่งพาร์ติชัน
  
- Powerful Caching
  
  รูปแบบการเขียนโปรแกรมแบบง่ายมีความสามารถในการแคชและ disk persistence ที่มีประสิทธิภาพ
  
- Deployment

  มันสามารถปรับใช้ผ่าน Mesos, Hadoop via YARN, or Spark’s own cluster manager
  
- Real-Time

  มีการ Real-time computation  และ low latency เนื่องจาก in-memory computation.
  
- Polyglot

  Spark มี API ระดับสูงใน Java, Scala, Python และ R. Spark code สามารถเขียนได้ในสี่ภาษาเหล่านี้และยังมี shell ใน Scala และ Python 
  
- Scalable


## Spark Architecture Overview

Apache Spark Architecture ตั้งอยู่บนพื้นฐานหลักสองประการ: Resilient Distributed Dataset (RDD), Directed Acyclic Graph (DAG)
  

  
 ![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/09/2018-09-28-18_12_51-Apache-Spark-Architecture-_-Understanding-the-Spark-Components-_-Edureka.png)
 
## Spark Eco-System

   spark ประกอบด้วยส่วนประกอบต่าง ๆ เช่น Spark SQL, Spark Streaming, MLlib, GraphX และส่วนประกอบ Core API ยกตัวอย่างเช่น

- Spark Core

     Spark Core เป็นเครื่องมือพื้นฐานสำหรับการประมวลผลข้อมูลแบบขนานและขนาดใหญ่ นอกจากนี้ยังมี libraries ที่สร้างขึ้น on top of core งานที่หลากหลายสำหรับ      streaming, SQL, และ machine learning และทำหน้าที่ รับผิดชอบการจัดการหน่วยความจำ การกู้คืนข้อผิดพลาด (fault recovery), scheduling ,                distributing และการตรวจสอบงานในคลัสเตอร์และการโต้ตอบกับระบบจัดเก็บข้อมูล
     
- Spark Streaming

     Spark Streaming เป็นส่วนประกอบของ Spark ซึ่งใช้ในการประมวลผลข้อมูลการสตรีมแบบเรียลไทม์ ดังนั้นจึงเป็นประโยชน์เพิ่มเติมจาก Core Spark API มันช่วยให้        high-throughput และ fault-tolerant การประมวลผลสตรีมของสตรีมข้อมูลสด
     
## Resilient Distributed Dataset(RDD)

RDDs เป็นหน่วยการสร้างของแอปพลิเคชัน Spark ใด ๆ RDDs ย่อมาจาก:

Resilient: Fault tolerant และสามารถสร้างข้อมูลใหม่ได้เมื่อเกิดข้อผิดพลาด

Distributed: กระจายข้อมูลระหว่างหลายโหนดในคลัสเตอร์

Dataset: การรวบรวมข้อมูลที่แบ่งพาร์ติชันด้วย value

  ![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/07/Partitions.png)

  - ข้อมูลใน RDD ถูกแบ่งออกเป็นส่วน ๆ ตามคีย์  RDDs มีความยืดหยุ่นสูงสามารถกู้คืนได้อย่างรวดเร็วจากปัญหาใด ๆ  เนื่องจากมีการ replicate ข้อมูลบน multiple executor nodes ดังนั้นแม้ว่า executor node จะล้มเหลว node อื่นๆจะยังคงประมวลผลข้อมูล
  
  - นอกจากนี้ เมื่อคุณสร้าง RDD มันจะกลายเป็น immutable คือไม่สามารถแก้ไขสถานะได้หลังจากสร้างขึ้นแล้วแต่สามารถเปลี่ยนแปลงได้อย่างแน่นอน
  
  - แต่ละชุดข้อมูลใน RDD แบ่งออกเป็นโลจิคัลพาร์ติชัน ซึ่งอาจคำนวณบนโหนดต่าง ๆ ของคลัสเตอร์ ด้วยเหตุนี้คุณสามารถทำการเปลี่ยนแปลงหรือการดำเนินการกับข้อมูลที่สมบูรณ์แบบคู่ขนาน มีสองวิธีในการสร้าง RDDs 1.การแบ่งคอลเลกชันที่มีอยู่ในโปรแกรม 2.อ้างอิงชุดข้อมูลในระบบจัดเก็บข้อมูลภายนอกเช่นระบบไฟล์ที่ใช้ร่วมกัน, HDFS, HBase, ฯลฯ
  
RDD ดำเนินการสองประเภท:

1. Transformations: สร้าง RDD ใหม่

2. Actions: สั่งให้ Apache Spark ใช้การคำนวณและส่งผลลัพธ์กลับไปที่ไดรเวอร์


# Working of Spark Architecture

- ใน master node มี Driver program คุณสามารถเขียนโปรแกรมสั่ง Driver program หรือ ถ้าคุณใช้ interactive shell . Shell นั้น ก็ทำหน้าที่เป็น driver program ภายใน Driver program ลำดับแรก คุณสร้าง spark context เปรียบเสมือน geteway คล้ายกับการเชื่อมต่อฐานข้อมูลของคุณ คำสั่งใดๆ ที่คุณดำเนินการต้องผ่านการเชื่อมต่อข้อมูล คล้ายกัน ทุกสิ่งที่ทำบน spark ต้องผ่าน spark context

![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/09/Picture6-2-768x447.png)

- spark context ทำงานกับ cluster manager เพื่อจัดการงานต่างๆ โดย Driver program และ spark context ดูแลเรื่อง job execution ภายใน cluster โดย job หนึ่ง job จะถูก แตกออกเป็นหลายๆ task ซึ่งกระจายอยู่บน worker node ตลอดเวลา RDD จะถูกสร้าง ใน spark context มันสามารถกระจายข้าม node ต่างๆ และสามารถ cached ได้อีกด้วย
 
- worker nodes (slave nodes) ทำหน้าที่ execute tasks โดย task เหล่านี้จะถุกดำเนินการบน RDD ที่แบ่งพาร์ติชันแล้วใน worker node ดังนั้นผลลัพธ์จะถูกส่งกลับที่ spark context
    
