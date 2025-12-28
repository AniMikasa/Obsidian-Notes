### 研究具身智能的核心原则 (Core Principles)
[PKU EPIC](https://github.com/jiangranlv/embodied-ai-start?tab=readme-ov-file#3-%E7%A0%94%E7%A9%B6%E5%85%B7%E8%BA%AB%E6%99%BA%E8%83%BD%E7%9A%84%E6%A0%B8%E5%BF%83%E5%8E%9F%E5%88%99-core-principles)
- **首先把任务定义（task formulation）想清楚，而不是一开始就盯着模型**。在CV领域，研究者之所以可以直接关注模型，是因为任务往往已经被定义得很清晰，数据集也由他人整理好， 比如图像分类就是输入图片输出类别标签，检测就是输出四个数的bounding box；但在具身智能中，如何合理地建模任务、确定目标与评价指标，往往比模型选择更为关键。说白了，你得知道你想让机器人学会什么样的技能，输入是啥，输出是啥，用的什么传感器？你所研究的问题是否在合理的setting下？有没有有可能通过更好的setting来解决问题（比如机器人头部相机对场景观测不全，那我们可以考虑加装腕部相机，或者使用鱼眼相机）
- 必须认识到**用学习（learning）来解决机器人问题并不是理所当然的选择**。在许多场景中，传统的控制（Control）、规划（Planning）或优化方法（Optimization）依然高效且可靠，而学习方法更多是在任务复杂、环境多变(泛化性) 或缺乏解析建模手段时才展现优势。因此，做具身智能研究时，首先要想回答，为什么你研究的这件事传统robotics解决不了？为什么非得用learning？

## 科研工作中的必备能力
[PKU EPIC](https://github.com/jiangranlv/embodied-ai-start?tab=readme-ov-file#%E5%85%ABoptional-%E7%A7%91%E7%A0%94%E5%B7%A5%E4%BD%9C%E4%B8%AD%E7%9A%84%E5%BF%85%E5%A4%87%E8%83%BD%E5%8A%9B)
- **Sharp Mind**：戳穿别人工作的包装，看到本质  
    
- **Writing and Presentation**： 包装自己的工作，别让别人拆穿  
    
- **Warm Mind**：不要一味的批评别人的工作，能够欣赏到别人的亮点