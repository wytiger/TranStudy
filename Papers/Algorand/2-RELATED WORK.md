## 2 RELATED WORK
相关工作
**Proof-of-work.** Bitcoin [ 41 ], the predominant cryptocurrency, uses proof-of-work to ensure that everyone agrees on the set of approved transactions; this approach is often called “Nakamoto consensus”. 
Bitcoin must balance the length of time to compute a new block with the possibility of wasted work [ 41 ], and sets parameters to generate a new block every 10 minutes on average.
 Nonetheless, due to the possibility of forks, it is widely suggested that users wait for the blockchain to grow by at least six blocks before considering their transaction to be confirmed [7].
 This means transactions in Bitcoin take on the order of an hour to be confirmed.
 Many follow-on cryptocurrencies adoptBitcoin’s proof-of-work approach and inherit its limitations.
The possibility of forks also makes it difficult for new users to bootstrap securely: an adversary that isolates the user’s network can convince the user to use a particular fork of the blockchain [28].
工作量证明。比特币，主要的加密货币，使用工作量证明来确保每个人都认可区块的交易集合（简单说就是使用pow达成共识），这种共识机制经常被叫做“中本聪共识”。
比特币必须平衡计算新块的时间长度和浪费工作的可能性[ 41 ]，并设置参数以平均每10分钟生成一个新块。尽管如此，因为有分叉的可能性，人们普遍认为用户等待区块链增长至少六块才考虑他们的交易被确认[ 7 ]。这意味着比特币的交易需要一个小时的时间才能得到确认。许多后续的加密货币采用了比特币的pow同时也继承了他的局限性。分叉的可能性也使得引导新用户安全性变得困难：攻击者通过隔离用户网络，使用户使用一个特别的区块链分叉。


By relying on Byzantine agreement, Algorand eliminates the possibility of forks, and avoids the need to reason about mining strategies [ 8 , 25 , 46 ].
 As a result, transactions are confirmed on the order of a minute.
 To make the Byzantine agreement robust to Sybil attacks, Algorand associates weights with users according to the money they hold.
 Other techniques have been proposed in the past to resist Sybil attacks in Byzantine-agreement-based cryptocurrencies, including having participants submit security deposits and punishing those who deviate from the protocol [13].
依靠拜占庭协议，algorand消除了分叉的可能性，避免了挖矿的需要。因此，交易按一分钟就可以完成。为了使拜占庭协议面对对女巫攻击任然健壮，algorand将他们持有的资金与权重相关联。基于拜占庭协议的货币中，其他的技术在过去已经被提出以抵抗女巫攻击，包括参加者提交保证金和惩罚那些作恶者[ 13 ]。

**Byzantine consensus.** Byzantine agreement protocols have been used to replicate a service across a small group of servers, such as in PBFT [ 15 ].
 Follow-on work has shown how to make Byzantine fault tolerance perform well and scale to dozens of servers [ 1 , 17 , 33 ].
 One downside of Byzantine fault tolerance protocols used in this setting is that they require a fixed set of servers to be determined ahead of time;
 allowing anyone to join the set of servers would open up the protocols to Sybil attacks.
 These protocols also do not scaleto the large number of users targeted by Algorand.
BA⋆is a Byzantine consensus protocol that does not rely on a fixed set of servers, which avoids the possibility of targeted attacks on well-known servers.
 By weighing users according to their currency balance,BA⋆allows users to join the cryptocurrency without risking Sybil attacks, as long as the fraction of the money held by honest users is at least a constant greater than 2/3.
BA⋆’s design also allows it to scale to many users(e.g., 500,000 shown in our evaluation) using VRFs to fairly select a random committee.
拜占庭式共识。拜占庭协议被重写改进，通过一小组服务器，如PBFT [ 15 ]。接下来的工作展示了如何使拜占庭容错性能良好，并扩展到更多台服务器上[ 1, 17, 33 ]。拜占庭容错协议使用此设置的一个缺点是，它们需要提前确定一套固定的服务器；允许任何人加入服务器集合将使得对女巫攻击开放。这些协议还不能扩展到满足algorand用户数量。BA⋆是一个拜占庭共识的协议，不依赖于固定的一组服务器，避免了针对已知服务器攻击的可能性。根据他们的货币余额的权衡用户权重，BA⋆允许用户不用冒Sybil攻击风险加入货币网络，只要诚实的用户余额比例至少是大于2/3的常数。BA⋆的设计也可以扩展到许多用户（例如，在我们的评估中是500000），用以公平地选择随机委员会。



Most Byzantine consensus protocols require more than2 / 3 of servers to be honest, and Algorand’s BA⋆inherits this limitation (in the form of 2 / 3 of the money being held by honest users).
 BFT2F [ 35 ] shows that it is possible to achieve “fork∗-consensus” with just over half of the servers being honest, but fork∗-consensus would allow an adversary to double-spend on the two forked blockchains, which Algorand avoids.
 大多数的拜占庭式共识协议需要超过2/3的服务器是诚实的，algorand BA⋆ 继承了这个限制（以2/3的钱被诚实的用户持有）。bft2f [ 35 ]表明，它是可能实现“fork∗-consensus”的，只需要服务器超过1/2是诚实的，但"fork∗-consensus"会让对手在两个分叉的区块链上双花，Algorand避免了这个问题。
 
