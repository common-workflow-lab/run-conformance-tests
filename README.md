# run-conformance-tests
It is a custom Github Action for running CWL conformance tests with a given CWL runner.

## Example

```yaml
jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      # setup CWL runner
      - uses: actions/checkout@v2
      - name: Setup python for cwltool
        uses: actions/setup-python@v2
        with:
          python-version: '3.9.x'
      - name: Install cwltool
        run: pip install cwltool
      - uses: actions/setup-node@v2
        with:
          node-version: '14.x'

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
| `skip-python-install` | false | false | skip installing python interpreter |

The `skip-python-install` parameter is useful when CWL runner requires specific version of Python.

## Output parameters

| Parameters | Description |
|---|---|
| `badgedir` | directory name that stores conformance badges |
| `result` | file name that stores the result of conformance tests in JUnit XMl format |
