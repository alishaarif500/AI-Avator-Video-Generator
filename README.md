# AI-Avator-Video-Generator
Built a Python script that integrates the HeyGen REST API to generate AI avatar videos from text, with request handling, status polling, and automatic download.
# AI Avatar Video Generator (Python + HeyGen API)

A Python command-line tool that turns text into an AI avatar video request using the [HeyGen](https://www.heygen.com/) REST API. You type a sentence, the script sends it to HeyGen, checks the video status until it is ready, and then downloads the finished `.mp4` file.

> **Note:** This project was built by following a public YouTube tutorial on creating AI avatar videos with Python. The code was reconstructed, run and debugged locally as a learning project in REST API integration.

---

## Features

- Takes text input from the terminal
- Sends a video generation request to the HeyGen API (`/v2/video/generate`)
- Handles HTTP errors, API errors and network exceptions
- Polls the video status (`/v1/video_status.get`) every 10 seconds
- Extracts the file name from the returned URL and downloads the video automatically

## Tech Stack

- **Language:** Python 3.12+
- **Library:** [`requests`](https://pypi.org/project/requests/)
- **API:** HeyGen REST API (v2 generate, v1 status)

## Project Structure

```
heyGenProject/
├── heygen_Clone.py    # main script
├── .env               # API key placeholder (not read by the script)
├── .env.example       # template for your own key
├── requirements.txt   # dependencies (requests)
└── README.md
```

## How It Works

1. `main()` asks the user for the text the avatar should speak.
2. `create_video(text_input)` builds the request payload (avatar, voice, 1280×720 size) and sends it to HeyGen. It returns a `video_id` on success.
3. `get_status(video_id)` requests the current status of that video.
4. A loop checks the status every 10 seconds. When the status is `Success`, the script reads the video URL, extracts the file name and saves the video in the current folder.

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Add your HeyGen API key
Open `heygen_Clone.py` and replace the placeholder in **both** places:

- `API_KEY = "YOUR_API_KEY_HERE"` (near the top of the file)
- `"x-api-key": "YOUR_API_KEY_HERE"` (inside `get_status()`)

Get your key from your own HeyGen account. **Never commit a real API key to GitHub.**

### 4. Run
```bash
python heygen_Clone.py
```

Type your text when prompted. When the video is ready, it is downloaded into the same folder.

## Example Output

```
🚀 HeyGen AI Video Generator
========================================
Enter the text you want the AI to speak:

💬 Enter text: Hello, this is my first AI avatar video.

🎬 Creating video with text: 'Hello, this is my first AI avatar video.'
⏳ Please wait...
✅ Video generation started!
📹 Video ID: <video_id>
⏳ Checking video status...
```

## Configuration

| Setting | Where | Description |
|---|---|---|
| `API_KEY` | `heygen_Clone.py`, line 8 | Your HeyGen API key |
| `x-api-key` | `get_status()` | Your HeyGen API key (same value) |
| `"test": True` | `create_video()` | Test mode. Set to `False` for production videos |
| `avatar_id`, `voice_id` | `create_video()` | Avatar and voice used for the video |
| `width`, `height` | `create_video()` | Output video size (1280×720) |

## Troubleshooting

- **`HTTP Error: 402`:** HeyGen rejected the request, which usually points to missing API credits or plan access. Check the billing and usage section of your HeyGen account.
- **`HTTP Error: 401` or an authentication message:** Check that your API key is valid and pasted correctly in both places.
- **`can't open file 'heygen_Clone.py'`:** Run the command from the folder that contains the file.

## Known Limitations

- The status loop has no timeout or failure handling, so it keeps checking if the video never reaches `Success`.
- The API key is placed directly in the code. For real use, load it from an environment variable or a `.env` file instead.
- Video generation requires an active HeyGen account with API credits.

## Future Improvements

- Read the API key from environment variables (`python-dotenv`)
- Add a timeout and a maximum number of status checks
- Let the user choose the avatar, voice and video size
- Add logging in place of `print` statements

## Disclaimer

This is a learning project and is not affiliated with HeyGen. Avatars, voices and generated videos are subject to HeyGen's terms of service.

## Author

<Your Name> · [LinkedIn](https://www.linkedin.com/) · [GitHub](https://github.com/)
