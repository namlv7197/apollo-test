# apollo-test

Data pipeline

Author: Le Viet Nam

## Create Kafka cluster
[repo](https://github.com/namlv7197/kafka-cluster)

Build Kafka cluster including 3 nodes for anyone to create and listen topic.

Bootstrap servers: 54.255.215.10:9092,13.212.251.13:9092,13.212.207.232:9092

List of topics:
- bao_tuoi_tre_topic: Store messages as binary collected by Crawler
- bao_tuoi_tre_word_counter: Store messages as binary processed by Spark
  
Message structure of bao_tuoi_tre_topic decoded is a dictionary type
|Key|Description|Value|
|-|-|-|
|news_id|Id of news|20230619045746463|
|link|News URL|https://tuoitre.vn/tin-tuc-the-gioi-19-6-mat-tran-mien-nam-ukraine-tan-bao-nhat-xa-sung-bat-thuong-nhieu-noi-o-my-20230619045746463.htm|
|title|Title of news|Tin tức thế giới 19-6: Mặt trận miền nam Ukraine 'tàn bạo nhất'; Xả súng bất thường nhiều nơi ở Mỹ|
|summary|Short content of news|Nga nói Ukraine chịu tổn thất lớn chỉ trong một ngày; Xả súng tại nhiều bang ở Mỹ; Triều Tiên thừa nhận vụ phóng vệ tinh là "thất bại nghiêm trọng nhất"... là một số tin tức thế giới đáng chú ý sáng 19-6.|
|content|Long content of news||
|author|Author of news|BÌNH AN|
|main_category|main category of news|Thế giới|
|sub_categories|list of sub categories of news||
|upload|The time news published|19/06/2023 06:42 GMT+7|
|comments|number of comments of news|0|

Message structure of bao_tuoi_tre_word_counter decoded is a dictionary type
|Key|Description|Value|
|-|-|-|
|news_id|Id of news|20230619045746463|
|tokens|Dictionary of tokens exclude single characters|{"token": "frequency"}|

## Create Crawler
[repo](https://github.com/namlv7197/vietnamese-news-crawler)

This source will collect data from [bao tuoi tre](https://tuoitre.vn/) and use Kafka send it to bao_tuoi_tre_topic topic

Input: https://tuoitre.vn/tin-moi-nhat.htm

Output: Message

## Word Counter with Spark Standalone
[repo](https://github.com/namlv7197/vietnamese-news-crawler-word-counter)

This source will listen messages produced by bao_tuoi_tre_topic, then count word frequency and send the output to bao_tuoi_tre_word_counter topic by Spark query streaming

Input: Message from bao_tuoi_tre_topic

Output: Message of bao_tuoi_tre_word_counter

Note: There are some limitations about finance so I can not run this source on AWS

