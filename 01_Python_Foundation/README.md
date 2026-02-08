# Python for Azure Data Engineering

This folder contains my journey learning Python from scratch, specifically tailored for Azure Data Engineering tasks.
The goal was to move from basic scripting to building robust ETL pipelines using OOP and Pandas.

## Modules

| Nb | Topic | Key Concepts |
| :--- | :--- | :--- |
| **01** | [Basics](01_Basics_Variables_Types.ipynb) | Variables, f-strings, basic types |
| **02** | [Control Flow](02_Control_Flow.ipynb) | `if/else`, `for` loops, range |
| **03** | [Functions](03_Functions.ipynb) | `def`, return values, DRY principle |
| **04** | [Collections](04_Lists_and_Dicts.ipynb) | Lists, Dictionaries (JSON), Sets |
| **05** | [File I/O](05_File_IO.ipynb) | `with open`, `try/except`, Error Handling |
| **06** | [OOP](06_OOP.ipynb) | Classes, `__init__`, Inheritance |
| **07** | [NumPy](07_NumPy.ipynb) | Arrays, Vectorization, Masking |
| **08** | [Pandas](08_Pandas.ipynb) | DataFrames, Filtering, GroupBy |
| **09** | [Capstone](09_Capstone_ETL.ipynb) | **End-to-End ETL Pipeline** |

## Capstone Project
The final module (`09_Capstone_ETL.ipynb`) simulates a real-world scenario:
1.  Extracts raw data from CSV (handling errors).
2.  Transforms data (cleaning, calculations) using Pandas.
3.  Loads aggregated results to JSON.
4.  Orchestrated via a custom `ETLPipeline` class.
