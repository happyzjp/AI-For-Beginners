# 深度强化学习



强化学习 (RL) 被视为基本机器学习范例之一，紧随监督学习和无监督学习之后。虽然在监督学习中我们依赖于具有已知结果的数据集，但 RL 基于通过实践进行学习。例如，当我们第一次看到电脑游戏时，我们开始玩，即使不知道规则，很快我们就能仅仅通过玩和调整我们的行为来提高我们的技能。

## [ 课前测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/122)



要执行 RL，我们需要：

- 一个设置游戏规则的环境或模拟器。我们应该能够在模拟器中运行实验并观察结果。
- 一些奖励函数，它指示我们的实验有多成功。在学习玩电脑游戏的情况下，奖励将是我们的最终得分。

基于奖励函数，我们应该能够调整我们的行为并提高我们的技能，以便下次我们玩得更好。其他类型的机器学习和 RL 之间的主要区别在于，在 RL 中，我们通常不知道我们在完成游戏之前是否会赢或输。因此，我们不能说某个动作本身是好还是坏——我们只在游戏结束时收到奖励。

在 RL 期间，我们通常会执行许多实验。在每次实验期间，我们需要在遵循我们迄今为止学到的最优策略（利用）和探索新的可能状态（探索）之间取得平衡。

## OpenAI Gym



强化学习的一个好工具是 OpenAI Gym - 一个模拟环境，它可以模拟许多不同的环境，从雅达利游戏到平衡杆背后的物理原理。它是训练强化学习算法最流行的模拟环境之一，由 OpenAI 维护。

> 注意：您可以在此处查看 OpenAI Gym 提供的所有环境。

##  平衡小车



您可能都见过诸如 Segway 或 Gyroscooters 等现代平衡设备。它们能够通过根据加速度计或陀螺仪的信号调整其车轮来自动平衡。在本节中，我们将学习如何解决一个类似的问题——平衡一根杆子。这类似于马戏团表演者需要用手平衡一根杆子的情况——但这种杆子平衡只发生在 1D 中。

平衡的简化版本称为 CartPole 问题。在 CartPole 世界中，我们有一个可以左右移动的水平滑块，目标是在滑块移动时在其顶部平衡一根垂直杆子。

[![a cartpole](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole.png)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole.png)

要创建和使用此环境，我们需要几行 Python 代码：

```
import gym
env = gym.make("CartPole-v1")

env.reset()
done = False
total_reward = 0
while not done:
   env.render()
   action = env.action_space.sample()
   observaton, reward, done, info = env.step(action)
   total_reward += reward

print(f"Total reward: {total_reward}")
```



每个环境都可以完全相同的方式访问：

- `env.reset` 开始一个新实验
- `env.step` 执行模拟步骤。它从动作空间接收动作，并返回观察（来自观察空间），以及奖励和终止标志。

在上面的示例中，我们在每一步执行随机动作，这就是实验生命非常短的原因：

[![non-balancing cartpole](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole-nobalance.gif)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole-nobalance.gif)

强化学习算法的目标是训练一个模型 - 所谓的策略 π - 它将根据给定的状态返回动作。我们还可以考虑策略是概率性的，例如对于任何状态 s 和动作 a，它将返回我们应该在状态 s 中采取 a 的概率 π(a|s)。

## 策略梯度算法



建模策略最明显的方法是创建一个神经网络，它将状态作为输入，并返回相应的动作（或更确切地说，所有动作的概率）。从某种意义上说，它类似于一个普通的分类任务，但有一个主要区别——我们事先不知道在每个步骤中应该采取哪些动作。

这里的想法是估计这些概率。我们构建一个累积奖励向量，它显示了我们在实验的每个步骤中的总奖励。我们还通过将较早的奖励乘以某个系数 γ=0.99 来应用奖励折扣，以减少较早奖励的作用。然后，我们沿着产生更大奖励的实验路径强化这些步骤。

> 详细了解策略梯度算法，并在示例笔记本中查看其实际应用。

##  Actor-Critic 算法



策略梯度方法的改进版本称为 Actor-Critic。其背后的主要思想是神经网络将被训练以返回两件事：

- 策略，它决定采取什么行动。这部分称为 actor
- 我们期望在此状态下获得的总奖励的估计 - 这部分称为 critic。

从某种意义上说，此架构类似于 GAN，其中我们有两个相互训练的网络。在 actor-critic 模型中，actor 提出我们需要采取的行动，而 critic 试图批判并估计结果。但是，我们的目标是同时训练这些网络。

因为我们知道实验期间 critic 返回的真实累积奖励和结果，所以构建一个最小化它们之间差异的损失函数相对容易。这将给我们带来 critic 损失。我们可以使用与策略梯度算法中相同的方法来计算 actor 损失。

在运行其中一种算法后，我们可以预期我们的 CartPole 会像这样表现：

[![a balancing cartpole](https://github.com/happyzjp/AI-For-Beginners/raw/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole-balance.gif)](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/6-Other/22-DeepRL/images/cartpole-balance.gif)

## ✍️ 练习：策略梯度和 Actor-Critic RL



在以下笔记本中继续学习：

- [ TensorFlow 中的强化学习](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/6-Other/22-DeepRL/CartPole-RL-TF.ipynb)
- [ PyTorch 中的强化学习](https://github.com/happyzjp/AI-For-Beginners/blob/main/translations/zh_cn/6-Other/22-DeepRL/CartPole-RL-PyTorch.ipynb)

##  其他强化学习任务



强化学习如今是一个快速发展的研究领域。强化学习的一些有趣示例包括：

- 教计算机玩雅达利游戏。此问题的难点在于，我们没有以向量形式表示的简单状态，而是一个屏幕截图——我们需要使用 CNN 将此屏幕图像转换为特征向量或提取奖励信息。雅达利游戏可在 Gym 中获得。
- 教计算机玩棋盘游戏，例如国际象棋和围棋。最近，Alpha Zero 等最先进的程序通过两个代理相互对抗并逐步改进而从头开始接受训练。
- 在工业中，RL 用于根据模拟创建控制系统。名为 Bonsai 的一项服务专门为此而设计。

##  结论



我们现在已经学会了如何训练代理，只需为它们提供定义游戏所需状态的奖励函数，并让它们有机会智能地探索搜索空间，即可获得良好的结果。我们已经成功尝试了两种算法，并在相对较短的时间内取得了良好的结果。然而，这仅仅是您进入 RL 旅程的开始，如果您想深入挖掘，您绝对应该考虑参加单独的课程。

##  🚀 挑战



探索“其他 RL 任务”部分中列出的应用程序，并尝试实现一个！

## [ 课后测验](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/222)



##  复习与自学



在我们的机器学习初学者课程中了解有关经典强化学习的更多信息。

观看此精彩视频，了解计算机如何学会玩超级马里奥。

## 作业：训练山地车



您在此任务期间的目标是训练不同的 Gym 环境 - 山地车。
