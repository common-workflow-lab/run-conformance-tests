# run-conformance-tests
It is a custom Github Action for running CWL conformance tests with a given CWL runner.

## Example

```yaml
jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      # setup CWL runner
      - uses: actions/checkout@v4
      - name: Setup python for cwltool
        uses: actions/setup-python@v5
        with:
          python-version: '3.12.x'
      - name: Install cwltool
        run: pip install cwltool
      - uses: actions/setup-node@v4
        with:
          node-version: '20.x'

      - name: Run conformance tests
        id: run-conformance
        uses: common-workflow-lab/run-conformance-tests@v1
        with:
          cwlVersion: v1.0
          runner: cwltool
          timeout: 30
          skip-python-install: true
     
      - name: Do something with badgedir
        run: |
          echo ${{ steps.run-conformance.outputs.badgedir }}
```

## Input parameters

| Parameters | Required | Default | Description |
|---|---|---|---|
| `cwlVersion` | true | - | target CWL version |
| `runner` | true | - | full path to CWL runner to be tested |
| `timeout` | false | 30 | timeout in seconds |
| `tags` | false | "" | tags to be tested (e.g., "docker,required") |
| `n` | false | "" | specify tests by their numbers (e.g., "1-5,10,13") |
| `s` | false | "" | specify tests by their names (e.g., "test_a,test_b") |
| `extra` | false | "" | extra arguments for the given CWL runner |
| `skip-python-install` | false | false | skip installing python interpreter |

The `skip-python-install` parameter is useful when CWL runner requires specific version of Python.

## Output parameters

| Parameters | Description |
|---|---|
| `badgedir` | directory name that stores conformance badges |
| `result` | file name that stores the result of conformance tests in JUnit XMl format |
