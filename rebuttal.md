We sincerely thank the reviewers for the constructive comments. We clarify concerns below and will make them clear in the revision.


**Reviewer A:**

**1. Question about The backdoor targeted attacks can be easily tackled by the backdoor defense methods, and the sample-wise targeted attacks can only cause limited malicious effects due to the high cost of deployments, especially when deployed to hardware devices.**
- To the best of our knowledge, there are no existing defense methods that are specific to targeted bit-flip attacks. Therefore, we try our best to compare with other methods.
- We also compare with the tradtional defense methods, and results are listed:

To do list:
(1) No bit-flip based targeted attacks published.
(2) ziyuan runs backdoor attacks to validate
(3) Qiu clarifies why existing backdoors cannot be applied to our scenrios.


**2. Question about the This paper evaluates the defense performance against targeted attacks but comparing with defense methods designed for untargeted attacks. This paper should also include the defense methods for backdoor attacks as the comparison methods**
-	Backdoor attacks relys on the effectiveness of the triggers.  We have restricted the effectiveness of the triggers.

(1) ziyuan 


**3. Question about Some literature is missing in the related works and experiment comparisons** (to do)
-	This work focuses on untargeted attack, and not been published yet. We will cite this work and compare with it.
(1) validate this point, 
(2) leave this in our future work,
(3) citation for unpublished,

**4. Question about the increments of model size.**
-  model size will increase, cifar10, resnet-32 12M, 
-  However, Aegis could decrease inference-latency, and results are listed in 
-  Beasides, we can reduce the number of ICs, and its a tradeoff between defense effectiveness and the model size. Further, the number of parameters in ICs is adjustable, which may further restrict model size increments.


**5. Question about selecting the shallow layers.**
-	For each inference, Aegis randomly selects ICs for inference, while other ICs will be masked. Therefore, only selecting shallow layers to attack could not beat Aegis, as shallow layers are masked in many scenarios (we show samples uniform exit from different ICs in Figure 6). 
We also validate this point. Taking adaptive TBT as an example, we make it focus on attacking $IC_{1-3}$ (denoted as **Shallow**). Results are shown as follows. We find the ASR on **Shallow** is lower than Aegis, indicating that attacking shallow layers could not beat us.
- Evaluation results of ASR against adaptive TBT. 

| Dataset | Model | BASE | SDN | Aegis | **Shallow** |
| :--------: | :------------: | :-----: | :-------: | :----: | :----: |
| CIFAR-10 | ResNet32 | 70.7 | 37.2 | 31.1 | **14.2** |
| CIFAR-10 | VGG16 | 71.1 | 86.5 | 58.1 | **19.5** |
|  CIFAR-100 | ResNet32 | 95.8 | 79.3 | 49.7 | **18.6** |
|  CIFAR-100 | VGG16 | 65.9 | 85.9 | 44.8 | **16.4** |
| STL-10 | ResNet32| 100.0 | 35.0 | 31.8 | **9.8**|
| STL-10 | VGG16| 64.1 | 93.0 | 27.0 | **11.2**|
| Tiny-ImageNet | ResNet32| 100.0 | 96.3 | 28.2 | **14.6**|
| Tiny-ImageNet | VGG16| 69.7 | 63.4 | 54.4 | **18.5**|

**6. Question about the claim of model-agnostic is not fully correct since the number of added layers depends on the depth of the target model. Also, the added layers will need to be re-train, the training process also depends on the pre-trained target model**
-	qiu 查一下model-agnostic的定义，


**Reviewer B:**

**1. Question about Elaborate on the dataset splits and strictly separate data training/validation from testing data if not done already (please refer to Arp et al., "Dos and Don'ts of Machine Learning in Computer Security," USENIX SEC 2022)**
-	qiu, read this paper.

**2. Question about the ablation study.**
-	We follow your advice to extend upon the ablation study to include the remaining datasets under basic and adaptive ProFlip.  
-  Impact of ROB on basic and adaptive ProFlip. 

| Dataset | Model | DESDN | Aegis | DESDN (Adaptive) | Aegis (Adaptive) |
| :--------: | :------------: | :-----: | :-------: | :----: | :----: |
| CIFAR-10 | ResNet32 | 24.1 | 19.8 | 45.1 | 38.4 |
| CIFAR-10 | VGG16 | 33.7 | 28.9 | 49.4 | 43.6 |
|  CIFAR-100 | ResNet32 | 29.8 | 19.2 | 39.2 | 25.8 |
|  CIFAR-100 | VGG16 | 28.7 | 20.3 | 51.4 | 33.7 |
| STL-10 | ResNet32| 36.2 | 33.9 | 45.0 | 41.3 |
| STL-10 | VGG16| 22.9 | 18.7 | 39.6 | 34.5 |
| Tiny-ImageNet | ResNet32| 33.4 | 20.1 | 45.4 | 36.1 |
| Tiny-ImageNet | VGG16| 22.9 | 15.6 | 50.2 | 40.8 |


**3. Question about Variance/Std Deviation.**
-	The evaluation under attacks contains randomness, as Aegis randomly selects ICs for inference. To mitigate this, we repeat each attack experiments for 10 times, and the ASR varies below $2\%$. We will follow your advice to calculate the variance/std deviation in the revision.

**Reviewer C:**

**1. Question about Perform precise bit flip attacks on a real system and then demonstrate the defense.**
- Both untargeted/targeted bit-flip attacks need to precisely flip the specific bits [1]
- ？

**2. Question about this work compare to methods that aim to eliminate bit flips, such as ECC?**
- 有些设备是没有ECC保护的，nvidia-nano是否有ECC?
- 正交的；


**Reviewer D:**
**1. Question about  I would suggest that you quantify the feasibility of these attacks in terms of time and cost. How expensive is it to flip 1 bit? 10 bits? 100 bits? 1000 bits?.**
-	我们未来测试，放到ablation study里，举例子TA-LBF需要多久；解释50（引用原文），时间很久，而且我们在figure5里也做了，

**2. Question about measure the number of bits that are required to be flipped for an attack to be successful.**
-	Figure 5 做了这个实验，我们发现需要几天，

**3. Question about the paper is a little bit misleading in saying that the models do not need to be retrained. For AEGIS to work, the early exit ICs need to be trained with a similar cost to the original training. 3X needs to upon**
-	这个取决于数据集与模型，我们测试了所有模型，发现最差的是3倍（因为tiny-imagenet更难，limitted sample per class），最好的是8倍, 这里是一个tradeoff，如果我们降低acc的话，降低2%，可以提升到15倍。

**4. Question about the feasibility of the attacks is not sufficiently discussed and the evaluation should have been measuring the number of bit flips required for success**
-	We have evaluate this point and results show that they are 


**Reviewer E:**

**1. Question about what is N. Basically my intuition is that for your defense, a good attack would be to meddle with the early neurons in the base model (not in the ICs).**
-	

**2. Question about the computational load of re-training vs Aegis was not evaluated**
-	We have evaluate this point and results show that they are 

**3. Question about how do you compute the gradient of a function wrt a specific bit since the kth bit is either 0 or 1**
-	follow proflip attacks.

**4. Question about how to tune qq and \tauτ by running various attacks and setting them accordingly?**
-	We adjust them for accuracy and exit uniform, 这也是我们和sdn的区别，sdn是不均匀的。
but not specific to some attacks. Users could also adjust them in their own scenarios.
S


**References**

[1] Harel-Canada, Fabrice, et al. "Bit-Flip Attack: Crushing Neural Network with Progressive Bit Search" ICCV 2019.


