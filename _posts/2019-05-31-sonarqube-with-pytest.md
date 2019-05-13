---
layout: post
title:  Sonarqube with pytest
date:   2019-05-31
categories: ["Python","CI"]
---

Use pytest to generate a coverage report in coverage.xml and enable Sonarqube to read the report.

**Ignoring directory/ files**[^config]

In `.coveragerc`:

``` txt
[run]
omit = tests/*
```

Generate your coverage report called `coverage.xml`:

``` bash
pytest --cov-config=.coveragerc
       --cov-report xml:path/to/coverage.xml
       --cov=myproj
       myproj/tests/
```

`--cov-report xml` generate coverage report in xml
`--cov-report xml:/path/to/store/coverage.xml`

Both approaches mentioned below work[^coverage-show].

**Approach 1** using pytest and specify the coverage.xml path**

``` bash
pytest --cov-config=.coveragerc
       --cov-report xml:path/to/coverage.xml
       --cov=myproj
       myproj/tests/
```

**Approach 2** using `coverage xml -i to format for SonarQube`

``` bash
pytest --cov-config=.coveragerc
       --cov-report xml
       --cov=myproj
       myproj/tests/
```

``` bash
coverage xml -i
```

**sonar-project.properties**[^report-path]

To get SonarQube to the read the coverage report include the following in the properties file.

``` yaml
sonar.python.coverage.report-path=path/to/coverage.xml
```

[^config]: [https://pytest-cov.readthedocs.io/en/latest/config.html](https://pytest-cov.readthedocs.io/en/latest/config.html)
[^report-path]: [https://docs.sonarqube.org/display/PLUG/Python+Coverage+Results+Import](https://docs.sonarqube.org/display/PLUG/Python+Coverage+Results+Import)
[^coverage-show]: [https://stackoverflow.com/questions/12844451/test-test-coverage-with-python-in-sonar-not-showing-up](https://stackoverflow.com/questions/12844451/test-test-coverage-with-python-in-sonar-not-showing-up)
