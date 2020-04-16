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
  
## Spark Architecture Overview

Apache Spark Architecture ตั้งอยู่บนพื้นฐานหลักสองประการ: Resilient Distributed Dataset (RDD), Directed Acyclic Graph (DAG)
  

  
 ![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/09/2018-09-28-18_12_51-Apache-Spark-Architecture-_-Understanding-the-Spark-Components-_-Edureka.png)
 
## Spark Eco-System

   spark ประกอบด้วยส่วนประกอบต่าง ๆ เช่น Spark SQL, Spark Streaming, MLlib, GraphX และส่วนประกอบ Core API

 ![Push up to github](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2018/09/001-768x583.png)

- Spark Core

Spark Core เป็นเครื่องมือพื้นฐานสำหรับการประมวลผลข้อมูลแบบขนานและขนาดใหญ่ นอกจากนี้ยังมี libraries ที่สร้างขึ้น on top of core งานที่หลากหลายสำหรับ streaming, SQL, และ machine learning และทำหน้าที่ รับผิดชอบการจัดการหน่วยความจำ การกู้คืนข้อผิดพลาด (fault recovery), scheduling , distributing และการตรวจสอบงานในคลัสเตอร์และการโต้ตอบกับระบบจัดเก็บข้อมูล
