# Assignment 2: Prompting via Crowdsourcing Strategies

In this assignment, you will **apply decades of research findings in Crowdsourcing instructions to LLM prompting**. Hopefully the assignment will help you:

- Get familiar with the concept of prompting.
- Get familiar with the OpenAI interface.
- Get familiar with crowdsourcing techniques & limitations.
- Explore the feasibility of using LLMs as crowdworkers -- _If the results are interesting, we might use them to compile a research paper with everyone in the class listed as a coauthor!_

## Assignment overview

**Background**: If you think of each large language model call as a crowdworker completing a small task, then naturally we can ask: can multiple LLM modules collectively solve a larger task, if each of them completes a microtask?
This is the basic idea behind our CHI 2022 paper [AI Chains](https://arxiv.org/pdf/2110.01691.pdf). **I highly recommend reading this paper, especially the Introduction, Sec 2.3 (gives you an overview of Crowdsouring workflow vs. LLM workflow), and Sec 3.1 (why LLMs can be used in workflows).**

This framing opens up additional possible explorations of LLM usage. Researchers have explored microtasking/building workflow and pipelines for crowdsourcing tasks for decades, and so we can see if their design pipelines can also be applied to LLMs.

In this assignment, you will:

1. read a crowdsourcing paper in detail,
2. replicate its pipeline by writing multiple prompts that would instruct different LLM modules to complete different subtasks,
3. test the pipeline on some tasks and inputs you selected, and
4. write a reflection on why the pipeline worked or did not work.

## Steps for completing the assignment.

### Pick a Crowdsoucring paper to focus on.

Pick from one of the six papers below and read in detail. You will replicate its baseline when you do prompting in the notebook (see next section).

1. Bernstein, Michael S., et al. ["Soylent: a word processor with a crowd inside."](https://dl.acm.org/doi/pdf/10.1145/1866029.1866078) UIST 2010.
   - Pipeline: "Find-Fix-Verify" in Figure 4
   - Example inputs/outptus in tables
2. Kulkarni, Anand Pramod, Matthew Can, and Bjoern Hartmann. ["Turkomatic: automatic, recursive task and workflow design for mechanical turk."](https://www.aaai.org/ocs/index.php/WS/AAAIW11/paper/viewPaper/3884) AAAI Workshop 2011

   - Pipeline: "Recursive Divide-and-Conquer" in Figure 1/3
   - Example tasks in Table 1

3. Kittur, Aniket, et al. ["Crowdforge: Crowdsourcing complex work."](https://dl.acm.org/doi/pdf/10.1145/2047196.2047202) UIST 2011.
   - Pipeline: "Map-Reduce" in Figure 1/2
   - Example tasks in "Case Study"
4. Chilton, Lydia B., James A. Landay, and Daniel S. Weld. ["Humortools: A microtask workflow for writing news satire."](http://www.cs.columbia.edu/~chilton/web/my_publications/ChiltonHumorTools2016submission.pdf) 2016.
5. Little, Greg, et al. ["Exploring iterative and parallel human computation processes."](https://www.cs.columbia.edu/~chilton/web/my_publications/LittleIterativeHCOMP2010.pdf) SIGKDD workshop. 2010.

   - Pipeline: Iterative & parallel process in Section 3.3
   - Example task: Brainstorming in 4.2

6. Cheng, Justin, et al. ["Break it down: A comparison of macro-and microtasks."](https://hci.stanford.edu/publications/2015/micromacro/micromacro.pdf) CHI 2015.

   - Pipeline: (A bit like iterative in 5)
   - Example tasks: Arithmetic / Sorting in Figure 1.

### Set up the environment and code.

1. **Envoirnment setup.** Similar to Assignment 1, setup the virual Python environment so you can use the same version of Python. Here, I show an example of using [Conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html#before-you-start) (which I highly recommend):

   ```sh
   # create an environment named eval_env, under the version of python 3.7
   conda create --name prompt_env python=3.7
   # start the environment.
   conda activate prompt_env
   ```

2. **Install necessary packages.** This assignment only requires to install OpenAI API and Jupyter Notebook.

   ```sh
   # You should make sure you are in the environment when installing packages.
   conda activate prompt_env

   pip install openai
   pip install notebook
   ```

3. **Start the programming environment**

   ```sh
   # Make sure you are in the right environment
   conda activate eval_env
   # clone this git repo.
   git clone $GIT_REPO_URL
   # Make sure you are in the right folder
   cd $PATH_TO_YOUR_LOCAL_REPO
   # start the jupyter notebook
   jupyter notebook
   ```

4. **Setup OpenAI account and get an API key**. You will create a new OpenAI account, and this will give you $18 credit which should be enough for this small exploration study.

   - Go to [OpenAI](https://openai.com/api/), create a new account.
   - Log into OpenAI, create your [API Key](https://beta.openai.com/account/api-keys) (In Account -> View API Keys -> create new secret key).
   - create a json file `credential.json` in the repo folder, and put the following information there (for safety, this information will not be uploaded to GitHub):

     ```json
     {
       "openai": "[INPUT YOUR APIKEY HERE]"
     }
     ```

   - Read the [tutorial doc](https://beta.openai.com/docs/introduction), and the [pricing page](https://openai.com/api/pricing/) to get a general sense of GPT-3.
   - **Tips for saving credits:** (1) You can use a cheaper model for tuning your prompts (see [pricing](https://openai.com/api/pricing/)) before generating the final responses using the most capable model `text-davinci-003`. (2) [AI21](https://www.ai21.com/studio) provides a model similar to GPT-3 / API service similar to OpenAI, and it involves more free credits for you to try.
   - Please keep track of your granted free credits! **Unfortunately we won't be able to reimburse any additional costs.**

5. **Start to complete the task in the notebook.** Visit `http://localhost:8888/notebooks/A2-Notebook.ipynb` so you can see your notebook. Follow its steps. To help you with, we provide a [toy example solution](http://localhost:8888/notebooks/A2-Notebook-Sample.ipynb) of this assignment.

## Delivery

1. Make sure you have finished all of the steps in the notebook
2. Download the notebook as HTML, via File => Download As => HTML in Jupyter Notebook.
3. Rename the downloaded HTML `A2-Notebook.html` to `A2-Notebook-[AndrewID].html` (e.g. `A2-Notebook-janedoe.html`)
4. Go back to Convas, and submit both `A2-Notebook-[AndrewID].html`.

## Grading:

If you find the assignment difficult, there are some ways to earn partial credits, as shown below.

The base grade will be up to 80, based on how you attempted the task:

- **0 point** if no submission.
- **30 points if you only describe your task and strategies without actual prompting**. You will only need to fill in the writeup sessions in the notebook. For the result part, you should write how you _envision_ the model to perform.
- **60 points if you modified the pipeline in the Sample notebook** -- i.e., if you created a different task, and showed that a pipeline similar to the Sample pipeline also works for your new task.
- **70 points if you wrote a pipeline that's applicable to 1 input** (i.e., you made it work for a particular input).
- **80 points if you wrote a pipeline that's applicable to 3 inputs** (i.e., you showed some generalizability).

**The remaining 20 point will be based on peer review results:**:

- Three students will read your notebook and rate your prompting effort on a 1-5 scale. This rating will be about whether you've put in enough effort replicating and improving the selected crowdsourcing pipeline for LLM prompting (and, relatedly, how thoughtful was your writeup), not on the final prompt efficiency.
- Your score depends on the relative ranking of averaged your test score usefulness. you wil get {20, 15, 10, 5} scores if your averaged score is ranked top {25%, 50%, 75%, 100%} percent respectively.

## Questions?

Please post your questions on Canvas.
