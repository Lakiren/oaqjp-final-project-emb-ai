"""
Server module for the Emotion Detection application.
Provides an API endpoint to analyze emotions in text using Watson NLP.
"""

from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask(__name__)


@app.route("/emotionDetector")
def emot_detector():
    """
    Endpoint to process the text provided by the user and return
    the scores of various emotions along with the dominant emotion.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again."

    # Construimos la respuesta en partes cortas para cumplir con PyLint
    part1 = f"For the given statement, the system response is 'anger': {anger}, "
    part2 = f"'disgust': {disgust}, 'fear': {fear}, 'joy': {joy} "
    part3 = f"and 'sadness': {sadness}. The dominant emotion is {dominant_emotion}."

    return f"{part1}{part2}{part3}"


@app.route("/")
def render_index_page():
    """
    Renders the main index page of the application interface.
    """
    return render_template('index.html')


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)

