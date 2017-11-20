# Summary of Adversarial Examples for Evaluating Reading Comprehension Systems
 by Robin Jia and Percy Liang

## Abstract
In this paper, the authors proposed adversarial evaluation on Stanford Question Answering Dataset (SQuAD). Without confusing humans, the generated sentence distracted the models and their average F1 score dropped from 75% to 36%. This indicates that nowadays
the reading comprehension system techniques remain a large gap of improvement.

## Introduction
### SQuAD
Stanford Question Answering Dataset [(SQuAD)](https://rajpurkar.github.io/SQuAD-explorer/) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles. Each question refers to a paragraph and the answer is a span in the paragraph. Currently, the most accurate model achieves F1 score of 86.45% while human performance is 91.20%.

### Adversarial Examples
In computer vision, small perturbations were added on image but did not change the label of it. However, in a paragraph, even a word change can differ the meaning. Hence, they generate sentence and append it to the end of the paragraph to fool the model without confusing human. While models in computer vision suffer from *oversensitivity* to noise in images, this paper focuses on *overstability*, which means that a model cannot determine which sentence truly answers the question from two alike sentences.

![Image](/examples.png){: .center-image height="50%" width="50%"}

Images from Szegedy et al. (2014).

As we can see in the above table, adding adversaries in image classifications makes the model consider two images are different while the perturbations do not change. For reading comprehension tasks, adversaries cause the model to ignore the semantics changes. 

## Concatenative Adversaries
In this paper, the authors did not use paraphrasing as adversaries, since it is challenging to edit sentence without changing its meaning. Instead, they use concatenative adversaries. The form is listed below:
```markdown
(p + s, q, a)
```
, where p is a paragraph, s is an additional sentence, q is the question and a is the answer.

Next, they propose two concatenative adversaries and two of their variants. 

### ADDSENT
*ADDSENT* generates a sentence that is similar to the question but does not contradict the answer. There are four steps in *ADDSENT*.
1.  Apply semantics-altering perturbation to the question.
    ```
    What city did Tesla move to in 1880? -> What city did Tadakatsu move to in 1881?
    ```
2.  Create a fake answer that has the same type of the original answer.
    ```
    Prague -> Chicago
    ```
3.  Combine the mutated question and the fake answer into declarative form.
    ```
    Tadakatsu moved the city of Chicago to in 1881.
    ```
4.  Crowdworkers fix the error in the sentences.
    ```
    Tadakatsu moved to the city of Chicago in 1881.
    ```

![Image](/AddSent.png){: .center-image height="50%" width="50%"}

    
### ADDONESENT
*ADDONESENT* adds a random human-approve sentence. It does not require any queries to the model. 

### ADDANY
*ADDANY* generates a sentence `s` containing `d` words, `s = w1w2...wd`.
1. Initialize `w1w2...wd`. Randomly choose words from common English words. 
2. Switch `wi` with word `x`. word `x` is in candidate words `W` where `W = union of 20 sampled common words and words in q`.
3. Update `wi` to minimize the F1 score of the model's output.

![Image](/AddAny.png){: .center-image height="50%" width="50%"}

### ADDCOMMON
*ADDCOMMON* is same as *ADDANY* but only adds common words.


## Experiments
### Adversarial Evaluation
This experiment uses four types of concatenative adversaries on four models: Match-LSTM (single version), Match-LSTM (ensemble version), BiDAF model (single version) and BiDAF model (ensemble version).

![Image](/Adversarial.png){: .center-image height="50%" width="50%"}


The result shows that the performance of all models drop on every types of adversary. They also ran *ADDSENT* on other 12 models and found that the average F1 score fell from 75.4% to 36.4%. This means that concatenative adversaries can indeed fool the model. 

### Human Evaluation
This experiment is to ensure that human can answer correctly after adding adversaries. As we can see in the below table, the accuracy of human prediction did not decrease a lot. 

![Image](/Human.png){: .center-image height="30%" width="30%"}


### What Went Wrong and What Went Right
In adversarial evaluation, they divided the output of the models into two cases:  
1. Model failure: The answer got wrong during adversarial evaluation.
2. Model success: The answer remains correct during adversarial evaluation.

In *ADDSENT* examples, 96.6% of the model failures predict a span in the adversarial sentence. For model success, it usually happens when the question has n-gram match in the original paragraph. In addition, the model does well when the question is shorter. 

### Transferability across Models
Here "Transferability" means that an adversarial example that fools one model can also fool another. The result shows that *ADDSENT* has good transferability while *ADDANY* is quite limited in transferring between models. 

![Image](/Transferability.png){: .center-image height="50%" width="50%"}


## Training on Adversarial Models
In this experiment, they trained the BiDAF Single model with *ADDSENT* examples and compare the performance with the original training data. Though it seems that after training, the accuracy on *ADDSENT* examples increase, the improvement is actually limited. To prove this, they modified *ADDSENT* in two ways and called it *ADDSENTMOD*. The two differences are:
1. Use different fake answer in step 2. 
2. Add the adversarial sentence in the beginning of the paragraph instead of adding it in the end of the paragraph.
The performances on *ADDSENTMOD* on original model and augmented model are equally bad since the model just rejected the fake answer and ignored the last appended sentence.

![Image](/Training_on_Adversarial_Examples.png){: .center-image height="50%" width="50%"}

## Conclusion
Though the accuracy of the recent models seems to reach human performance, they might not really understand the meaning of passage. With the adversarial examples, the models are still vulnerable. This suggests that people may have to come up with new model training methods to accommodate this problem.

## Reference
All figures and tables are from:
[Adversarial Examples for Evaluating Reading Comprehension Systems](http://aclweb.org/anthology/D17-1214) 

