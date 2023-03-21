---
author_name: Elena Grigorova
author_profile:  
title: Automation Pilot
description: Learn the Markdown syntax for use with the v2 parser
keywords: tutorial
auto_validation: false
time: 10
tags: [ tutorial>beginner, type>tutorial ]
primary_tag: type>tutorial
parser: v2
---

# Examples of Markdown Syntax for v2 Parser
<!-- description --> Learn the Markdown syntax for use with the v2 parser


## You will learn
- Learning objective #1
- Learning objective #2
- Learning objective #3

## Prerequisites
- Prerequisite #1
- Prerequisite #2
- Prerequisite #3


## Intro
1. Remove the "## Details" title.

2. Notice below  that there is no longer a need to use the **"ACCORDION BEGIN"** and **"ACCORDION END"** statements.

3. Just use three hashtags "###" followed by the name of the step to identify a step.

### Revise the Meta Data
- The meta data section is still separated by three dashes "---" before and after.
- You must add the following meta data attribute, which will call the v2 parser "parser: v2"
- If you do not add the title and description in the meta data, it will be displayed by adding the following after the meta data list.

    ```
    # This is the Title
    <!-- description --> This is the description
    ```


### Set up options within a step

[OPTION BEGIN [Option Number 1]]
This is the content for the first option.
[OPTION END]

[OPTION BEGIN [Option Number 2]]
This is the content for the second option.
[OPTION END]

[OPTION BEGIN [Option Number 3]]
This is the content for the third option.
[OPTION END]


### Set up a table in a step
Following is an example of a table.

**Markdown written as...**
```
| Tables|Are|Cool
|--- | :---: | ---:
|col 3 is | right-alinged | $1600 
|col 2 is | centered | $12
|zebra stripes | are neat | $1
```

**Renders as...**

| Tables|Are|Cool
| --- | :---: | ---:
| col 3 is | right-alinged | $1600 
| col 2 is | centered | $12
| zebra stripes | are neat | $1


**Markdown written as...**
```
| Markdown | Less | Pretty
| --- | --- | ---
| *Still* | `renders` | **nicely*
```

**Renders as...**

| Markdown | Less | Pretty
| --- | --- | ---
| *Still* | `renders` | *nicely*


### Set up images in different styles

There is no change in the syntax for an image without a border.

Here is the image with no border:

![Here is the image with no border](Example_Image.png)


To add a border, remove the extra "!" and enter `<!-- border -->`  before the standard image syntax.
    
 Here is the image with a border:

<!-- border --> ![Here is the image with a border](Example_Image.png)

To change the size of an image, you can add "size:XXXpx" to the descriptor like this `<!border; size:540px -->`
    
Here is the image with a specified size and border: 

<!-- border; size:540px --> ![Here is the image with a border and specified size](Example_Image.png)


### Revise how you add validations

NB: It does not matter in which order multiple validation rules are entered in the `rules.vr` file.

[OPTION BEGIN [Rules Syntax Image]]
The syntax `[VALIDATE_#]` and `[DONE]` should be removed.

In the `rules.vr` file, indicate the place of the question before and after the validation detail.  If you want a validation to appear on the third step, add it like this:

![Sample Validation Text](Sample_Validation.PNG)
[OPTION END]

[OPTION BEGIN [Rules Syntax in code block]]
```
[VALIDATE_3]
###Rule
single-choice

###Question
True or False - Pipes are required on the outside of table content.

###Match
[ ] True
[x] False

[VALIDATE_3]
```
[OPTION END]