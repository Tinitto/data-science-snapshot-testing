# Snapshot Testing in Data Science
An Introduction to Snapshot testing in Data Science Pipelines and Systems

## What Snapshot Testing is

It is usually quite hard to test data analytic systems. This is due to the sheer number of different calculations that have to be unit tested.

As a work-around, I have borrowed the concept of snapshot testing from the front-end world and used it in a few data analytic system projects. I think I like it.

Snapshot Testing is a concept popularized by [jest](https://jestjs.io/), a Facebook-created testing framwork for JavaScript.

In snapshot testing, the first run of the test saves the state of the final result (or front end, the html markup while for data science, the actual numbers) and compares the results of any consequent test runs against these saved snapshots.

To allow for innovation (and correction), snapshot tests allow a user to explicitly replace these snapshots such that in case it has been decided that the final result should be a given value different from the original, the tests don't keep failing.

It is basically automating something developers (and Quality Assurance professionals) do. They always check whether the new changes have changed ONLY what was meant to be changed, leaving other things as they should be.

### Process of Snapshot testing in Data Science Architecture

We will be using python to show how a snapshot test for a data science project can be done.
To save our snapshots, we will use pickle files.

The core code for the snapshot test (which can be put in any test) is:

```python

 import os
 import pickle

 # ...... more code
 # ......

 results = some_calculation()

 expected_result_file_path = 'path_to_the_snapshot_file_of_choice'
 # the snapshot file should not exist at the beginning
 
 if not os.path.isfile(expected_result_file_path):
    # create the snapshot on the first run or anytime it is deleted manually
    with open(expected_result_file_path, 'wb') as expected_results_file:
        pickle.dump(results, expected_results_file)

with open(expected_result_file_path, 'rb') as expected_results_file:
    # the actual test to compare against snapshot
    expected_results = pickle.loads(expected_results_file)

    # here is an assertion basing on a unittest test
    self.assertEqual(results, expected_results)
```

### Suggestions

- Put all the snapshots in one folder appropriately labelled
- Put any necessary mocked input data in files in another folder appropriately labelled

### Other Considerations

- There is a possibility of also testing the time spent calculating as opposed to the accuracy of the final results.
This too can be saved in a pickle file of say performance

- Actually a whole range of performance parameters can be snapshot tested, including memory usage, time spent etc.

### ToDo
- Create a sample project to showcase snapshot testing in data science
