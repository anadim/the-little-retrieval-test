# The Little Retrieval Test (LRT): Testing the ability of LLMs to retrieve data in ordered and randomly shuffled contexts



This repository showcases an experiment conducted using the Anthropic API to assess a language model's ability to recall specific lines from a generated text file, in both orderly and shuffled contexts.

This experiment aims to investigate the recall abilities of language models, particularly in the context of long text sequences. The ability of a model to accurately recall and utilize information from distant parts of the input text is crucial for many tasks, especially those that involve understanding and manipulating complex contexts.

At the core of this experiment, we're interested in assessing how well the models can perform "goto" operations, where they need to retrieve specific pieces of information from earlier parts of a long text. This is reflective of real-world tasks where a model might need to refer back to earlier parts of a conversation or document to answer a question or make an informed decision.

## Experiment Design

To test this, we generate long sequences of text where each line contains a piece of information ("REGISTER_CONTENT") and a unique identifier. At a random point in the text, we insert a "goto" command that instructs the model to recall the information from a specific line.

The lines are not necessarily ordered, and the "goto" command is not necessarily at the start or end of the text. This is to ensure that the model cannot simply attend to the first or last token in the text to find the "goto" command, which might be possible if we always placed the command at the start or end.

We also introduce a variation of the experiment where the lines are shuffled. We hypothesize that the ordered lines might provide a stronger signal to the attention mechanism, guiding it towards the relevant information. By shuffling the lines, we can make the task more challenging and potentially reveal more about the inner workings of the model's attention mechanism.


We initiate the experiment by generating a text file with each line following the format:

```
line {i}: REGISTER_CONTENT is <{random.randint(1, 10000)}>
```

At a random point in the file, we insert a line that says:

```
[EXECUTE THIS]: Go to line {i} and report only REGISTER_CONTENT, without any context or additional text, just the number, then EXIT
```

The language model is then prompted with the text file's content, and its output is compared to the expected REGISTER_CONTENT value.

Here is how a sample prompt looks like

```
Testing Long Context

line 1: REGISTER_CONTENT is <2156>
line 2: REGISTER_CONTENT is <9805>
[EXECUTE THIS]: Go to line 5 and report only REGISTER_CONTENT, without any context or additional text, just the number, then EXIT
line 3: REGISTER_CONTENT is <6668>
line 4: REGISTER_CONTENT is <1432>
line 5: REGISTER_CONTENT is <6727>
line 6: REGISTER_CONTENT is <3936>
line 7: REGISTER_CONTENT is <1805>
line 8: REGISTER_CONTENT is <431>
line 9: REGISTER_CONTENT is <1720>
line 10: REGISTER_CONTENT is <6794>

```

## Testing in a Shuffled Context

To evaluate the model's ability to recall content in a shuffled context, we alter the text file by randomly shuffling the lines. The structure of each line and the execute command remains the same. Below is an example of a shuffled text file:

```
Testing Long Context

line 7: REGISTER_CONTENT is <53914>
line 3: REGISTER_CONTENT is <21221>
line 1: REGISTER_CONTENT is <72318>
line 9: REGISTER_CONTENT is <32901>
line 4: REGISTER_CONTENT is <63509>
line 8: REGISTER_CONTENT is <34620>
line 6: REGISTER_CONTENT is <90474>
[EXECUTE THIS]: Go to line 9 and report only REGISTER_CONTENT, without any context or additional text, just the number, then EXIT
line 10: REGISTER_CONTENT is <54099>
line 2: REGISTER_CONTENT is <18859>
line 5: REGISTER_CONTENT is <36058>
```

## Experiment Procedure and Results

For each test, the model is provided with the generated text file's content as a prompt. The model's output is then compared to the expected REGISTER_CONTENT value. If the model's output matches the expected value, the test is considered successful. If the model's output does not match the expected value, the line in the prompt containing the incorrect output value provided by the model is returned for further analysis.

The tests are conducted across different values of n, the number of lines in the text file. These tests are performed several times for each value of n to ensure statistical validity. The success rate, defined as the percentage of correct matches over the total number of tests, is then calculated for each model and each value of n.




