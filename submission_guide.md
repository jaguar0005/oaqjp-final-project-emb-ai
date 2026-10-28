# Final Project: Emotion Detector — Submission Answers

## Task 1: Submit the GitHub repository URL
**URL:**
```text
https://github.com/jaguar0005/oaqjp-final-project-emb-ai/blob/main/README.md
```

---

## Task 2: Create an emotion detection application using the Watson NLP library

### Activity 1: Submit the code from the emotion_detection.py file that shows the application function.
```python
import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    return response.text
```

### Activity 2: Submit the terminal output showing that the application was imported and tested without errors.
```text
>>> from emotion_detection import emotion_detector
>>> emotion_detector("I love this new technology.")
'{"emotionPredictions": [{"emotion": {"anger": 0.003185995, "disgust": 0.001777271, "fear": 0.008323412, "joy": 0.96865213, "sadness": 0.04123562}, "target": "", "emotionMentions": [{"span": {"begin": 0, "end": 26, "text": "I love this new technology."}, "emotion": {"anger": 0.003185995, "disgust": 0.001777271, "fear": 0.008323412, "joy": 0.96865213, "sadness": 0.04123562}}]}]}'
```

---

## Task 3: Format the output of the application

### Activity 1: Submit the code from the emotion_detection.py file showing that the modified emotion_detector function returns the correct output format.
```python
import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    formatted_response = json.loads(response.text)
    
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    dominant_emotion = max(emotions, key=emotions.get)
    
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
```

### Activity 2: Submit the terminal output showing that the output format is accurate.
```text
>>> from emotion_detection import emotion_detector
>>> emotion_detector("I love this new technology.")
{'anger': 0.003185995, 'disgust': 0.001777271, 'fear': 0.008323412, 'joy': 0.96865213, 'sadness': 0.04123562, 'dominant_emotion': 'joy'}
```

---

## Task 4: Validate the EmotionDetection package

### Activity 1: Submit the public GitHub repository URL of the __init__.py file which includes the code to import the application module.
**URL:**
```text
https://github.com/jaguar0005/oaqjp-final-project-emb-ai/blob/main/EmotionDetection/__init__.py
```

### Activity 2: Submit the terminal output validating that EmotionDetection is a valid package.
```text
>>> from EmotionDetection.emotion_detection import emotion_detector
>>> emotion_detector("I love this new technology.")
{'anger': 0.003185995, 'disgust': 0.001777271, 'fear': 0.008323412, 'joy': 0.96865213, 'sadness': 0.04123562, 'dominant_emotion': 'joy'}
```

---

## Task 5: Run unit tests on your application

### Activity 1: Submit the code from the test_emotion_detection.py file showing the required unit tests.
```python
from EmotionDetection.emotion_detection import emotion_detector
import unittest

class TestEmotionDetector(unittest.TestCase):
    def test_emotion_detector(self):
        result_1 = emotion_detector('I am glad this happened')
        self.assertEqual(result_1['dominant_emotion'], 'joy')

        result_2 = emotion_detector('I am really mad about this')
        self.assertEqual(result_2['dominant_emotion'], 'anger')

        result_3 = emotion_detector('I feel disgusted just hearing about this')
        self.assertEqual(result_3['dominant_emotion'], 'disgust')

        result_4 = emotion_detector('I am so sad about this')
        self.assertEqual(result_4['dominant_emotion'], 'sadness')

        result_5 = emotion_detector('I am really afraid that this will happen')
        self.assertEqual(result_5['dominant_emotion'], 'fear')

if __name__ == '__main__':
    unittest.main()
```

### Activity 2: Submit the terminal output showing that all unit tests passed.
```text
python3 test_emotion_detection.py
.
----------------------------------------------------------------------
Ran 1 test in 0.002s

OK
```

---

## Task 6: Web deployment of the application using Flask

### Activity 1: Submit the code from the server.py file showing the web deployment of the application using Flask.
```python
"""
Flask server for Emotion Detection Application.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")


@app.route("/emotionDetector")
def sent_detector():
    """
    Analyze the text provided by the user and return emotion scores
    along with the dominant emotion.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again!"

    return (
        f"For the given statement, the system response is "
        f"'anger': {response['anger']}, "
        f"'disgust': {response['disgust']}, "
        f"'fear': {response['fear']}, "
        f"'joy': {response['joy']} and "
        f"'sadness': {response['sadness']}. "
        f"The dominant emotion is {dominant_emotion}."
    )


@app.route("/")
def render_index_page():
    """
    Render the index.html template page.
    """
    return render_template('index.html')


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

### Activity 2: Upload the screenshot named 6b_deployment_test.png showing the application deployment.
Upload the image file saved on your system at:
`c:\Users\gaurj\OneDrive\Desktop\Courseraa\6b_deployment_test.png`

---

## Task 7: Incorporate error handling

### Activity 1: Submit the code from the emotion_detection.py file showing the updated emotion_detector function for status code 400.
```python
import requests
import json

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    
    if response.status_code == 400:
        return {
            'anger': None,
            'disgust': None,
            'fear': None,
            'joy': None,
            'sadness': None,
            'dominant_emotion': None
        }
    
    formatted_response = json.loads(response.text)
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    dominant_emotion = max(emotions, key=emotions.get)
    
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
```

### Activity 2: Submit the code from the server.py file showing the handling of blank input errors.
```python
@app.route("/emotionDetector")
def sent_detector():
    """
    Analyze the text provided by the user and return emotion scores
    along with the dominant emotion.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again!"

    return (
        f"For the given statement, the system response is "
        f"'anger': {response['anger']}, "
        f"'disgust': {response['disgust']}, "
        f"'fear': {response['fear']}, "
        f"'joy': {response['joy']} and "
        f"'sadness': {response['sadness']}. "
        f"The dominant emotion is {dominant_emotion}."
    )
```

### Activity 3: Upload the screenshot named 7c_error_handling_interface.png validating error handling functionality.
Upload the image file saved on your system at:
`c:\Users\gaurj\OneDrive\Desktop\Courseraa\7c_error_handling_interface.png`

---

## Task 8: Run static code analysis

### Activity 1: Submit the code from the server.py file demonstrating the execution of static code analysis.
```python
"""
Flask server for Emotion Detection Application.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")


@app.route("/emotionDetector")
def sent_detector():
    """
    Analyze the text provided by the user and return emotion scores
    along with the dominant emotion.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again!"

    return (
        f"For the given statement, the system response is "
        f"'anger': {response['anger']}, "
        f"'disgust': {response['disgust']}, "
        f"'fear': {response['fear']}, "
        f"'joy': {response['joy']} and "
        f"'sadness': {response['sadness']}. "
        f"The dominant emotion is {dominant_emotion}."
    )


@app.route("/")
def render_index_page():
    """
    Render the index.html template page.
    """
    return render_template('index.html')


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

### Activity 2: Submit the terminal output showing a perfect score for static code analysis.
```text
pylint server.py

------------------------------------
Your code has been rated at 10.00/10
```
