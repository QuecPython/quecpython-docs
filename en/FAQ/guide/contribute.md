# Document Contribution

The contribution to QuecPython-FAQ project would be appreciated, such as fixing bugs, adding documentation, etc. Additionally, it is recommended to submit to [Github Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).

## Submission process

This section is a brief introduction on `adding new question` and `modifying question`. Please click the link to view the specific requirements.

There is no specific introduction to gi-related operations, please query [Git related tutorials](https://git-scm.com/book/en/v2) for details.

### Add questions

1.  Create a [Create new branch](#Create-new-branch) locally and adhere to the [Branch Name Standard](#Branch-Name-Standard);

2.  Find the `*.md` file corresponding to the question type locally or in the web IDE, and add a new question according to the template format;

3.  After editing, open the preview interface to check whether the display result meets expectations. You can use [Local Compilation Environment](#Local-Compilation-Environment) to compile the document and check whether the generated web page is satisfactory;

4. Adhere to [Submission information specifications](#Submission-information-specifications), push to github and hand in Pull Requests;

5. If above expectations are met, then [Submit merge request](#Submit-merge-request);

6. When all discussions on the document are resolved and the PR is successfully submitted, the process of adding new issues is completed.

### Modify questions

1.  Create a [Create new branch](#Create-new-branch) locally and adhere to the [Branch Name Standard](#Branch-Name-Standard);
2.  Find the `*.md` file corresponding to the question type locally or in the web IDE, and add a new question according to the template format;

3.  After editing, open the preview interface to check whether the display result meets expectations. You can use [Local Compilation Environment](#Local-Compilation-Environment) to compile the document and check whether the generated web page is satisfactory;

4.  Adhere to [Submission information specifications](#Submission-information-specifications), push to github and hand in Pull Requests;

5.  If above expectations are met, then [Submit merge request](#Submit-merge-request);

6.  When all discussions on the document are resolved and the PR is successfully submitted, the process of adding new issues is completed.

## Create new branch

New branches are all based on the **main branch**; when operating, please pay attention to whether the current branch is to be merged as your wish.

Operation example:

```bash
git status # View current branch
git checkout -b add/API-reference_machine_pin # Used for adding new problem "API-reference_machine pin"
```

## Branch Name Standard

*   Add new problems: `add/API-reference_{q&a}`.
*   `{q&a}` : Use English brief for filenames, such as adding new problem `API-reference_machine pin`, Branch name: `add/API-reference_machine_pin`.
*   Modify the problems: `add/API-reference_{q&a}`.
*   `{q&a}`: Use English brief for filename, such as modifying the problem `API-reference_machine pin`. Branch name: `add/API-reference_machine_pin`.

## Question edition Specifications

Please add or update Q\&A according to following format specifications:

**General rules**：

*   When adding a new Q\&A, remember to make the question a tertiary heading and add ordinal number.

*   Example: `### 1. What is the downlink bandwidth in CAT1? `

**Question format**：

*   The problem must be described concisely and clearly, such as:
    *   QPYcom fails to burn firmware. **(UNCLEAR)**
    *   QPYcom fails to burn firmware, how to solve it? **(CLEAR)**

*   Questions should be as short as possible. Normally, please refine the main questions as titles and describe the background and details in the text of the answer if the description is excessive.

**Answer format**：

*   If you need to quote codes in the text, please separate it from the text from code syntax.

*   If the answer to a question only contains one sentence, it can be written in a normal paragraph instead of listing.

*   When you need to list multiple items or sort them in order, a list will be available:

    *   Number list: There is a certain order (such as operation steps), or an item in the list needs to be referenced later.

    *   Bulleted list: No particular order.

*   There must be introductory text before the list to illustrate the meaning or purpose of the following list, and end with a colon ":".

*   If the two items are mutually selective, they should be listed in a bulleted list (not a numerical list), and the relationship should be explained in the preface before the paragraph.

*   In the main text (no matter a list or a paragraph), there must be two spaces before each line.

*   If an entry needs to be followed by a comment or explanatory text, the comment should be indented to make it a sub-entry.

## Local Compilation Environment

*   Windows 10 and above, Ubuntu or Debian system can be applied in testing verification environment and the Python environment used to configure will be `3.6` and above.
*   It is recommended to use the virtual environment of Python or docker environment.

```bash
# Install python3 and virtual environment (for Windows systems, just double-click the python3.exe)

sudo apt-get install python3 python3-pip python3-venv

# Create a virtual environment (the path under Windows system is path\to\.pyenv3, path/to, which refers to the real path)

python3 -m venv path/to/.pyenv3

# Activate the virtual environment (It is necessary for Windows systems to execute path\to\.pyenv3\Scripts\activate.bat in the working directory)

source path/to/.pyenv3/bin/activate

# Install the teedoc

pip3 install teedoc

# Get the source file

git clone https://gitee.com/qpy-doc-center/teedoc_with_qpydoc.git

# Install plug-in

cd teedoc_with_qpydoc
teedoc install

# Compile and start the service (append the -s option to start the service)
./build.sh -s
```

## Submission information specifications



Add submission information in branch to describe adding/modifying/removing issue functionalities. Each submission will be endowed with one message, for example:

```text
API-reference_machine_pin: add 'value' method for machine.Pin class.

1. add 'value' method for machine.Pin class.
```

The first line of the submitted message should be something like "Issue Category: Add/Fix/Remove/Change Content". The first line begins with the name of the file to be modified and submitted. For example:

`API-reference_machine_pin: add 'value' method for machine.Pin class.`

For adding more details related to this submission, please place it in the submission message after the first line.

A good git message will explain why it is needed to modify. Therefore, those who read corresponding log can learn about the development of the project. Writing a good commit message may seem like a waste of time now, but it will be useful to you and your colleagues (and to our customers too) in the future when trying to understand why something changed.

## Submit merge request

Once the modifications are accomplished, it is available to submit the branch for the first time. If more branches are needed, please submit continuously. Finally, it is needed to submit merge request.

We will merge the branch into the main branch via github merge request,see following steps:

*   Push your branch to the github repository;

*   Switch to teedoc\_with\_qpydoc and click "New pull request";

*   Select the branch created just now to merge and fill in the "Merge Request Specifications".

### Specifications for submitting merge requests

*   Title  Requirement:&#x20;

    `add: A brief description`

*   Description  Requirement:

     ` Describe the merge point by point .`

*    Example：

    Title:

    `API-reference_machine_pin: add 'value' method for machine.Pin class.`

    Description:

    `1. add 'value' method for machine.Pin class.`