Honey Badger [ 39 ] demonstrated how Byzantine fault tolerance can be used to build a cryptocurrency.
 Specifically,Honey Badger designates a set of servers to be in charge of reaching consensus on the set of approved transactions.
This allows Honey Badger to reach consensus within 5 minutes and achieve a throughput of 200 KBytes/sec of data appended to the ledger using 10 MByte blocks and 104 participating servers.
 One downside of this design is that the cryptocurrency is no longer decentralized; there are a fixed set of servers chosen when the system is first configured.
Fixed servers are also problematic in terms of targeted attacks that either compromise the servers or disconnect them from the network.
 Algorand achieves better performance(confirming transactions in about a minute, reaching similar throughput) without having to choose a fixed set of servers ahead of time.
Honey Badger[ 39 ]展示了拜占庭容错可以用来建立一个加密货币。具体而言，Honey Badger指定了一组服务器，负责在批准的事务集上达成协商一致。这允许Honey Badger在5分钟内达到共识，实现200k/s的吞吐量数据追加到账本，使用10M字节块和104个参与服务器。这种设计的一个缺点是，加密货币不再是分散的；当系统首次配置时有一套固定的服务器选择。固定服务器在针对服务器的攻击或断开网络的目标攻击方面也存在问题。algorand达到更好的性能（确认交易在大约一分钟，达到类似的吞吐量）无需提前选择一组固定的服务器。
 
Bitcoin-NG [ 26 ] suggests using the Nakamoto consensus to elect a leader, and then have that leader publish blocks of transactions, resulting in an order of magnitude of improvement in latency of confirming transactions over Bitcoin.
Hybrid consensus [ 30 , 32 , 42 ] refines the approach of using the Nakamoto consensus to periodically select a group of participants (e.g., every day), and runs a Byzantine agreement between selected participants to confirm transactions until new servers are selected.
 This allows improving performance over standard Nakamoto consensus (e.g., Bitcoin); for example, ByzCoin [ 32 ] provides a latency of about 35 seconds and a throughput of 230 KBytes/sec of data appended to the ledger with an 8 MByte block size and 1000 participants in the Byzantine agreement.
 Although Hybrid consensus makes the set of Byzantine servers dynamic, it opens up the possibility of forks, due to the use of proof-of-work consensus to agree on the set of servers; this problem cannot arise in Algorand.
 比特币非盈利组织 [ 26 ]建议使用中本聪共识来选出一个领导者，然后由领导者发布交易区块，从而在确认交易比特币延迟方面提示一个数量级。混合共识[ 30, 32, 42 ]提炼了中本聪共识的使用，定期选择一组参与者（例如，每天），并在选定参与者中运行一个拜占庭协议来确认交易，直到到新的服务器被选择。这可以提高标准中本聪共识性能（例如，比特币）；例如，byzcoin [ 32 ]提供延迟35秒左右，吞吐量为230字节/秒，8M区块的大小和1000参与者。虽然混合共识的拜占庭服务器是动态的集合，它任然可能分叉，因为服务器集群使用pow共识来达成一致；这个问题不会出现在Algorand里。
 
 
Pass and Shi’s paper [ 42 ] acknowledges that the Hybrid consensus design is secure only with respect to a “mildly adaptive” adversary that cannot compromise the selected servers within a day (the participant selection interval), and explicitly calls out the open problem of handling fully adaptive adversaries.
 Algorand’sBA⋆explicitly addresses thisopen problem by immediately replacing any chosen committee members.
 As a result, Algorand is not susceptible to either targeted compromises or targeted DoS attacks.
 Pass and Shi 的论文指出， 混合共识的设计只针对那种温和的攻击者是安全的，这种攻击者在一天之内（见证人选举间隔）无法使服务器瘫痪，并明确提出了处理完全自适应的对手时的开放性问题。Algorand的BA*算法针对这个问题提出了解决办法，立刻更换被敌方选中的委员会成员，因此，Algorand不会受到针对性妥协或dos攻击的影响。
Stellar [ 36 ] takes an alternative approach to using Byzantine consensus in a cryptocurrency, where each user can trust quorums of other users, forming a trust hierarchy.
 Consistency is ensured as long as all transactions share at least one transitively trusted quorum of users, and sufficiently many of these users are honest.
 Algorand avoids this assumption, which means that users do not have to make complex trust decisions when configuring their client software.
 恒星币[ 36 ]以另一种方法使用拜占庭式的共识，即每个用户可以信任其他用户的法定人数，形成一个信任等级。共识能够达成，只要所有事务共享至少一个可信赖的群体用户，和足够多的这些用户都是诚实的。algorand并不需要这个假设前提，这意味着用户在配置客户机软件时不必做出复杂的信任决策。
 
 
