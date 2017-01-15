---
layout: default
title: Association 
---

## Definitions

### Items

Items are binary attributes that can or not belong to a transaction. Each transaction has a unique identifier. For example a transaction can be a purchase in a shop, and the items of that transaction are *Sausages* and *Mustard*.
$$I={i_1,i_2,...,i_n}$$ is the collection of all the available items.
$$D={t_1,t_2,....t_m}$$ is the set of all the transactions.

### Rule

A rule is an implication of the form $$X \Rightarrow Y$$ where $$X,Y \subseteq  I$$ and $$X \cap Y=\varnothing$$. The itemsets X is the *antecedent* (left-hand-side or LHS) and Y is the *consequent* (right-hand-side or RHS).



### Support

The *support* of an itemset is defined as the proportions of *transactions T* in the *dataset D* which contains that *itemset*.
\\[
supp(X)=\frac{\left | \left \\{ T \in D | X \subseteq T \right \\} \right | }{\left | D  \right |}
\\]
This is can be considered an estimate of P(X), the probability to find this *itemset* in the transactions dataset.

### Confidence

The *confidence* is defined as:
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