# Summary of Adversarial Examples for Evaluating Reaing Comprehension Systems
 by Robin Jia and Percy Liang

## Abstrast
In this paper, the authors proposed adversarial evaluation on Stanford Question Answering Datset (SQuAD). Without confusing humans, the generated sentance distrated the models and their average F1 score dropped from 75% to 36%. This indicates that nowadays
the reading comprehension system techniques remain a large gap of improvement.
## 
![Image](/1.png)

## SQuAD
Stanford Question Answering Datset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles. Each question is refer to a paragraph and the answer is a span in the paragraph.

## Adversarial Examples
![Image](/examples.png){: .center-image }
Images from Szegedy et al. (2014).

As we can see in the above table, adding adversaries in image classifications makes the model consider two images are diffirent while the perturbations do not change. For reading comprehension tasks, adversaries causes the model to ignore the semantics changes. 

## Concatenative Adversaries
In this paper, the authors did not use paraphrasing as adversaries, since it is challenging to edit sentence without changing its meaning. Instead, they use concatentive adversaries. The form is listed below:
```markdown
(p + s, q, a)
```
, where p is a paragraph, s is a additional sentence, q is the question and a is the answer.

Next they propose two concatenative adversaries and two of their variants. 

### ADDSENT
*ADDSENT* generates a sentence that is similar to the question but does not condradict the answer. There are four steps in *ADDSENT*.
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
    
### ADDONESENT
*ADDONESENT* adds a random human-approve sentence. It does not require any queries to the model. 

### ADDANY
*ADDANY* generates a sentence `s` containing `d` words, `s = w1w2...wd`.
1. Initailize `w1w2...wd`. Randomly choose words from common English words. 
2. Switch `wi` with word `x`. word `x` is in candidate words `W` where `W = union of 20 sampled common words and words in q`.
3. Upade `wi` to minimze the F1 score of the model's output.

### ADDCOMMON

## Experiments


pertubations 

## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/mandyfu84/CS269_PaperSummary/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/mandyfu84/CS269_PaperSummary/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
