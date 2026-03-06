### Pull Request Description

**Enhancement**: Implement LOW priority detection in `classify_priority()` method.

**Summary**:
This PR addresses the issue of adding a LOW priority to the `classify_priority()` function. A new list, `LOW_KEYWORDS`, is introduced to identify issues that should be considered of LOW priority. Additionally, a test case has been added to ensure that the classifier correctly identifies and returns a 'LOW' classification when applicable keywords are present in the input.

**Changes**:
1. **`src/ai_classifier_simple.py`**:
   - Added the `LOW_KEYWORDS` list containing example keywords: "minor", "small", "cosmetic", "slightly", "paint", "scratch".
   - Updated the `classify_priority()` function to check for the presence of keywords from `LOW_KEYWORDS` and return 'LOW' priority if any keyword is found.

2. **`tests/test_classifier.py`**:
   - Added a test function to verify that the `classify_priority()` function correctly identifies LOW priority issues using the newly introduced keywords.

```python
# src/ai_classifier_simple.py

URGENT_KEYWORDS = ['urgent', 'immediate', 'asap', 'crash', 'failure']
HIGH_KEYWORDS = ['high', 'important', 'necessary', 'major', 'critical']
MEDIUM_KEYWORDS = ['medium', 'regular', 'normal', 'average']
LOW_KEYWORDS = ['minor', 'small', 'cosmetic', 'slightly', 'paint', 'scratch']

def classify_priority(text):
    text = text.lower()
    
    if any(keyword in text for keyword in URGENT_KEYWORDS):
        return 'URGENT'
    if any(keyword in text for keyword in HIGH_KEYWORDS):
        return 'HIGH'
    if any(keyword in text for keyword in MEDIUM_KEYWORDS):
        return 'MEDIUM'
    if any(keyword in text for keyword in LOW_KEYWORDS):
        return 'LOW'
    
    return 'UNKNOWN'
```

```python
# tests/test_classifier.py

import unittest
from src.ai_classifier_simple import classify_priority

class TestPriorityClassifier(unittest.TestCase):
    
    def test_urgent_priority(self):
        text = "This is an urgent issue that needs an immediate fix."
        self.assertEqual(classify_priority(text), 'URGENT')
    
    def test_high_priority(self):
        text = "This is an important task that should be done."
        self.assertEqual(classify_priority(text), 'HIGH')
    
    def test_medium_priority(self):
        text = "A regular update is required."
        self.assertEqual(classify_priority(text), 'MEDIUM')
    
    def test_low_priority(self):
        text = "This is a minor cosmetic change."
        self.assertEqual(classify_priority(text), 'LOW')
    
    def test_unknown_priority(self):
        text = "This is a random text with no specific keywords."
        self.assertEqual(classify_priority(text), 'UNKNOWN')

if __name__ == '__main__':
    unittest.main()
```

**Explanation**:
- **`classify_priority()`**: By adding a `LOW_KEYWORDS` list, the function can now handle LOW priority classification effectively. Each priority level checks if its associated keywords appear in the input text. Priority checks proceed in descending order of importance to correctly evaluate the most critical priority first.
- **Tests**: Test cases are added to cover typical scenarios for the four priorities, ensuring that the changes work as expected in various contexts.

This enhancement provides a comprehensive solution to the issue, ensuring that the classifier can now detect and categorize LOW priority issues based on specific keywords.