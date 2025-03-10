# The Interface

All interactions with the Exercism website are handled automatically. Test runners have the single responsibility of taking a solution, running all tests and returning a standardised output.

## Execution

- A test runner should provide an executable script. You can find more information in the [docker.md](https://github.com/exercism/automated-mentoring-support/blob/master/docs/docker.md) file.
- The script will receive two parameters:
  - The slug of the exercise (e.g. `two-fer`).
  - A path to a directory containing the submitted file(s) (with a trailing slash).
- The script must write a file to that directory named `results.json`

## Output format

The `results.json` file should be structured as followed:

```json
{
  "status": "fail",
  "message": null,
  "tests": [
    {
      "name": "Test that the thing works",
      "status": "fail",
      "message": "Expected 42 but got 123123"
    }
  ]
}
```

The following overall statuses are valid:
- `pass`: All tests passed
- `fail`: At least one test failed
- `error`: To be used when the tests did not run correctly

The top level `message` key should provide a message when tests fail to execute due to an error/exception. For example, in Ruby, we provide the error and stack trace if an exception occurs while running the tests. It should otherwise be `null`.

The following per-test statuses are valid:
- `pass`: The test passed
- `fail`: The test failed
- `error`: The test errored


The `message` key is optional and can be used to display human-readable error messages. Presume that whatever is written here will be displayed to the student.

**We will provide a Ruby script that converts JUnit to this JSON output, which you can add as a final step of your Docker images, if you prefer to use an existing format.**

## Debugging

The contents of `stdout` and `stderr` from each run will be stored in files that can be viewed later.

You may write an `results.out` file that contains debugging information you want to later view.

At a later date, we will provide an interface for you to download these files along with the submitted files and `results.json`.
