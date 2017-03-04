---
layout: default
title: Association 
---

# Association rules

[Association rules](https://en.wikipedia.org/wiki/Association_rule_learning) mean finding the items that group together more frequently. It is a method for discovering relations between variables. This algorithms are used in shops to figure out which products are usually sell together to make recommendations. For example if you buy sausage and hotdog bread you may need to buy ketchup or mustard.



## Definitions

### Items

Items are binary attributes that can or not belong to a transaction. Each transaction has a unique identifier. For example a transaction can be a purchase in a shop, and the items of that transaction are *Sausages* and *Mustard*.
$$I={i_1,i_2,...,i_n}$$ is the collection of all the available items.
$$D={t_1,t_2,....t_m}$$ is the set of all the transactions.

### Rule

A rule is an implication of the form $$X \Rightarrow Y$$ where $$X,Y \subseteq  I$$ and $$X \cap Y=\varnothing$$. The itemsets X is the *antecedent* (left-hand-side or LHS) and Y is the *consequent* (right-hand-side or RHS).


### Support

The *support* of an itemset is defined as the proportions of *transactions T* in the *dataset D* which contains that *itemset*. This is:
\\[
supp(X)=\frac{\left | \left \\{ T \in D | X \subseteq T \right \\} \right | }{\left | D  \right |}
\\]
This is can be considered an estimate of P(X), the probability to find this *itemset* in the transactions dataset.

### Confidence

The *confidence* indicates how often the rule is accomplished, it is defined as:
\\[
conf(X \Rightarrow Y)=\frac{supp(X \cup Y)}{supp(X)}
\\]
Which is estimate an of $$P(Y|X)=\frac{P(Y,X)}{P(X)}$$

### Lift

Another measure is the *lift* wich can be interpreted as the deviation of the support of the whole rule from the support expected under independence given the supports of the LHS and the RHS. Greater lift values indicate stronger associations. 


\\[
lift(X \Rightarrow Y)=\frac{supp(X \cup Y)}{supp(X)·supp(Y)}
\\]


## Apriori

This is by far the most popular associative algorithm. This algorithm can be described in the following steps:
1. We start with the complete list of itemsets and extract all the items.
2. With all the items create a list of itemsets of only one item.
3. Calculate the Support from each of the itemsets.
4. Remove the itemsets with a support value lower than the user defined threshold.
5. Increase the remaining itemsets with one new item, with all the possible combinations.
6. Goto 3 until all the itemsets in the original list has been inspected.

First we need to define a support value. In this case we will set it to 0.6.

Let's assume that we have in the database the following itemsets:


Then we select the items:

{: .table-border}
| Itemset| Support |
|-------|---------|
| **Item1** | **$$ \frac{4}{5}=0.8 $$** |
| **Item2** | **$$ \frac{4}{5}=0.8 $$** |
| **Item3** | **$$ \frac{4}{5}=0.8 $$** |
| **Item4** | **$$ \frac{2}{5}=0.4 $$** |
| _Item5_ | _$$ \frac{1}{5}=0.2 $$_ |




{: .table-border}
| Itemset| Support |
|-------|---------|
| {Item1, Item2} | $$ \frac{2}{5}=0.4 $$ |
| **{Item1, Item3}** | **$$ \frac{3}{5}=0.6 $$** |
| {Item1, Item4} | $$ \frac{1}{5}=0.2 $$ |
| **{Item2, Item3}** | **$$ \frac{3}{5}=0.6 $$** |
| {Item2, Item4} | $$ \frac{2}{5}=0.4 $$ |
| {Item3, Item4} | $$ \frac{1}{5}=0.2 $$ |



{: .table-border}
| Itemset| Support |
|-------|---------|
| {Item1, Item2, Item3} | $$ \frac{2}{5}=0.4 $$ |
| {Item1, Item2, Item4} | $$ \frac{1}{5}=0.2 $$ |
| {Item1, Item3, Item4} | $$ \frac{1}{5}=0.2 $$ |
| {Item2, Item3, Item4} | $$ \frac{1}{5}=0.2 $$ |


{: .table-border}
| Itemset| Support |
|-------|---------|
| {Item1, Item2, Item3, A5} | $$ \frac{1}{5}=0.2 $$ |




First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column


![Alt text](https://g.gravizo.com/g?
  digraph G {
    aize ="4,4";
    main [shape=box];
    main -> parse [weight=8];
    parse -> execute;
    main -> init [style=dotted];
    main -> cleanup;
    execute -> { make_string; printf};
    init -> make_string;
    edge [color=red];
    main -> printf [style=bold,label="100 times"];
    make_string [label="make a string"];
    node [shape=box,style=filled,color=".7 .3 1.0"];
    execute -> compare;
  }
)

```R
> node1<-c(T,T,T,F,T,F,T,T,T,F)
> node2<-c(T,F,F,T,T,F,T,T,F,T)
> node3<-c(F,F,T,F,T,F,T,T,F,F)
> node4<-c(F,F,T,T,T,T,F,F,F,F)
> node5<-c(F,T,T,F,T,F,T,F,F,F)
> node6<-c(F,F,T,T,T,T,F,F,F,F)
> node7<-c(F,F,F,F,T,F,F,F,F,F)
> trans<-as(data.frame(node1,node2,node3,node4,node5,node6,node7),"transactions")
> inspect(trans)
     items                                       transactionID
[1]  {item1,item2}                               1            
[2]  {item1,item5}                               2            
[3]  {item1,item3,item4,item5,item6}             3            
[4]  {item2,item4,item6}                         4            
[5]  {item1,item2,item3,item4,item5,item6,item7} 5            
[6]  {item4,item6}                               6            
[7]  {item1,item2,item3,item5}                   7            
[8]  {item1,item2,item3}                         8            
[9]  {item1}                                     9            
[10] {item2}                                     10           
> image(trans)
> rules<-apriori(trans, parameter=list(support=0.3, confidence=0.9),control = list(verbose=F))
> inspect(rules)
    lhs              rhs     support confidence lift    
[1] {item4}       => {item6} 0.4     1          2.500000
[2] {item6}       => {item4} 0.4     1          2.500000
[3] {item5}       => {item1} 0.4     1          1.428571
[4] {item3}       => {item1} 0.4     1          1.428571
[5] {item3,item5} => {item1} 0.3     1          1.428571
[6] {item2,item3} => {item1} 0.3     1          1.428571
```




Download the data from:
https://www.kaggle.com/c/titanic/download/train.csv

> titanic<-read.csv(file="train.csv")
> titanic$Age<-factor(cut(titanic$Age,c(0,16,100)),labels=c("child","adult"))
> titanic$Survived<-factor(titanic$Survived,labels=c("No","Yes"))
> titanic$Pclass<-factor(titanic$Pclass)

> rules <- apriori(titanic)
> rules <- apriori(titanic, appearance = list(rhs=c("Survived=No", "Survived=Yes"), default="lhs"))
> rules<-sort(rules,by="lift")
> rulesSubset <- is.subset(rules, rules)
> rulesSubset[lower.tri(rulesSubset, diag=T)] <- NA
> redundant<-colSums(rulesSubset, na.rm=T)>=1
> rules<-rules[!redundant]
> plot(rules,method="graph", control=list(type="items"))










