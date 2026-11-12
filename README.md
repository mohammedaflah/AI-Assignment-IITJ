# AI-Assignment-IITJ
AI Assignment - Logical Entailment Reasoning Agent (P26AI0009)Sub

What's in this folder
----------------------
  reasoning_agent.ipynb   The complete system: parsing, reasoning engine,
                          evaluation and submission writer, in one notebook.
                          All cells are already executed, so the printed
                          accuracy, the inference time and the reasoning
                          traces can be checked without re-running anything.
  report.pdf              Short report: representation, reasoning algorithm,
                          experiments on the public datasets, reasoning traces.
  submission.csv          id + predicted label (True/False) for every query of
                          the evaluation run (2,003 queries).
  README.txt              This file.

How to run
----------
  1. pip install datasets huggingface_hub pandas   (standard libraries only)
  2. Open reasoning_agent.ipynb and run all cells (Kernel -> Restart & Run All).

  The first run downloads RuleTaker and FOLIO from the Hugging Face Hub and
  CounterBench from its dataset repository (internet needed once; afterwards
  everything is served from the local HF cache). Running all cells takes a
  few seconds and regenerates submission.csv next to the notebook.

notebook prints
----
  - accuracy per dataset and overall:
        ruletaker      800/800   = 100.0%
        counterbench   960/1000  =  96.0%
        folio          132/203   =  65.0%
        TOTAL          1892/2003 =  94.5%
  - total inference time (about 0.5 s for all 2,003 queries)

submission.csv
----
  Columns: id, prediction.  The ids encode the source dataset and the row
  index inside the notebook's evaluation sample (fixed seed 42):
      ruletaker_0000 ... ruletaker_0799   (RuleTaker test split sample)
      counterbench_0000 ... counterbench_0999
      folio_0000 ... folio_0202           (full validation set)
  If the course provides its own test file, only the "load the datasets"
  cell needs to be pointed at it, the engine and the writer stay the same.

The system 
---------------------------
  Natural language -> template-based parser -> facts + rules -> forward
  chaining (breadth-first, to a fix point) -> True / False.
  The parser turns sentences like "If someone is young then they are smart"
  into rules young(x) -> smart(x). The engine derives everything derivable,
  records which rule produced which fact (that is what the traces show), and
  the final True/False decision is always made by the engine itself.
