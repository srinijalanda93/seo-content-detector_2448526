

<h1> SEO Content Analysis Pipeline</h1>

<h2> Overview</h2>
<p>
  <strong>SEO Content Detector</strong> is an end-to-end pipeline that extracts, analyzes, and evaluates webpage content quality for SEO performance.  
  It automates text extraction, duplicate detection, and machine-learning-based quality scoring.
</p>

<h2> Why This Project</h2>
<p>
  Search engines prioritize <strong>unique, high-quality content</strong>.  
  This system helps website administrators and SEO analysts:
</p>
<ul>
  <li>Detect duplicate or thin pages hurting SEO rankings</li>
  <li>Score overall content quality using machine learning</li>
  <li>Provide real-time content analysis for any given webpage</li>
</ul>

<hr>

<h2> Step-by-Step Pipeline</h2>

<h3>Step 1: Data Extraction</h3>
<ul>
  <li>Input: <code>data/data.csv</code> containing <code>url</code> and <code>html_content</code></li>
  <li>Parsed using <code>HTMLParser</code> (no BeautifulSoup)</li>
  <li>Extracted title, body text, and word count</li>
  <li>Output → <code>data/extracted_content.csv</code></li>
</ul>

<h3>Step 2: Feature Extraction</h3>
<ul>
  <li>Clean text: lowercased, whitespace normalized</li>
  <li>Features computed:</li>
  <ul>
    <li>Word count</li>
    <li>Sentence count</li>
    <li>Flesch Reading Ease score</li>
    <li>Top 5 TF-IDF keywords</li>
    <li>Text embeddings</li>
  </ul>
  <li>Output → <code>data/features_with_thin_flag.csv</code></li>
</ul>

<h3>Step 3: Duplicate & Thin Content Detection</h3>
<ul>
  <li>Cosine similarity on TF-IDF vectors</li>
  <li>Threshold: <code>> 0.80</code> → mark as duplicate</li>
  <li>Thin pages: <code>word_count < 500</code></li>
  <li>Outputs:</li>
  <ul>
    <li><code>data/duplicates.csv</code></li>
    <li><code>is_thin</code> column in features CSV</li>
  </ul>
</ul>

<h3>Step 4: Content Quality Scoring</h3>
<ul>
  <li>Labels generated using rules:</li>
  <ul>
    <li><strong>High:</strong> word_count &gt; 1500 and 50 ≤ readability ≤ 70</li>
    <li><strong>Low:</strong> word_count &lt; 500 or readability &lt; 30</li>
    <li><strong>Medium:</strong> all other cases</li>
  </ul>
  <li>Trained <code>RandomForestClassifier</code> on:</li>
  <ul>
    <li>word_count</li>
    <li>sentence_count</li>
    <li>flesch_reading_ease</li>
  </ul>
  <li>Split: 70/30 (train/test)</li>
  <li>Model saved to <code>models/quality_model.pkl</code></li>
</ul>

<div class="highlight">
  <h4> Classification Report:</h4>
  <pre>
              precision    recall  f1-score   support
  Low             1.00      1.00      1.00        21

  accuracy                            1.00        21
  macro avg        1.00      1.00      1.00        21
  weighted avg     1.00      1.00      1.00        21

  <strong>Overall Accuracy: 1.0</strong>
  </pre>
</div>

<h3>Step 5: Real-Time URL Analysis</h3>
<ul>
  <li>Function: <code>analyze_url(url)</code></li>
  <li>Fetches and parses live page HTML</li>
  <li>Extracts features and predicts quality using saved model</li>
  <li>Matches duplicates (if any) from <code>duplicates.csv</code></li>
</ul>

<pre><code>result = analyze_url("https://example.com/test-article")</code></pre>
<h4>Example Output:</h4>
<pre>
{
  "url": "https://example.com/test-article",
  "word_count": 1450,
  "readability": 65.2,
  "quality_label": "High",
  "is_thin": false,
  
}
</pre>

<hr>

<h2>📂 Folder Structure</h2>
<pre>
project_root/
│
├── data/
│   ├── data.csv
│   ├── extracted_content.csv
│   ├── features_with_thin_flag.csv
│   └── duplicates.csv
│
├── models/
│   └── quality_model.pkl
│
├── notebooks/
│   └── SEO_Pipeline_Notebook.ipynb
│
└── README.html
</pre>

<h2> Key Libraries</h2>
<ul>
  <li><code>pandas</code>, <code>numpy</code></li>
  <li><code>nltk</code> (tokenization)</li>
  <li><code>textstat</code> (readability)</li>
  <li><code>sklearn</code> (TF-IDF, cosine similarity, RandomForest)</li>
  <li><code>requests</code>, <code>BeautifulSoup</code> (for live scraping)</li>
</ul>

<h2> Results Summary</h2>
<ul>
  <li>Total pages analyzed: 60</li>
  <li>Duplicate pairs detected: 3</li>
  <li>Thin content pages: 6 (10%)</li>
  <li>Classifier Accuracy: 100%</li>
</ul>

<hr>

<h2> Author</h2>
<p><strong>Srinija Landa</strong><br>
Core Committee – Xebit 2025<br>
Project: <em>SEO Content Detection and Quality Scoring Pipeline</em></p>


