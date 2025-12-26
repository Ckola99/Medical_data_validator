# Medical Records Validator

## Overview

This project implements a **data validation utility** for medical records represented as Python dictionaries. It verifies both the **structure** and **content** of each medical record and reports detailed validation errors.

The validator ensures that:
- The input is a list or tuple of records
- Each record is a dictionary with the correct keys
- Each field in a record conforms to predefined format and type constraints

---

## Features

- ✅ Validates data structure (list/tuple of dictionaries)
- ✅ Ensures all required fields are present
- ✅ Validates field types and formats
- ✅ Provides detailed error messages with record position
- ✅ Pattern matching for ID fields using regular expressions
- ✅ Case-insensitive validation for specific fields

---

## Data Format

Each medical record must be a dictionary with **exactly** the following keys:

```python
{
    'patient_id': str,
    'age': int,
    'gender': str,
    'diagnosis': str | None,
    'medications': list[str],
    'last_visit_id': str
}
```

### Example (Valid Record)

```python
{
    'patient_id': 'P1001',
    'age': 34,
    'gender': 'Female',
    'diagnosis': 'Hypertension',
    'medications': ['Lisinopril'],
    'last_visit_id': 'V2301',
}
```

---

## Validation Rules

### Field-Level Constraints

| Field | Type | Rule |
|-------|------|------|
| `patient_id` | `str` | Must start with `p` or `P` followed by digits (pattern: `p\d+`) |
| `age` | `int` | Must be an integer ≥ 18 |
| `gender` | `str` | Must be `male` or `female` (case-insensitive) |
| `diagnosis` | `str` or `None` | Can be a string or `None` |
| `medications` | `list[str]` | Must be a list containing only strings |
| `last_visit_id` | `str` | Must start with `v` or `V` followed by digits (pattern: `v\d+`) |

> **Note:** Regular expressions are validated using `re.fullmatch` with `re.IGNORECASE` flag.

---

## Functions

### `find_invalid_records(...)`

```python
def find_invalid_records(
    patient_id, age, gender, diagnosis, medications, last_visit_id
)
```

#### Purpose
Validates the values of a single medical record.

#### Returns
A list of field names whose values are invalid.

#### Example Return Value
```python
['age', 'last_visit_id']
```

#### How It Works
1. Builds a dictionary mapping field names to boolean validation results
2. Returns all keys whose validation result is `False`

---

### `validate(data)`

```python
def validate(data):
```

#### Purpose
Validates a collection of medical records and prints detailed error messages.

#### Input
- `data`: A list or tuple of dictionaries

#### Behavior
1. Checks that `data` is a list or tuple
2. Iterates over each record and:
   - Verifies it is a dictionary
   - Verifies it contains exactly the required keys
   - Identifies invalid fields using `find_invalid_records`
3. Prints descriptive validation errors for each issue found

#### Example Error Messages
```
Invalid format: expected a dictionary at position 2.
Invalid format: {...} at position 1 has missing and/or invalid keys.
Unexpected format 'age: 15' at position 0.
Unexpected format 'last_visit_id: x123' at position 3.
```

---

## Installation

No installation required! This project uses only Python standard library modules.

### Prerequisites
- Python 3.8 or higher

---

## Usage

### Basic Example

```python
import re

# Sample data
medical_records = [
    {
        'patient_id': 'P1001',
        'age': 34,
        'gender': 'Female',
        'diagnosis': 'Hypertension',
        'medications': ['Lisinopril'],
        'last_visit_id': 'V2301',
    },
    {
        'patient_id': 'p1002',
        'age': 47,
        'gender': 'male',
        'diagnosis': 'Type 2 Diabetes',
        'medications': ['Metformin', 'Insulin'],
        'last_visit_id': 'v2302',
    },
]

# Validate the records
validate(medical_records)
```

### Testing with Invalid Data

Modify `medical_records` to include invalid entries to see validation output:

```python
invalid_records = [
    {
        'patient_id': 'X123',  # Invalid: doesn't start with P
        'age': 15,              # Invalid: under 18
        'gender': 'Female',
        'diagnosis': 'Asthma',
        'medications': ['Albuterol'],
        'last_visit_id': 'V2301',
    },
]

validate(invalid_records)
```

**Output:**
```
Unexpected format 'patient_id: X123' at position 0.
Unexpected format 'age: 15' at position 0.
```

---

## Code Structure

```
.
├── README.md
└── medical_records_validator.py
```

### Main Components

1. **Data Definition**: Sample medical records for testing
2. **`find_invalid_records()`**: Field-level validation logic
3. **`validate()`**: Collection-level validation and error reporting

---

## Design Notes

- ✨ Uses **set equality** to detect missing or extra keys
- ⚡ Uses **short-circuit boolean logic** for efficient validation
- 🔍 Separates **structure validation** from **value validation**
- 📍 Error messages include **record index** for easier debugging
- 🎯 Minimalist design with no external dependencies

---

## Dependencies

- **Python 3.8+**
- **Standard library only:**
  - `re` (regular expressions)

No third-party packages are required.

---

## Error Handling

The validator handles three types of errors:

1. **Type Errors**: When `data` is not a list or tuple
2. **Structure Errors**: When records are not dictionaries or have incorrect keys
3. **Value Errors**: When field values don't meet validation constraints

All errors are printed with descriptive messages and position information.

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Suggested Improvements
- Add unit tests
- Return validation results instead of printing
- Add support for custom validation rules
- Export validation reports to JSON/CSV

---

## License

This project is provided for educational purposes. No license restrictions are applied.

---

## Author

Created as a Python data validation utility example.

---

## Changelog

### Version 1.0.0
- Initial release
- Basic validation for medical records
- Field-level constraint checking
- Detailed error reporting

---

## Support

If you encounter any issues or have questions, please open an issue on the project repository.
