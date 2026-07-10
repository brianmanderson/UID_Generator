# UID_Generator

A small Python utility for handing out unique integer IDs from a persistent, file-backed generator. Useful when code needs a stable stream of non-colliding IDs (for example, placeholder DICOM/structure identifiers) that must survive across separate runs.

## How it works

`Generators.py` defines a single `Generator` class that holds a counter (`UID_Value`) and a file path. Each call to `return_uid()` decrements the counter and returns it, so issued IDs are unique negative integers. `return_uid_and_save()` does the same and then pickles the generator's state to disk, so the next process picks up where the last one left off.

The module-level helper `return_generator(generator_path)` loads an existing generator from a `.pkl` file if one exists at the path, or creates a fresh one otherwise (appending the `.pkl` extension if missing).

## Requirements

- Python 3
- `PickleObjects` (provides `Pickler.load_obj` / `save_obj`; see brianmanderson/PickleObjects)

## Usage

```python
from UID_Generator.Generators import return_generator

gen = return_generator("my_uids")   # loads my_uids.pkl or creates it
uid = gen.return_uid_and_save()     # returns the next id and persists state
```

Small single-purpose utility, not actively maintained (last updated 2023).