**Proof of stake.** Algorand assigns weights to users proportionally to the monetary value they have in the system, inspired by proof-of-stake approaches, suggested as an alternative or enhancement to proof-of-work [ 3 , 10 ].
 There is a key difference, however, between Algorand using monetary value as weights and many proof-of-stake cryptocurrencies.
In many proof-of-stake cryptocurrencies, a malicious leader(who assembles a new block)can create a fork in the network, but if caught (e.g., since two versions of the new block are signed with his key), the leader loses his money.
 The weights in Algorand, however, are only to ensure that the attacker cannot amplify his power by using pseudonyms; as long as the attacker controls less than 1/3 of the monetary value, Algorand can guarantee that the probability for forks is negligible.
 Algorand may be extended to “detect and punish” malicious users, but this is not required to prevent forks or double spending.
 股份证明
 algorand分配用户权重，根据他们在系统中的货币价值的比例，从股权的方法证明，建议作为一种POW的替代或增强。但是有一个关键区别，在algorand使用货币价值量作为权重和许多股权证明货币之间。在许多其他股权证明的货币中，一个恶意的见证人可以在网络中创建分叉，但是如果被抓住，这个见证人将丢失他的钱。在Algorand中，只能确保攻击者无法通过使用假名来扩大他的影响。只要攻击者控制的价值少于1/3，Algorand能够保证分叉的可能性是微乎其微的，algorand可以扩展到“检测和惩罚“恶意用户，但这不需要叉或双花。
  
 
Proof-of-stake avoids the computational overhead of proof-of-work and therefore allows reducing transaction confirmation time.
 However, realizing proof-of-stake in practice is challenging [ 4 ].
 Since no work is involved in generating blocks, a malicious leader can announce one block, and then present some other block to isolated users.
 Attackers may also split their credits among several “users”, who might get selected as leaders, to minimize the penalty when a bad leader is caught.
 Therefore some proof-of-stake cryptocurrencies require a master key to periodically sign the correct branch of the ledger in order to mitigate forks [ 31 ].
 This raises significant trust concerns regarding the currency, and has also caused accidental forks in the past [ 43 ].
 Algorand answers this challenge by avoiding forks, even if the leader turns out to be malicious.
 股权证明避免了工作证明的计算开销，因此可以减少交易确认时间。然而，在实践中也需要认识到股权证明是会面临挑战性的[ 4 ]。由于没有涉及生成块的工作，恶意的领导者可以宣布一个块，然后向隔离的用户呈现其他的块。攻击者也可以把他们的存款分成几个“用户”，他们可能会被选为领导者，从而当一个坏领导被抓住时，将惩罚降至最低。因此，一些股权证明的加密货币需要一个主key在账本的正确分支上不断签名，来减少分叉。这引发了对货币的重大信任问题，并在过去[ 43 ]中也引起了意外的分叉。algorand解决了这一挑战避免分叉，即使领导原来是恶意的。
 
Ouroboros [ 30 ] is a recent proposal for realizing proof-ofstake.
 For security, Ouroboros assumes that honest users can communicate within some bounded delay (i.e., a strongly synchronous network).
 Furthermore, it selects some users to participate in a joint-coin-flipping protocol and assumes that most of them are incorruptible by the adversary fora significant epoch (such as a day).
 In contrast Algorand assumes that the adversary may temporarily fully control the network and immediately corrupt users in targeted attacks.
 Ouroboros（衔尾蛇）[ 30 ]是最近的一个实现股权证明的提议。为了安全，衔尾蛇假定诚实的用户能够在一些有限的延迟里进行沟通（即，一个强烈的同步网络）。此外，它还选择了一些用户参与一个联合掷币协议，并假设他们中的大部分人在一个重要的时代（比如一天）被对手破坏了。相反，Algorand认为对手可能暂时完全控制网络和有针对性的攻击，立即损坏用户。
 
**Trees and DAGs instead of chains.** GHOST [ 47 ], SPECTRE [ 48 ], and Meshcash [ 5 ] are recent proposals for increasing Bitcoin’s throughput by replacing the underlying chain structured ledger with a tree or directed acyclic graph (DAG)structures, and resolving conflicts in the forks of these data structures.
 These protocols rely on the Nakamoto consensus using proof-of-work.
 By carefully designing the selection rule between branches of the trees/DAGs, they are able to substantially increase the throughput.
 In contrast, Algorand is focused on eliminating forks; in future work, it may be interesting to explore whether tree or DAG structures can similarly increase Algorand’s throughput.
 使用数或者有向无环图代替链
 GHOST [ 47 ], SPECTRE [ 48 ], and Meshcash [ 5 ] 是最近用来提示比特币吞吐量的一些提议，通过使用树或者有向无环图来代替底层区块链的账本结构，并且解决区块链分叉的问题。这些协议依赖于使用工作量证明的中本聪共识，通过精心设计的树/图分支之间的选择规则，能够大幅提高吞吐量。相比之下，Algorand的重点是消除叉；在今后的工作中，可以探讨树或DAG结构同样可以增加Algorand的吞吐量。
 
 翻译：伍歌歌wytiger
