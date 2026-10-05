# POC: Python CI Checks | Unit Testing

## Author Information

| Author         | Created On | Version | Last Updated By | Last Edited On | Pre Reviewer | L0 Reviewer            | L1 Reviewer | L2 Reviewer        |
| -------------- | ---------- | ------- | --------------- | -------------- | ------------ | ---------------------- | ----------- | ------------------ |
| Vikas Badliwal | 28-09-2026 | 1.0     | Vikas Badliwal  | 28-09-2026     | Team         | Deepak kushwaha/Ayushi | Mohit Kumar | Mahesh kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Pre-requisites](#2-pre-requisites)
3. [Step-by-Step Setup Guide](#3-step-by-step-setup-guide)
   - [3.1 Verify Application Structure](#31-verify-application-structure)
   - [3.2 Install Dependencies](#32-install-dependencies)
   - [3.3 Configure Unit Tests and Mocks](#33-configure-unit-tests-and-mocks)
   - [3.4 Execute Unit Tests](#34-execute-unit-tests)
   - [3.5 Generate and Analyze Code Coverage](#35-generate-and-analyze-code-coverage)
4. [Conclusion](#4-conclusion)
5. [Contact Information](#5-contact-information)
6. [References](#6-references)

---

# 1. Introduction

This Proof of Concept (POC) demonstrates Unit Testing for Python microservices (Attendance & Notification) using Pytest, Pytest-cov, and Pytest-mock.
It covers test creation, dependency mocking, test execution, code coverage analysis, and CI gate verification.

---

# 2. Pre-requisites

The following components are required to perform Python unit testing:

| Requirement               | Purpose                                                                           |
| ------------------------- | --------------------------------------------------------------------------------- |
| **Python 3.11+**    | Programming runtime required to execute application code and test scripts.        |
| **pip**             | Package manager used to install testing frameworks and plugins.                   |
| **Python Services** | Codebase containing Attendance and Notification business logic modules.           |
| **Pytest**          | Modern test framework used to discover and execute unit test cases.               |
| **Pytest-cov**      | Coverage plugin based on`coverage.py` used to track statement and line metrics. |
| **Pytest-mock**     | Mocking plugin based on`unittest.mock` used to safely isolate third-party APIs. |

---

# 3. Step-by-Step Setup Guide

### 3.1 Verify Application Structure

Navigate to the Python project directory:

```bash
cd /workspace/python-services
```

Verify that the target service files exist:

```bash
ls -l services/attendance_service.py services/notification_service.py
```

Verify the test configuration file `pytest.ini`:

```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_functions = test_*
addopts = -ra -q
```

Because Pytest auto-discovers test files starting with `test_`, zero manual registration is needed.

<img width="1902" height="951" alt="image" src="https://github.com/user-attachments/assets/7606ad5e-b1f5-4637-8689-aa1e64aede17" />

---

### 3.2 Install Dependencies

Install clean testing dependencies using `pip`:

```bash
pip install pytest pytest-cov pytest-mock
```

Check the command exit status:

```bash
echo $?
```

Expected output:

```text
0
```

<img width="1853" height="116" alt="image" src="https://github.com/user-attachments/assets/ae39e158-1573-4a4b-a7cc-c9fd59ad3e8b" />

---

### 3.3 Configure Unit Tests and Mocks

Create test files `tests/test_attendance.py` and `tests/test_notification.py`.

The test suite mocks external notification gateways and validates isolated attendance logic:

```python
# Mock external notification gateway
@pytest.fixture
def mock_gateway(mocker):
    gateway = mocker.MagicMock()
    gateway.send.return_value = True
    return gateway

# Test attendance calculation and validation logic
def test_calculate_work_duration():
    check_in = datetime(2026, 10, 5, 9, 0)
    check_out = datetime(2026, 10, 5, 17, 30)
    assert calculate_work_duration(check_in, check_out) == 8.5
```

The test files implement **5 test cases**:

1. **Duration Calculation:** Verifies exact work hours calculation between check-in and check-out.
2. **Late Check-in Detection:** Verifies that arrivals after 09:30 AM are correctly flagged as late.
3. **Invalid Timestamp Handling:** Verifies that check-out timestamps earlier than check-in raise a `ValueError`.
4. **Mocked Notification Dispatch:** Verifies that alerts are dispatched to the external gateway with proper parameters.
5. **Recipient Validation:** Verifies that malformed email addresses raise an error without triggering the gateway.

<img width="1867" height="863" alt="image" src="https://github.com/user-attachments/assets/1895a490-8fc2-40aa-a34a-4d4773ef83e6" />

---

### 3.4 Execute Unit Tests

Run the test suite in non-interactive CI mode:

```bash
pytest -v
```

Expected output:

```text
============================= test session starts ==============================
platform linux -- Python 3.11.8, pytest-7.4.3, pluggy-1.3.0
rootdir: /workspace/python-services, configfile: pytest.ini
plugins: cov-4.1.0, mock-3.12.0
collected 5 items

tests/test_attendance.py::test_calculate_work_duration PASSED           [ 20%]
tests/test_attendance.py::test_late_arrival_detection PASSED            [ 40%]
tests/test_attendance.py::test_invalid_checkout_raises_error PASSED     [ 60%]
tests/test_notification.py::test_send_alert_success PASSED              [ 80%]
tests/test_notification.py::test_send_alert_invalid_email PASSED        [100%]

============================== 5 passed in 0.14s ===============================
```

All 5 implemented unit tests passed successfully.

<img width="1542" height="449" alt="image" src="https://github.com/user-attachments/assets/3beab124-599f-4c93-bd93-92e106b8879e" />

---

### 3.5 Generate and Analyze Code Coverage

Generate the code coverage report using `pytest-cov`:

```bash
pytest --cov=services --cov-report=term-missing --cov-fail-under=80
```

Coverage results for the tested services:

| File                              |  % Statements  |   % Branches   |  % Functions  |    % Lines    |      Status      |
| --------------------------------- | :------------: | :------------: | :------------: | :------------: | :--------------: |
| **attendance_service.py**   | **100%** | **100%** | **100%** | **100%** | **Passed** |
| **notification_service.py** | **100%** | **100%** | **100%** | **100%** | **Passed** |

The POC achieves **100% code coverage** for both `attendance_service.py` and `notification_service.py`, exercising all statements, branches, and functions.

<img width="1728" height="889" alt="image" src="https://github.com/user-attachments/assets/9769fdf0-c0f4-4739-838e-b19764d58911" />

---

# 4. Conclusion

This Proof of Concept successfully demonstrated Unit Testing for the Python microservices. The test suite validated shift calculations, late check-in detection, timestamp validation, mocked gateway delivery, and input error handling with all 5 tests passing and achieving 100% coverage on target services.

**Chosen Tool:**
**Pytest with Pytest-cov and Pytest-mock** is chosen for our Python CI pipeline because it requires minimal boilerplate, provides fast in-memory execution, allows clean fixture mocking, and includes built-in coverage reporting with failure gates.

---

# 5. Contact Information

| Name           | Email Address                                                         |
| -------------- | --------------------------------------------------------------------- |
| Vikas Badliwal | [vikas.badliwal@mygurukulam.co](mailto:vikas.badliwal@mygurukulam.co) |


---

# 6. References

| Reference                      | Link                                              |
| ------------------------------ | ------------------------------------------------- |
| **Pytest**               | [Pytest](https://docs.pytest.org/)                 |
| **Pytest-cov**           | [Pytest-cov](https://pytest-cov.readthedocs.io/)   |
| **Pytest-mock**          | [Pytest-mock](https://pytest-mock.readthedocs.io/) |
| **Python Documentation** | [Python](https://docs.python.org/3/)               |
| **pip Documentation**    | [pip](https://pip.pypa.io/en/stable/)              |
