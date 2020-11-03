# Double DQN

核心思想，使用了两个神经网络分别执行不同的操作，来解决Q value被高估的问题。

选 action 的 Q-function 跟算 value 的 Q-function不是同一个。

 一个是 target 的 Q-network，一个是真正你会 update 的 Q-network。所以在 Double DQN 里面，你的实现方法会是拿你会 update 参数的那个 Q-network 去选 action，然后你拿 target 的network，那个固定住不动的 network 去算 value。

# Dueling DQN

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.4.png)

本来的 DQN 就是直接 output Q value 的值。现在这个 dueling 的 DQN，就是下面这个 network 的架构。它不直接output Q value 的值，它分成两条 path 去运算，第一个 path 算出一个 scalar，这个 scalar 我们叫做 V(s)。因为它跟input s 是有关系，所以叫做 V(s)V，V(s)是一个 scalar。下面这个会 output 一个 vector，这个 vector 叫做 A(s,a)。下面这个 vector，它是每一个 action 都有一个 value。然后你再把这两个东西加起来，就得到你的 Q value。

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.5.png)

# Prioritized Experience Replay

我们原来在 sample data 去 train 你的 Q-network 的时候，你是 uniformly 从 experience buffer 里面去 sample data。那这样不见得是最好的， 因为也许有一些 data 比较重要。假设有一些 data，你之前有 sample 过。你发现这些 data 的 TD error 特别大（TD error 就是 network 的 output 跟 target 之间的差距），那这些 data 代表说你在 train network 的时候， 你是比较 train 不好的。那既然比较 train 不好， 那你就应该给它比较大的概率被 sample 到，即给它 `priority`。这样在 training 的时候才会多考虑那些 train 不好的 training data。实际上在做 prioritized experience replay 的时候，你不仅会更改 sampling 的 process，你还会因为更改了 sampling 的 process，更改 update 参数的方法。所以 prioritized experience replay 不仅改变了 sample data 的 distribution，还改变了 training process。

在经验池中进行挑选的一个过程，根据不同的重要性对不同的数据选取有着不同的概率，从而使得结果更优。

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.7.png)

# Balance between MC and TD

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.8.png)

MC 跟 TD 的方法各自有各自的优劣，怎么在 MC 跟 TD 里面取得一个平衡呢？我们的做法是这样，在 TD 里面，在某一个 state$ s_t $采取某一个 action $a_t$ 得到 reward $r_t$，接下来跳到那一个 state$ s_{t+1}$。但是我们可以不要只存一个 step 的data，我们存 N 个 step 的 data。

# Noisy Net

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.9.png)

Noisy Net 的意思是说，每一次在一个 episode 开始的时候，在你要跟环境互动的时候，你就把你的 Q-function 拿出来，Q-function 里面其实就是一个 network ，就变成你把那个 network 拿出来，在 network 的每一个参数上面加上一个 Gaussian noise，那你就把原来的 Q-function 变成$ \tilde{Q}$ 。因为$ \hat{Q}$ 已经用过，$\hat{Q}$是那个 target network，我们用 $\tilde{Q} $来代表一个`Noisy Q-function`。我们把每一个参数都加上一个 Gaussian noise，就得到一个新的 network 叫做 $\tilde{Q}$。

在参数中加入高斯噪音。目的是为了有系统性的进行Explore。

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.10.png)

# Distributional Q-function

![img](https://datawhalechina.github.io/leedeeprl-notes/chapter7/img/7.11.png)

后面详细根据论文进行拆解。