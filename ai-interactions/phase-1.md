# Phase I AI Interaction Record

## 1. Tool and Setup

We used Claude (Anthropic) as a chat assistant for planning, drafting code, explaining domain concepts and reading our results. We ran all code ourselves in Colab, and fed the outputs (tables, plots, error messages) back into the chat. The AI never had direct access to the notebook or the data.

## 2. Important Prompts

| Stage | Prompt (shortened) | What it led to |
|---|---|---|
| Scoping | Shared the assignment slides and asked “I don’t really understand what we need to do here, any ideas” | Framing the project around shortcut learning in chest X-ray models, using a pretrained model plus metadata to split results. |
| Registration | “We need to describe what we are going to do, both in phase 1 and 2” | Text for the Phase I and II plan; flagged that CheXpert was in the model’s training data. |
| Concept check | “Are those also inputs to the model?” | Clarified that the metadata is only used to group results, never as model input. |
| Analysis | “Not very good” (after low AUC values) | Diagnostic checks: valid vs train labels, frontal vs lateral, input range, plotted example image. |
| Interpretation | “What’s the takeaway? We wanted to see whether the model uses shortcuts” | The healthy-patients-only control, to separate shortcuts from co-occurring disease. |
| Perturbation | Results of the border masking | Comparing the AP vs PA gap before and after, instead of the absolute drop. |
| Feedback | Professor’s feedback on readability | Ordered matrix, age by view blocks, image grids, score correlation view. |

## 3. How AI Suggestions Affected Our Analysis

### Main hypothesis

The AI suggested testing whether the model reacts to scan type (AP/PA), devices and demographics instead of the pathology. That became our research question.

### Fixed-FPR thresholds

When the 0.5 cutoff gave false positive rates of 80 to 97%, the AI suggested one threshold per pathology at a fixed 20% FPR. Without that, the subgroup comparison would not have been readable.

### Control experiment

The healthy-only analysis came from the AI pointing out that AP patients are sicker, so false alarms could come from real, similar-looking diseases. That changed our conclusion from “the model uses AP as a shortcut” to “most of the effect is co-occurring disease, but a residual AP effect remains”.

### Reading the masking test

The AI warned that the drop in AP false positives after masking was just the whole score distribution shifting under a fixed threshold, and suggested comparing the AP vs PA gap instead. Because of that we concluded that text markers are not the cue.

## 4. Accepted, Modified and Rejected Suggestions

| Suggestion | Decision | Why |
|---|---|---|
| Shortcut framing, healthy-only control, Fisher and Mann-Whitney tests | Accepted | Directly answered our research question. |
| Fixed 20% FPR threshold | Accepted with caveat | Needed for comparison, but the 20% is arbitrary. We report it as a reference point and confirm the main result without a threshold. |
| Border mask of 15% | Modified to 7.5% | 15% cut into the lungs when we plotted it. |
| Analysing all scans | Modified to frontal only | Our idea after laterals showed clearly lower AUC; the AI agreed. |
| Fine-tuning the model on lateral views | Rejected | Would change the model under analysis, outside the scope and the timeline. |
| Delta heatmap as the main overview | Refined | Professor asked for more readable, ordered views; we switched to absolute FPR with grouped rows. |

## 5. Failure Patterns We Observed

### 5.1. Wrong assumptions about the environment

The AI’s file path code assumed a folder layout that didn’t match the Kaggle copy, and failed again after Colab moved the dataset to a different location. The code was reasonable in theory but untested against our setup.

### 5.2. Wrong assumptions about the data

The first pipeline treated blank CheXpert labels as missing instead of negative. Our negatives were then mostly sick patients, and the false positive rates came out at 80 to 97%. We had already started interpreting a heatmap built on these numbers before the AI flagged them as implausible.

### 5.3. Trusting library defaults

The AI first described the model’s outputs as calibrated so that 0.5 is a meaningful threshold. On our data, 0.5 flagged almost everything.

### 5.4. Fixes that don’t survive the notebook

A label fix was placed in a cell before the cell that rebuilds the results table, so it was silently overwritten. The AI couldn’t see the notebook’s cell order or state.

### 5.5. Interpreting too early

An early reading (“view and age are separate effects”) was based on the bad threshold and had to be revised once the threshold was fixed. The AI produced convincing explanations even for buggy numbers.

### 5.6. Documentation drift

AI-drafted notebook explanations described a frontal vs lateral comparison that no longer existed in the pipeline after we switched to frontal only. We noticed this ourselves.

### 5.7. Technically correct but hard-to-read visualizations

The first suggestions (an unordered delta heatmap, raw number tables) were fine for us but not for an audience, which matched the professor’s feedback.

### What worked well

Suggesting control experiments and alternative explanations, explaining medical background (AP/PA projection, support devices, why the heart looks bigger on AP), choosing statistical tests, and quickly turning ideas into working code.

## 6. Emerging Best Practices

1. **Show the AI real outputs, not descriptions.** Most useful corrections came from pasting actual tables and plots. The label bug was found because the numbers were visibly implausible.

2. **Run sanity checks before interpreting anything:** label value counts, AUC, input range and a plotted example image.

3. **Treat AI code as untested.** Check paths and data assumptions, and add small checks (for example, printing label counts) after each step.

4. **Restart and run the whole notebook after AI fixes**, so patches can’t be silently overwritten by later cells.

5. **Ask for alternative explanations and controls, not just interpretations.** The AI is most useful when asked “what else could explain this?”.

6. **Question every parameter the AI picks** (thresholds, mask size), and test whether the result depends on it.

7. **Check AI-written text against the current state of the code** before putting it in the report.

8. **Log prompts as you go.** Reconstructing them afterwards is much harder.



