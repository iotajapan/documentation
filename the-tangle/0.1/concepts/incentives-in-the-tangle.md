# タングル内のインセンティブ
<!-- # Incentives in The Tangle -->

**チップ選択アルゴリズムは強制されていません。その代わりに、ノードは、自分のトランザクションが確定される最良の機会を得るためにチップ選択アルゴリズムを使用するインセンティブがあります。**
<!-- **The tip selection algorithm is not enforced. Instead, nodes have an incentive to use it to have the best chance of their transactions becoming confirmed.** -->

ノードがチップ選択アルゴリズムを使用したかどうか、または独自の目的のためにカスタムのチップトランザクションを返すように変更したかどうかを確認することはできません。
<!-- It's impossible to check if a node used the tip selection algorithm or even changed it to return custom tip transactions for its own purposes. -->

ただし、[ホワイトペーパー](https://iota.org/IOTA_Whitepaper.pdf)で説明したように、チップトランザクションはランダムに選択される必要があります。
<!-- However, as discussed in the [whitepaper](https://iota.org/IOTA_Whitepaper.pdf), it's necessary that tip transactions are selected at random. -->

ノードが独自のチップトランザクション（たとえば、自分のトランザクションを承認するトランザクション）を選択することを選択した場合、少数の幸運なトランザクションしか確定されないため、ランダム性は重要です。この理論については、「[Equilibria in the tangle](https://arxiv.org/abs/1712.05385)」という論文で説明します。
<!-- This randomness is important because if nodes chose to select their own tip transactions (for example, those that approve their own transactions), only a few lucky transactions would ever be confirmed. We discuss this theory in the paper [Equilibria in the tangle](https://arxiv.org/abs/1712.05385). -->

![Nodes select the best tip transactions](https://cdn-images-1.medium.com/max/1600/1*Qs_KFwcXxXKuoERjfJ5xsw.jpeg)

上図では、黒色のトランザクションが最良と見なされ、新しい各トランザクションは黒色のトランザクションを参照しようとします。この状況では、すべてのトランザクションが確定される割合を減らします。
<!-- In this diagram, the black transactions are considered the best, and each new transaction tries to reference them. This situation reduces the rate at which all transactions are confirmed. -->

すべてのノードがチップ選択アルゴリズムを使用すると、すべてのチップトランザクションがランダムに選択されます。このランダム性は、新しいトランザクションがタングルに添付されて最終的に確定される割合を増加させます。
<!-- When all nodes use the tip selection algorithm, all tip transactions are selected at random. This randomness increases the rate at which new transactions are attached to the Tangle and eventually confirmed. -->

![Comparison between a random tip selection and a non-random tip selection](https://cdn-images-1.medium.com/max/1600/1*qvNmyzQijU3PpMYvYtaxGg.jpeg)
