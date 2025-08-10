# YouTube-Video-Summariser

Hey there! I built this project to make it super easy to summarize YouTube videos by extracting their transcripts and generating concise summaries using advanced NLP techniques. It’s a Streamlit web app that leverages the YouTube Transcript API to fetch video transcripts and the BART-large-CNN model to create abstractive summaries, all while handling long transcripts with a custom chunking algorithm. Whether you're a student, researcher, or just someone who wants the key points of a video without watching the whole thing, this tool’s got you covered!

## Features





Transcript Extraction: Pulls full transcripts from YouTube videos using the YouTube Transcript API, supporting multiple languages where available.



Abstractive Summarization: Generates 30–100-word summaries for 500-word transcript chunks using the BART-large-CNN model, achieving up to 94% text compression.



Custom Chunking Algorithm: Splits long transcripts into manageable 500-word segments to stay within BART’s input limits, ensuring efficient processing.



Interactive UI: Built with Streamlit, featuring expandable sections to display original transcript chunks alongside their summaries for easy reading.



Error Handling: Gracefully handles invalid URLs or missing transcripts with clear user feedback.

## Installation

To run this project locally, follow these steps. It’s tested on Python 3.11 in a Google Colab environment, but any Python 3.6+ setup should work.





## Clone the Repository (once you push it to GitHub):

git clone https://github.com/your-username/nlp-youtube-summarizer.git
cd nlp-youtube-summarizer



## Install Dependencies:

pip install streamlit youtube-transcript-api pytube sentence-transformers transformers torch scikit-learn numpy pandas nltk



## Download NLTK Data:

import nltk
nltk.download('punkt')



## Run the App:

streamlit run app.py

This will launch the app locally at http://localhost:8501.



## Optional: Deploy with ngrok (for public access, as in the Colab setup):

pip install pyngrok

Then, configure ngrok with your authtoken and run:

from pyngrok import ngrok
import subprocess
import threading
import time

ngrok.kill()
def run_streamlit():
    subprocess.call(["streamlit", "run", "app.py", "--server.port=8503", "--server.headless=true", "--server.enableCORS=false"])
thread = threading.Thread(target=run_streamlit)
thread.start()
time.sleep(5)
public_url = ngrok.connect(8503)
print(f"Open your app here: {public_url}")

Usage





## Open the app in your browser (local or via ngrok).



Enter a YouTube video URL (e.g., https://www.youtube.com/watch?v=dQw4w9WgXcQ).



Click Generate Summary.



The app will:





Fetch the video’s transcript using the YouTube Transcript API.



Split it into 500-word chunks with a custom algorithm.



Summarize each chunk using BART-large-CNN.



Display each chunk and its summary in expandable sections.

Example Output:





For a 10-minute TED Talk (~2,000 words), the app generates ~4 summaries (30–100 words each), compressing the content by up to 94% for quick insights.

How It Works





Video ID Extraction: Parses YouTube URLs to extract the video ID (e.g., dQw4w9WgXcQ from https://www.youtube.com/watch?v=dQw4w9WgXcQ).



Transcript Fetching: Uses youtube-transcript-api to retrieve the full transcript as a single string.



Chunking: A custom algorithm splits the transcript into 500-word chunks to fit BART’s ~1024-token input limit.



Summarization: The facebook/bart-large-cnn model, fine-tuned on the CNN/Daily Mail dataset, generates abstractive summaries for each chunk.



UI: Streamlit renders an interactive interface with expandable sections for each chunk and summary.

Limitations





Transcript Availability: Only works for videos with available transcripts (manual or auto-generated). Fails gracefully for unavailable transcripts.



Processing Speed: BART on CPU takes ~1–2 seconds per chunk. GPU support or caching could speed this up.



Summary Quality: BART-large-CNN is optimized for news articles, so conversational YouTube transcripts (e.g., vlogs) may have slightly less accurate summaries.



Age-Restricted Videos: Currently unsupported due to YouTube API limitations.

## Future Improvements

I’m planning to make this project even better! Here are some ideas:





Add sentence-based chunking using NLTK to preserve semantic context.



Evaluate summaries with ROUGE or BERTScore to quantify quality (targeting ROUGE-1 ~40, based on BART’s CNN/Daily Mail performance).



Cache transcripts and summaries to reduce processing time.



Deploy on Streamlit Cloud for a live demo.



Integrate pytube to extract video metadata (e.g., title, description) as a fallback for videos without transcripts.
