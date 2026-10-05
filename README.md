# Fake News Detection

A small Flask web application that classifies submitted news text as **looking real** or **looking fake**. It uses a trained scikit-learn model and a TF-IDF vectorizer, with NLTK preprocessing.

> This is an educational classifier, not a fact-checking service. Its output is a model prediction and should not be treated as proof that a claim is true or false.

## How it works

When text is submitted, the application removes non-letter characters, lowercases and tokenizes it, removes English stop words, lemmatizes the remaining words, and transforms the result with the saved TF-IDF vectorizer. The saved classifier then returns a label, which the page displays as “Looking Real News” or “Looking Fake News.”

The trained artifacts are `model.pkl` and `tfidf_vectorizer.pkl`. The repository also contains `Dataset/train.csv` and `Dataset/test.csv`; the web app loads the saved artifacts and does not train a model at startup. `Fake_News_Detector.ipynb` is currently empty.

## Requirements

- Python 3.9 or newer
- The packages listed in `requirements.txt`
- NLTK data packages: `punkt`, `stopwords`, and `wordnet` (and `punkt_tab` on NLTK versions that request it)

## Setup and run

From the project root, create and activate a virtual environment, then install the dependencies:

```bash
python -m venv .venv
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# macOS/Linux:
source .venv/bin/activate

python -m pip install -r requirements.txt
```

Download the NLTK resources used by preprocessing:

```bash
python -c "import nltk; nltk.download('punkt'); nltk.download('punkt_tab'); nltk.download('stopwords'); nltk.download('wordnet')"
```

Start the Flask development server from the project root:

```bash
python app.py
```

Open <http://127.0.0.1:5000>, choose **Prediction**, enter a news headline, and submit it. The model and vectorizer are loaded from the project root when the app starts, so keep both `.pkl` files there. `app.py` enables Flask debug mode; use a production WSGI server and turn debug mode off before any deployment.

## Project layout

```text
.
├── app.py                       # Flask routes and prediction preprocessing
├── model.pkl                    # Trained classifier
├── tfidf_vectorizer.pkl         # Fitted text vectorizer
├── requirements.txt             # Python dependencies
├── Dataset/
│   ├── train.csv                # Training data
│   └── test.csv                 # Test data
├── templates/
│   ├── index.html               # Landing page
│   └── prediction.html          # Text input and result page
└── static/                      # Images and icons used by the pages
```

## Notes

- The prediction form currently accepts a single-line headline (`<input type="text">`), although the model function can process longer text.
- The project does not include documented training or evaluation instructions, model metrics, or a non-empty notebook; the dataset and saved artifacts are provided as-is.
- Only load the pickle files included with or produced by a trusted source. Python pickle files can execute code when loaded.
